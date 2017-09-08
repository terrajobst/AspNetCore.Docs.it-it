---
title: Introduzione alla protezione dei dati
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4542cd37-b47c-454c-be19-d1b5810d67fe
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/introduction
ms.openlocfilehash: bcf1ce5a272a374c9605e50dee5c5fb27305527d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-data-protection"></a>Introduzione alla protezione dei dati

Le applicazioni Web, spesso è necessario archiviare i dati sensibili alla sicurezza. Windows fornisce DPAPI per le applicazioni desktop, ma questo non è adatto per le applicazioni web. Lo stack di protezione dati di ASP.NET Core forniscono un'API di crittografia semplice e facile da usare uno sviluppatore può utilizzare per proteggere i dati, tra cui la rotazione e la gestione delle chiavi.

Lo stack di protezione dati di ASP.NET Core è progettato per essere utilizzato come valore di sostituzione a lungo termine per il <machineKey> elemento ASP.NET 1. x - 4. x. È stato progettato per affrontare molti degli svantaggi dello stack di crittografia precedente, offrendo una soluzione di casella per la maggior parte dei casi di utilizzo di applicazioni moderne sono potrebbero verificarsi.

## <a name="problem-statement"></a>Presentazione del problema

L'istruzione di problema generale può essere dichiarata in una singola frase sinteticamente: è necessario rendere persistenti le informazioni attendibili per il successivo recupero, ma il meccanismo di persistenza non attendibili. In termini di web, questo potrebbe essere scritto come "Ho bisogno di round trip stato attendibile tramite un client non attendibile".

L'esempio canonico è un cookie di autenticazione o di trasporto token. Il server genera un "Sono Groot e disporre delle autorizzazioni xyz" del token e lo passa al client. In seguito il client presenta il token al server, ma il server necessita di un tipo di garanzia che il client non è ancora contraffatte il token. In questo modo il primo requisito: autenticità (anche noto come integrità, correzione manomissioni).

Poiché lo stato persistente è considerato attendibile dal server, si prevede che questo stato può contenere informazioni specifiche per l'ambiente operativo. È possibile sotto forma di un percorso di file, un'autorizzazione, un handle o altri riferimento indiretto o di altre parti di dati specifici del server. Tali informazioni in genere non identificabile da un client non attendibile. In questo modo il secondo requisito: riservatezza.

Infine, poiché sono componenti applicazioni moderne, abbiamo visto è che singoli componenti verranno possa sfruttare i vantaggi di questo sistema senza tenere in considerazione altri componenti nel sistema. Ad esempio, se un componente di token di connessione, viene utilizzato questo stack, devono operare senza interferenze da un meccanismo di anti-CSRF che potrebbe anche usare lo stesso stack. In questo modo il requisito finale: isolamento.

È possibile fornire ulteriori vincoli per limitare l'ambito dei requisiti. Si presuppone che tutti i servizi operando all'interno di sistema di crittografia sono ugualmente attendibili e che i dati non debbano essere generato o utilizzato all'esterno di servizi sotto il controllo diretto. Inoltre, è necessario che le operazioni sono più rapidamente possibile poiché ogni richiesta al servizio web può passare attraverso il sistema di crittografia una o più volte. In questo modo la soluzione ideale per questo scenario la crittografia simmetrica ed è possibile sconto crittografia asimmetrica fino ad una volta che è necessario.

## <a name="design-philosophy"></a>Filosofia di progettazione

Abbiamo iniziato identificando i problemi con lo stack di esistente. Una volta che abbiamo, è un sondaggio orizzontale delle soluzioni esistenti e ha rilevato che nessuna soluzione era piuttosto la funzionalità che è richiesto. È quindi progettato una soluzione basata su orientamenti diversi.

* Il sistema deve offrire semplicità di configurazione. Idealmente il sistema sarà senza operazioni di configurazione e gli sviluppatori Impossibile hit interamente in esecuzione. Situazioni in cui gli sviluppatori devono configurare un aspetto specifico (ad esempio l'archivio chiave), debba essere presi in considerazione semplificare tali configurazioni specifiche.

* Offre una semplice API per consumatori. Le API devono essere facile da utilizzare in modo corretto e difficile da usare in modo non corretto.

* Gli sviluppatori non imparare principi di gestione delle chiavi. Il sistema deve gestire selezione algoritmo e la durata di chiave per conto dello sviluppatore. Idealmente lo sviluppatore non è nemmeno dovrebbe disporre di accesso al materiale della chiave non elaborato.

* Le chiavi devono essere protetti inattivi quando possibile. Il sistema deve individuare un meccanismo di protezione predefinito appropriato e applicarlo automaticamente.

Con questi principi presente è stato sviluppato un semplice, [facile da usare](using-data-protection.md) dello stack di protezione dati.

Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservato. Altre tecnologie come [DPAPI CNG di Windows](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) sono più adatti per lo scenario di memorizzazione indefinito, e hanno funzionalità di gestione delle chiavi conseguentemente sicuro. Ciò premesso, non c'è niente che uno sviluppatore di utilizzare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati.

## <a name="audience"></a>Destinatari

Il sistema di protezione dati è suddivisa in cinque pacchetti principali. Vari aspetti di queste API destinazione tre gruppi di destinatari principale;

1. Il [Consumer API Panoramica](consumer-apis/overview.md) gli sviluppatori di applicazioni e framework di destinazione.

   "Non si desidera ulteriori informazioni sulla modalità di funzionamento dello stack o sul modo in cui è configurato. Desidera semplicemente eseguire alcune operazioni in come semplice modo possibile con elevata probabilità di utilizzando le API correttamente."

2. Il [le API di configurazione](configuration/overview.md) gli sviluppatori di applicazioni e gli amministratori di sistema di destinazione.

   "È necessario che l'ambiente richiede i percorsi non predefiniti o le impostazioni per il sistema di protezione dati".

3. Gli sviluppatori di destinazione API extensibility responsabile implementazione dei criteri personalizzati. Utilizzo di queste API limitato a determinate situazioni e si è verificato, gli sviluppatori compatibile con protezione.

   "È necessario sostituire un intero componente all'interno del sistema perché ho requisiti comportamentali realmente univoci. Desidero informazioni uncommonly parti della superficie dell'API per compilare un plug-in che soddisfa i requisiti."

## <a name="package-layout"></a>Layout del pacchetto

Lo stack di protezione dati è costituito da cinque pacchetti.

* Microsoft.AspNetCore.DataProtection.Abstractions contiene le interfacce di base IDataProtectionProvider e oggetto IDataProtector. Contiene inoltre i metodi di estensione utili che consentono l'utilizzo di questi tipi (ad esempio, gli overload di IDataProtector.Protect). Vedere la sezione di interfacce utente per ulteriori informazioni. Se un altro utente è responsabile della creazione di un'istanza nel sistema di protezione dati e si utilizza semplicemente le API, è opportuno Microsoft.AspNetCore.DataProtection.Abstractions di riferimento.

* Microsoft.AspNetCore.DataProtection contiene l'implementazione di base del sistema di protezione dati, compresi configurazione, estendibilità, gestione delle chiavi e operazioni di crittografia di base. Se si è responsabile della creazione di un'istanza nel sistema di protezione dati (ad esempio, aggiungerlo a un IServiceCollection) o modificare o estendere il comportamento, è opportuno Microsoft.AspNetCore.DataProtection di riferimento.

* Microsoft.AspNetCore.DataProtection.Extensions contiene API aggiuntive, quali gli sviluppatori possono risultare utili ma che non appartengono il pacchetto principale. Ad esempio, il pacchetto contiene un'API di semplice "creare un'istanza del sistema punta a una directory di archiviazione di chiavi specifico con alcuna installazione di aggiunta della dipendenza" (informazioni). Contiene inoltre i metodi di estensione per limitare la durata del payload protetto (informazioni).

* Microsoft.AspNetCore.DataProtection.SystemWeb può essere installato in un'applicazione di 4. x ASP.NET esistente per reindirizzare il <machineKey> operazioni per usare il nuovo stack di protezione dati. Vedere [compatibilità](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) per ulteriori informazioni.

* Microsoft.AspNetCore.Cryptography.KeyDerivation fornisce un'implementazione della password PBKDF2 hashing routine e può essere utilizzato dai sistemi che necessitano di gestire in modo sicuro le password utente. Vedere [Hashing della Password](consumer-apis/password-hashing.md) per ulteriori informazioni.
