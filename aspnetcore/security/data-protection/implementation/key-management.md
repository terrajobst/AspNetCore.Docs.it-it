---
title: Gestione delle chiavi
author: rick-anderson
description: Questo documento descrive i dettagli di implementazione di ASP.NET Core dati protezione della gestione delle chiavi API.
keywords: ASP.NET Core, protezione dei dati, gestione delle chiavi
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fb9b807a-d143-4861-9ddb-005d8796afa3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: d9e38fd5c8de2b10ad24fe557aa6e3063e40236e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="key-management"></a>Gestione delle chiavi

<a name="data-protection-implementation-key-management"></a>

Il sistema di protezione dati gestisce automaticamente la durata delle chiavi master utilizzata per proteggere e annullare la protezione di payload. Ogni chiave può essere presente in una delle quattro fasi:

* Create - la chiave esiste nell'anello chiave, ma non è ancora stata attivata. La chiave non deve essere usata per le nuove operazioni di proteggere fino a quando non è trascorso tempo sufficiente che la chiave abbia avuto la possibilità di propagare a tutti i computer che utilizzano la gestione delle chiavi.

* Attivo: la chiave esiste nell'anello chiave e deve essere utilizzato per tutte le nuove operazioni di protezione.

* La chiave scaduta - è stata eseguita la relativa durata naturale e non deve più essere usata per le nuove operazioni di protezione.

* Revocato - la chiave viene compromessa e non deve essere utilizzata per nuove operazioni di protezione.

Chiavi create, attive e scadute possono tutti essere utilizzate per rimuovere la protezione di payload in ingresso. Revocati chiavi per impostazione predefinita, non possono essere utilizzate per rimuovere la protezione di payload, ma lo sviluppatore di applicazioni può [eseguire l'override di questo comportamento](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) se necessario.

>[!WARNING]
> Lo sviluppatore potrebbe essere tentato di eliminare una chiave dall'anello chiave (ad esempio, eliminando il file corrispondente dal file system). A questo punto, tutti i dati protetti dalla chiave è definitivamente decifrabili e vi sono sostituzioni emergenza come con chiavi revocate. Eliminazione di una chiave è realmente distruttivo comportamento e di conseguenza il sistema di protezione dati non espone alcuna API di prima classe per eseguire questa operazione.

## <a name="default-key-selection"></a>Selezione chiave predefinita

Quando il sistema di protezione dati legge la gestione delle chiavi dal repository sottostante, verrà effettuato un tentativo di individuare una chiave "default" dalla chiave di sequenza. La chiave predefinita viene utilizzata per nuove operazioni di protezione.

L'euristica generale è che il sistema di protezione dati viene scelta la chiave con la data più recente di attivazione come chiave predefinita. (È disponibile una tecnica per indurre piccoli per consentire il server a server clock inclinazione). Se la chiave è scaduta o revocata, generazione di chiavi e se l'applicazione non ha disabilitato automatica, quindi verrà generata una nuova chiave con l'attivazione immediata per il [in sequenza e scadenza della chiave](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) criteri riportato di seguito.

Il motivo per il sistema di protezione di dati genera una nuova chiave immediatamente, anziché eseguire il fallback su un'altra chiave è che la nuova generazione di chiavi deve essere considerata come una scadenza implicita di tutte le chiavi che sono stati attivati prima la nuova chiave. L'idea generale è che le nuove chiavi potrebbero essere state configurate con algoritmi diversi o meccanismi di crittografia a riposo rispetto a quelle precedenti e il sistema deve utilizzare la configurazione corrente su cui eseguire il fallback.

Si verifica un'eccezione. Se lo sviluppatore di applicazioni ha [disabilitata la generazione automatica delle chiave](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), quindi il sistema di protezione dati è necessario scegliere un elemento come chiave predefinita. In questo scenario di fallback, il sistema sceglierà la chiave non è stato revocato con la data più recente di attivazione, usando preferibilmente delle chiavi che hanno avuto tempo per propagare ad altri computer nel cluster. Il sistema di fallback può finire la scelta di una chiave predefinita scaduta di conseguenza. Il sistema di fallback non sceglierà mai una chiave revocata come chiave predefinita e se la gestione delle chiavi è vuoto o se ogni chiave è stato revocato il sistema verrà generato un errore al momento dell'inizializzazione.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Scadenza della chiave e in sequenza

Quando viene creata una chiave, viene automaticamente assegnato una data di attivazione di {now + 2 giorni} e una data di scadenza di {now + 90 giorni}. Il ritardo di 2 giorni prima dell'attivazione offre il tempo chiave per propagare nel sistema. Che consente di altre applicazioni verso l'archivio di backup osservare la chiave al loro successivo periodo di aggiornamento automatico, pertanto consente di massimizzare le probabilità che la chiave dell'anello fa diventano attive sono state propagate a tutte le applicazioni che potrebbe essere necessario usarlo.

Se la chiave predefinita scadrà entro 2 giorni e la gestione delle chiavi non dispone già di una chiave che sarà attiva dopo la scadenza della chiave predefinita, il sistema di protezione dati mantiene automaticamente una nuova chiave per la gestione delle chiavi. La nuova chiave ha una data di attivazione di {data di scadenza della chiave predefinita} e una data di scadenza di {now + 90 giorni}. Questo consente al sistema di eseguire automaticamente il rollback delle chiavi a intervalli regolari senza interruzione del servizio.

Potrebbe verificarsi casi in cui verrà creata una chiave con attivazione immediato. Un esempio potrebbe essere quando l'applicazione non è stata eseguita per un periodo di tempo e tutte le chiavi dell'anello di chiave sono scadute. In questo caso, la chiave ha una data di attivazione di {ora} senza il ritardo di attivazione di 2 giorni normali.

Durata della chiave predefinita è 90 giorni, anche se questa opzione è configurabile come nell'esempio seguente.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Un amministratore può inoltre modificare il valore predefinito a livello di sistema, anche se una chiamata esplicita a `SetDefaultKeyLifetime` eseguirà l'override di tutti i criteri a livello di sistema. Durata della chiave predefinita non può essere inferiore a 7 giorni.

## <a name="automatic-key-ring-refresh"></a>Aggiornamento automatico delle chiavi

Quando si inizializza il sistema di protezione dati, legge la gestione delle chiavi dal repository sottostante e lo memorizza nella cache in memoria. Questa cache consente operazioni di protezione e Unprotect continuare senza raggiungere l'archivio di backup. Il sistema controlla automaticamente l'archivio di backup per le modifiche circa ogni 24 ore o quando scade la chiave predefinita corrente, verifica per primo.

>[!WARNING]
> Gli sviluppatori devono raramente (se applicabile) è necessario utilizzare direttamente le API di gestione principali. Il sistema di protezione dati eseguirà gestione automatica delle chiavi, come descritto in precedenza.

Il sistema di protezione dati espone un'interfaccia `IKeyManager` che può essere utilizzato per controllare e apportare modifiche per la gestione delle chiavi. Il sistema DI cui l'istanza di `IDataProtectionProvider` può anche fornire un'istanza di `IKeyManager` per il consumo. In alternativa, è possibile effettuare il pull di `IKeyManager` direttamente dal `IServiceProvider` come nell'esempio seguente.

Qualsiasi operazione che modifica la gestione delle chiavi (creazione di una nuova chiave in modo esplicito o l'esecuzione di una revoca) invalida la cache in memoria. La chiamata successiva a `Protect` o `Unprotect` il sistema di protezione dati esegua la rilettura di gestione delle chiavi e la ricreazione della cache.

Nell'esempio seguente viene illustrato l'utilizzo di `IKeyManager` interfaccia per esaminare e manipolare l'anello chiave, tra cui revocare le chiavi esistenti e generare manualmente una nuova chiave.

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Archiviazione delle chiavi

Il sistema di protezione dati è un approccio euristico in base al quale si tenta di dedurre un percorso di archiviazione chiavi appropriata e la crittografia a meccanismo rest automaticamente. È anche configurabile dallo sviluppatore di app. I documenti seguenti illustrano le implementazioni nella casella di questi meccanismi:

* [Nella casella provider di archiviazione chiavi](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Crittografia della chiave nella casella provider di rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
