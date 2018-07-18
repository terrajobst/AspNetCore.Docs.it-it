---
title: Protezione dei dati di ASP.NET Core
author: rick-anderson
description: Scopri il concetto di protezione dei dati e i principi di progettazione di ASP.NET Core Data Protection API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: 29a2bbef6f2fd9b61541173af143926ca82bfad7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095695"
---
# <a name="aspnet-core-data-protection"></a>Protezione dei dati di ASP.NET Core

Le applicazioni Web è spesso necessario archiviare i dati sensibili alla sicurezza. Windows fornisce DPAPI per le applicazioni desktop, ma ciò non è adatta per le applicazioni web. Lo stack di protezione dei dati di ASP.NET Core forniscono un'API di crittografia semplice e facile da usare uno sviluppatore può utilizzare per proteggere i dati, tra cui la rotazione e la gestione delle chiavi.

Lo stack di protezione dei dati di ASP.NET Core è progettato per essere utilizzato come valore di sostituzione a lungo termine per il &lt;machineKey&gt; elemento in ASP.NET 1.x - 4.x. È stato progettato per soddisfare molte delle carenze del vecchio stack di crittografia, offrendo una soluzione di out-of-the-box per la maggior parte dei casi d'uso di applicazioni moderne possono incontrare.

## <a name="problem-statement"></a>Presentazione del problema

L'istruzione di problema generale può essere dichiarata in una singola frase sinteticamente: è necessario rendere persistenti le informazioni attendibili per un successivo recupero, ma il meccanismo di persistenza non attendibili. In termini di web, questo potrebbe essere scritto come "Ho bisogno di andata e ritorno dello stato attendibile tramite un client non attendibile".

L'esempio canonico è un cookie di autenticazione o bearer token. Il server genera un "Sono Groot e disporre delle autorizzazioni xyz" del token e lo passa al client. Successivamente il client presenta tale token al server, ma il server deve avere un tipo di garanzia che il client non è ancora contraffatto il token. In questo modo il primo requisito: autenticità (noto anche come l'integrità, a prova di manomissione).

Poiché lo stato persistente è considerato attendibile dal server, si prevede che questo stato può contenere informazioni specifiche per l'ambiente operativo. Ciò potrebbe essere in forma di un percorso di file, un'autorizzazione, un handle o altri riferimento indiretto o altre parti di dati specifici del server. Tali informazioni a livello generale non devono essere rivelate a un client non attendibile. In questo modo il secondo requisito: riservatezza.

Infine, poiché vengano suddivisa in componenti di applicazioni moderne, ciò che abbiamo visto è che i singoli componenti desidera sfruttare i vantaggi di questo sistema indipendentemente da altri componenti nel sistema. Ad esempio, se un componente di bearer token Usa lo stack, dovrebbe funzionare senza interferenze da un meccanismo di anti-CSRF che potrebbe anche usare lo stesso stack. In questo modo l'ultimo requisito: isolamento.

Possiamo fornire ulteriormente i vincoli per limitare l'ambito dei requisiti. Si presuppone che tutti i servizi che operano all'interno di sistema di crittografia sono altrettanto attendibili e che i dati non devono essere generato o utilizzato di fuori di servizi sotto il controllo diretto. Inoltre, è necessario che le operazioni sono nel minor tempo poiché ogni richiesta al servizio web può passare attraverso il sistema di crittografia una o più volte. Ciò rende ideale per questo scenario la crittografia simmetrica e crittografia asimmetrica viene possibile discount fino ad una volta che è necessario.

## <a name="design-philosophy"></a>Filosofia di progettazione

Iniziammo identificando i problemi con lo stack di esistente. Una volta che avevamo, abbiamo intervistati ha dichiarato il panorama delle soluzioni esistenti e conclude che nessuna soluzione era piuttosto le funzionalità di cui che viene ricercati. Quindi, abbiamo progettato una soluzione basata su alcuni principi guida.

* Il sistema deve offrire semplicità di configurazione. Idealmente, il sistema sarebbe senza operazioni di configurazione e gli sviluppatori è stato possibile inizia subito. Situazioni in cui gli sviluppatori devono configurare un aspetto specifico (ad esempio l'archivio chiave), debba essere presi in considerazione rendendo semplice tali configurazioni specifiche.

* Offrono una semplice API per consumatori. Le API devono essere facile da utilizzare in modo corretto e difficile da usare in modo non corretto.

* Gli sviluppatori non devono illustrato i principi di gestione delle chiavi. Il sistema deve gestire la durata di chiave per conto dello sviluppatore e selezione dell'algoritmo. In teoria lo sviluppatore non è nemmeno avrà accesso al materiale della chiave non elaborato.

* Le chiavi devono essere protette quando sono inattivi quando possibile. Il sistema deve stabilire un meccanismo di protezione predefinito appropriato e lo applica automaticamente.

Con questi principi mente abbiamo sviluppato una semplice [facile da usare](xref:security/data-protection/using-data-protection) lo stack di protezione dati.

Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservati. Come altre tecnologie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) sono più adatte allo scenario di archiviazione illimitata, e hanno le funzionalità di gestione delle chiavi sicuro o ridotta di conseguenza. Ciò premesso, non c'è niente divieto di uno sviluppatore di usare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati.

## <a name="audience"></a>Destinatari

Il sistema di protezione dati è suddivisa in cinque pacchetti principali. Tre gruppi di destinatari principali; di destinazione di vari aspetti di queste API

1. Il [panoramica delle API Consumer](xref:security/data-protection/consumer-apis/overview) agli sviluppatori di applicazioni e framework di destinazione.

   "Non desidero informazioni sulle modalità di funzionamento dello stack o sulla relativa configurazione. Desidera semplicemente eseguire alcune operazioni in più semplice modo possibili con probabilità elevata usando le API è stata."

2. Il [API di configurazione](xref:security/data-protection/configuration/overview) agli sviluppatori di applicazioni e amministratori di sistema di destinazione.

   "È necessario indicare al sistema di protezione dati che l'ambiente richiede impostazioni o i percorsi non predefiniti."

3. Gli sviluppatori di destinazione le API estendibilità responsabile dell'implementazione di criteri personalizzato. Utilizzo di queste API limitata a situazioni rare ed esperti, gli sviluppatori con riconoscimento della protezione.

   "È necessario sostituire un intero componente all'interno del sistema perché ho requisiti comportamentali davvero unico. Accetto di altre parti usato insolito della superficie dell'API per creare un plug-in che soddisfa i requisiti."

## <a name="package-layout"></a>Layout pacchetto

Lo stack di protezione dati è costituito da cinque pacchetti.

* Microsoft.AspNetCore.DataProtection.Abstractions contiene le interfacce IDataProtectionProvider e oggetto IDataProtector base. Include anche i metodi di estensione utili che possono agevolare l'utilizzo di questi tipi (ad esempio, gli overload di IDataProtector.Protect). Vedere la sezione di interfacce utente per altre informazioni. Se un altro utente è responsabile della creazione di un'istanza nel sistema di protezione dei dati e si stanno semplicemente utilizzando le API, è opportuno riferimento Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection contiene l'implementazione di base del sistema di protezione dati, tra cui le operazioni di crittografia di base, la gestione delle chiavi, configurazione ed estendibilità. Se sei responsabile della creazione di un'istanza nel sistema di protezione dei dati (ad esempio, aggiungerlo a un IServiceCollection) oppure modificare o estendere il comportamento, è opportuno riferimento Microsoft.AspNetCore.DataProtection.

* Microsoft.AspNetCore.DataProtection.Extensions contiene API aggiuntive, quali gli sviluppatori possono risultare utili, ma che non appartengono nel pacchetto di base. Ad esempio, questo pacchetto contiene un'API "creare un'istanza del sistema che punta a una directory di archiviazione delle chiavi specifici senza alcuna impostazione di inserimento delle dipendenze" semplice (ulteriori informazioni). Include anche i metodi di estensione per la limitazione della durata di payload protetti (ulteriori informazioni).

* Microsoft.AspNetCore.DataProtection.SystemWeb può essere installato in un'applicazione 4.x ASP.NET esistente per reindirizzare relativi &lt;machineKey&gt; operazioni per usare invece il nuovo stack di protezione dati. Visualizzare [compatibilità](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) per altre informazioni.

* Microsoft.AspNetCore.Cryptography.KeyDerivation fornisce un'implementazione della password PBKDF2 hashing routine e può essere utilizzato da sistemi che devono gestire in modo sicuro le password degli utenti. Visualizzare [Hash delle password](xref:security/data-protection/consumer-apis/password-hashing) per altre informazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

<xref:host-and-deploy/web-farm>
