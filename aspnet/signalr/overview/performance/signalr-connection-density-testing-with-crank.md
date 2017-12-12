---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: "Densità di connessione SignalR test con Crank | Documenti Microsoft"
author: tfitzmac
description: "Densità di connessione SignalR test con perno"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2017
---
<a name="signalr-connection-density-testing-with-crank"></a>Densità di connessione SignalR test con perno
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come utilizzare lo strumento Crank per testare un'applicazione con più client simulato.


Quando l'applicazione è in esecuzione nel proprio ambiente di hosting (entrambi Azure un ruolo, IIS, web o in modo indipendente tramite Owin), è possibile testare la risposta dell'applicazione a un elevato livello di densità di connessione mediante lo strumento perno. L'ambiente di hosting può essere un server Internet Information Services (IIS), un host Owin o un ruolo web Azure. (Nota: i contatori delle prestazioni non sono disponibili nelle App Web di servizio App di Azure, pertanto, non sarà in grado di ottenere dati sulle prestazioni da un test di densità di connessione.)

Densità di connessione si riferisce al numero di connessioni TCP simultanee che è possibile stabilire in un server. Ogni connessione TCP comporta il proprio overhead e apertura di un numero elevato di connessioni inattive che consentono di creare un collo di bottiglia della memoria.

[SignalR codebase](https://github.com/signalr/signalr) include uno strumento di test di carico denominato **Crank**. La versione più recente del perno è reperibile [del branch Dev](https://github.com/SignalR/signalr/tree/dev) su GitHub. È possibile scaricare un file Zip codebase archivio del branch Dev di SignalR [qui](https://github.com/SignalR/SignalR/archive/dev.zip).

Crank può essere utilizzato per saturare completamente la memoria del server per calcolare il numero totale di connessioni inattive in hardware del server. In alternativa, è possibile anche utilizzare Crank per test di carico il server in una certa quantità di memoria, da aumentando connessioni fino a quando non viene raggiunto un numero specifico o una soglia di memoria specifici.

Durante il test, è importante utilizzare client remoto per evitare eventuali conflitti per le risorse (ad esempio, le connessioni TCP e memoria). Monitorare i computer client per assicurarsi che non raggiunge eventuali colli di bottiglia che potrebbero impedire il server di raggiungere la capacità massima (memoria o CPU). Si potrebbe essere necessario aumentare il numero di client per caricare completamente il server.

### <a name="running-a-connection-density-test"></a>Esecuzione di un Test di densità di connessione

Questa sezione descrive i passaggi necessari per eseguire un test di densità di connessione in un'applicazione di SignalR.

1. Scaricare e compilare il [codebase branch Dev di SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). In un prompt dei comandi, passare a &lt;directory progetto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Distribuire l'applicazione in ambiente di hosting di destinazione. Prendere nota dell'endpoint che l'applicazione utilizza; ad esempio, nell'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), l'endpoint è `http://<yourhost>:8080/signalr`.
3. Installare [i contatori delle prestazioni di SignalR](signalr-performance.md#perfcounters) sul server. Se l'applicazione è in esecuzione in Azure, vedere [utilizzando SignalR i contatori delle prestazioni in un ruolo Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Dopo avere scaricato e compilato la codebase e i contatori delle prestazioni sull'host sia installato, lo strumento da riga di comando Crank è reperibile nel `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` cartella.

Le opzioni disponibili per lo strumento Crank includono:

- **/?** : Mostra la schermata di Guida. Le opzioni disponibili vengono visualizzate anche se il **Url** parametro viene omesso.
- **/ Url**: l'URL per le connessioni SignalR. Questo parametro è obbligatorio. Per un'applicazione di SignalR mediante il mapping predefinito, il percorso termina "/ signalr".
- **/ Trasporto**: il nome del trasporto utilizzato. Il valore predefinito è `auto`, che verrà selezionato il protocollo ottimale. Le opzioni includono `WebSockets`, `ServerSentEvents`, e `LongPolling` (`ForeverFrame` non è un'opzione per Crank, poiché il client .NET anziché viene utilizzato Internet Explorer). Per ulteriori informazioni su come SignalR seleziona i trasporti, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: il numero di client aggiunti in ogni batch. Il valore predefinito è 50.
- **/ ConnectInterval**: l'intervallo in millisecondi tra l'aggiunta di connessioni. Il valore predefinito è 500.
- **/ Connessioni connessioni**: il numero di connessioni utilizzato per l'applicazione di test di carico. Il valore predefinito è 100.000.
- **/ ConnectTimeout**: il timeout in secondi prima di interrompere il test. Il valore predefinito è 300.
- **MinServerMBytes**: I megabyte minima per il server di raggiungere. Il valore predefinito è 500.
- **SendBytes**: la dimensione del payload inviato al server in byte. Il valore predefinito è 0.
- **SendInterval**: il ritardo in millisecondi tra i messaggi al server. Il valore predefinito è 500.
- **SendTimeout**: il timeout in millisecondi per i messaggi al server. Il valore predefinito è 300.
- **ControllerUrl**: l'Url in cui un client ospita un controller hub. Il valore predefinito è null (Nessun hub controller). L'hub controller viene avviato all'avvio della sessione di Crank; senza ulteriore tra l'hub di controller e Crank contatto.
- **NumClients**: il numero di client simulato per connettersi all'applicazione. Il valore predefinito è uno.
- **File di registro**: il nome del file per il file di registro per l'esecuzione dei test. Il valore predefinito è `crank.csv`.
- **SampleInterval**: il tempo in millisecondi tra i campioni del contatore delle prestazioni. Il valore predefinito è 1000.
- **SignalRInstance**: il nome dell'istanza dei contatori delle prestazioni nel server. Il valore predefinito consiste nell'utilizzare lo stato della connessione client.

### <a name="example"></a>Esempio

Il comando seguente verrà testare un sito denominato `pfsignalr` in Azure che ospita un'applicazione sulla porta 8080 con un hub denominato "ControllerHub", utilizzando 100 connessioni.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
