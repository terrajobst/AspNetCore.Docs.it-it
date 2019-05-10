---
title: Gestione delle chiavi in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione di protezione dei dati di ASP.NET Core di gestione delle chiavi API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892298"
---
# <a name="key-management-in-aspnet-core"></a>Gestione delle chiavi in ASP.NET Core

<a name="data-protection-implementation-key-management"></a>

Il sistema di protezione dati gestisce automaticamente la durata delle chiavi master utilizzata per proteggere e rimuovere la protezione di payload. Ogni chiave può esistere in uno dei quattro fasi:

* Created: la chiave presente nel gruppo di chiavi, ma non è ancora stata attivata. La chiave non deve essere utilizzata per nuove operazioni di protezione fino a quando non è trascorso tempo sufficiente che la chiave abbia avuto la possibilità di propagare a tutti i computer che utilizzano questo gruppo di chiavi.

* Attivo - la chiave esiste nel gruppo di chiavi e deve essere utilizzato per tutte le nuove operazioni di protezione.

* La chiave scaduta - è stata eseguita la relativa durata naturale e non deve più essere utilizzata per le nuove operazioni di protezione.

* Revocato - la chiave viene compromessa e non deve essere usata per le nuove operazioni di protezione.

Chiavi create, attive e scadute tutti consente di rimuovere la protezione di payload in ingresso. Revocati chiavi per impostazione predefinita non possono essere usate per rimuovere la protezione di payload, ma lo sviluppatore dell'applicazione possa [ignorare tale comportamento](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) se necessario.

>[!WARNING]
> Lo sviluppatore potrebbe essere tentato di eliminare una chiave dal gruppo di chiavi (ad esempio, eliminando il file corrispondente dal file system). A questo punto, tutti i dati protetti dalla chiave è definitivamente decifrabili e non vi è alcun override emergenza si è verificato con chiavi revocate. L'eliminazione di una chiave è realmente distruttivo comportamento e, di conseguenza, il sistema di protezione dati non espone alcuna API di prima classe per eseguire questa operazione.

## <a name="default-key-selection"></a>Selezione chiave predefinita

Quando il sistema di protezione dati legge il Keyring dal repository di backup, tenterà di individuare una chiave "default" dal gruppo di chiavi. La chiave predefinita viene usata per le nuove operazioni di protezione.

L'euristica generale è che il sistema di protezione dati viene scelta la chiave con la data più recente di attivazione come chiave predefinita. (Si è un fattore di fudge piccole destinato dello sfasamento dell'orologio server-to-server.) Se la chiave è scaduta o revocata, e se l'applicazione non ha disabilitato automatica della chiave di generazione, quindi verrà generata una nuova chiave con l'attivazione immediata per la [scadenza e l'implementazione della chiave](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) criterio riportato di seguito.

Il motivo per il sistema di protezione dei dati genera immediatamente una nuova chiave, anziché eseguire il fallback su un'altra chiave è di nuova generazione delle chiavi deve essere considerata una scadenza implicita di tutte le chiavi che sono stati attivati prima la nuova chiave. L'idea generale è che le nuove chiavi potrebbero essere state configurate con algoritmi diversi o meccanismi di crittografia dei dati inattivi delle chiavi precedenti e il sistema deve preferire eseguire il fallback alla configurazione corrente.

Si verifica un'eccezione. Se lo sviluppatore dell'applicazione ha [generazione della chiave automatico disabilitato](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), quindi il sistema di protezione dati è necessario scegliere un elemento come chiave predefinita. In questo scenario di fallback, il sistema sceglierà la chiave non revocato con la data più recente di attivazione, usando preferibilmente delle chiavi che hanno avuto tempo per propagare ad altri computer nel cluster. Il sistema di fallback può finire la scelta di una chiave scaduta predefinito di conseguenza. Il sistema di fallback non sceglierà mai una chiave revocata come chiave predefinita e se il gruppo di chiavi è vuoto o è stato revocato ogni chiave quindi il sistema genererà un errore in fase di inizializzazione.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Scadenza della chiave e l'implementazione di

Quando viene creata una chiave, gli ha assegnato automaticamente una data di attivazione del {ora + 2 giorni} e una data di scadenza del {ora + 90 giorni}. Il ritardo di 2 giorni prima dell'attivazione fornisce la chiave temporale a propagarsi nel sistema. Vale a dire, consente ad altre applicazioni che punta all'archivio di backup di osservare la chiave nel successivo periodo di aggiornamento automatico, ottimizzando così le probabilità che quando la chiave di ring fa diventare attivo sia stata propagata a tutte le applicazioni che potrebbe essere necessario usano.

Se la chiave predefinita scade entro 2 giorni e il gruppo di chiavi non la contiene già una chiave che sarà attiva allo scadere della chiave predefinita, il sistema di protezione dati mantiene automaticamente una nuova chiave per il gruppo di chiavi. Questa nuova chiave ha una data di attivazione della {data di scadenza della chiave predefinita} e una data di scadenza del {ora + 90 giorni}. Ciò consente al sistema di aggiornare automaticamente le chiavi regolarmente senza interruzione del servizio.

Potrebbero esserci casi in cui verrà creata una chiave con l'attivazione immediata. Un esempio sarebbe quando l'applicazione non è stata eseguita per un periodo di tempo e tutte le chiavi presenti il gruppo di chiavi sono scadute. In questo caso, la chiave ha una data di attivazione del {ora} senza il ritardo di attivazione di 2 giorni normali.

Durata della chiave predefinita è 90 giorni, anche se questa opzione è configurabile come nell'esempio seguente.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Un amministratore può anche modificare il valore predefinito a livello di sistema, anche se una chiamata esplicita a `SetDefaultKeyLifetime` eseguirà l'override di qualsiasi criterio a livello di sistema. La durata predefinita di chiave non può essere inferiore a 7 giorni.

## <a name="automatic-key-ring-refresh"></a>Aggiornamento automatico Keyring

Quando il sistema di protezione dati viene inizializzato, legge il Keyring dall'archivio sottostante e memorizza nella cache in memoria. Questa cache consente operazioni di protezione e Unprotect procedere senza raggiungere l'archivio di backup. Il sistema controllerà l'archivio di backup per le modifiche automaticamente circa ogni 24 ore oppure quando scade la chiave predefinita corrente, a seconda del valore raggiunto per primo.

>[!WARNING]
> Gli sviluppatori devono molto raramente (se applicabile) necessario usare direttamente la gestione delle chiavi API. Come descritto in precedenza, il sistema di protezione dati eseguirà la gestione delle chiavi automatica.

Il sistema di protezione dati espone un'interfaccia `IKeyManager` che può essere utilizzato per controllare e modificare il gruppo di chiavi. Il sistema di inserimento delle dipendenze che ha fornito l'istanza di `IDataProtectionProvider` può anche fornire un'istanza di `IKeyManager` per la quota di utilizzo. In alternativa, è possibile effettuare il pull il `IKeyManager` direttamente dal `IServiceProvider` come nell'esempio seguente.

Qualsiasi operazione che modifica il gruppo di chiavi (creazione di una nuova chiave in modo esplicito o eseguendo una revoca) invalida la cache in memoria. La chiamata successiva a `Protect` o `Unprotect` causerà il sistema di protezione dati esegua la rilettura il gruppo di chiavi e la ricreazione della cache.

L'esempio seguente viene illustrato l'utilizzo di `IKeyManager` interfaccia di ispezionare e manipolare il gruppo di chiavi, tra cui revoca delle chiavi esistente e generazione manuale di una nuova chiave.

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Archiviazione delle chiavi

Il sistema di protezione dati ha un'euristica in base al quale tenta di dedurre automaticamente un meccanismo di crittografia dei dati inattivi e il percorso di archiviazione delle chiavi appropriata. Il meccanismo di persistenza chiave è anche configurabile dallo sviluppatore dell'app. I documenti seguenti illustrano le implementazioni in arrivo di questi meccanismi:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
