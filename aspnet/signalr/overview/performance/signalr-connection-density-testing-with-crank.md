---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Densità di connessione di SignalR con Crank di test | Microsoft Docs
author: bradygaster
description: Densità di connessione di SignalR con Crank di test
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 40c9764f0c47b83df8300553b4b290429937345c
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836545"
---
<a name="signalr-connection-density-testing-with-crank"></a>Densità di connessione di SignalR con Crank di test
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive come usare lo strumento manovella per testare un'applicazione con più client simulato.


L'applicazione è in esecuzione nel relativo ambiente di hosting (un Azure ruolo, IIS, web o reso indipendente utilizzando Owin), è possibile testare la risposta dell'applicazione a un elevato livello di densità di connessione mediante lo strumento di perno. L'ambiente di hosting può essere un server Internet Information Services (IIS), un host Owin o un ruolo web di Azure. (Nota: I contatori delle prestazioni non sono disponibili in App Web di servizio App di Azure, in modo non sarà in grado di ottenere i dati sulle prestazioni da un test di densità di connessione.)

Densità di connessione fa riferimento al numero di connessioni TCP simultanee che possono essere stabilite in un server. Ogni connessione TCP comporta il proprio sovraccarico e apertura di un numero elevato di connessioni inattive consentono di creare un collo di bottiglia della memoria.

[SignalR codebase](https://github.com/signalr/signalr) include uno strumento di test di carico denominato **Crank**. La versione più recente del perno è reperibile nel [branch Dev](https://github.com/SignalR/signalr/tree/dev) su GitHub. È possibile scaricare un file Zip codebase archivio del ramo di sviluppo di SignalR [qui](https://github.com/SignalR/SignalR/archive/dev.zip).

Crank consente completamente saturare la memoria del server per calcolare il numero totale di connessioni inattive in hardware del server. In alternativa, si può anche usare manovella per test di carico il server in una certa quantità di richieste di memoria, da imparare le connessioni fino a quando non viene raggiunta una soglia di memoria specifico o un numero specifico.

Durante il test, è importante usare client remoto per evitare qualsiasi concorrenza per le risorse (ad esempio, le connessioni TCP e memoria). Monitorare i client per garantire che non si avvicinino eventuali colli di bottiglia che potrebbero impedire al server di raggiungere la piena capacità (memoria o CPU). Si potrebbe essere necessario aumentare il numero di client per caricare completamente i server.

### <a name="running-a-connection-density-test"></a>Esecuzione di un Test di densità di connessione

Questa sezione descrive i passaggi necessari per eseguire un test di densità di connessione in un'applicazione di SignalR.

1. Scaricare e compilare il [branch Dev di SignalR codebase](https://github.com/SignalR/SignalR/archive/dev.zip). In un prompt dei comandi, passare a &lt;directory di progetto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Distribuire l'applicazione per l'ambiente di hosting desiderata. Prendere nota dell'endpoint utilizzati dall'applicazione; ad esempio, nell'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), l'endpoint è `http://<yourhost>:8080/signalr`.
3. Installare [contatori delle prestazioni di SignalR](signalr-performance.md#perfcounters) nel server. Se l'applicazione è in esecuzione in Azure, vedere [utilizzo di contatori delle prestazioni di SignalR in un ruolo Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Dopo aver scaricato e compilato la codebase e installato i contatori delle prestazioni nell'host, lo strumento da riga di comando di perno è reperibile nella `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` cartella.

Le opzioni disponibili per lo strumento Crank includono:

- **/?**: Mostra la schermata della Guida. Le opzioni disponibili vengono visualizzate anche se il **Url** parametro viene omesso.
- **/Url**: L'URL per le connessioni SignalR. Questo parametro è obbligatorio. Per un'applicazione di SignalR con il mapping predefinito, il percorso termina "/ signalr".
- **/ Trasporto**: Il nome del trasporto utilizzato. Il valore predefinito è `auto`, che verrà selezionato il protocollo ottimale. Le opzioni includono `WebSockets`, `ServerSentEvents`, e `LongPolling` (`ForeverFrame` non è un'opzione per manovella, poiché il client .NET invece viene utilizzato Internet Explorer). Per altre informazioni su come SignalR seleziona trasporti, vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: Il numero di client aggiunti in ogni batch. Il valore predefinito è 50.
- **/ConnectInterval**: L'intervallo in millisecondi tra l'aggiunta di connessioni. Il valore predefinito è 500.
- **/ Connessioni**: Il numero di connessioni utilizzato per l'applicazione di test di carico. Il valore predefinito è 100.000.
- **/ConnectTimeout**: Il timeout in secondi prima di interrompere il test. Il valore predefinito è 300.
- **MinServerMBytes**: I megabyte minima per il server di raggiungere. Il valore predefinito è 500.
- **SendBytes**: Le dimensioni del payload inviato al server in byte. Il valore predefinito è 0.
- **SendInterval**: Il ritardo in millisecondi tra i messaggi al server. Il valore predefinito è 500.
- **SendTimeout**: Il timeout in millisecondi per i messaggi al server. Il valore predefinito è 300.
- **ControllerUrl**: L'Url in cui un client ospiterà un hub di controller. Il valore predefinito è null (Nessun hub controller). L'hub di controller viene avviato all'avvio della sessione Crank; alcuna ulteriore contatto tra l'hub di controller e Crank viene usato.
- **NumClients**: Il numero di client simulato per connettersi all'applicazione. Il valore predefinito è uno.
- **File di log**: Il nome del file per il file di log per l'esecuzione dei test. Il valore predefinito è `crank.csv`.
- **SampleInterval**: Il tempo in millisecondi tra i campioni di contatore delle prestazioni. Il valore predefinito è 1000.
- **SignalRInstance**: Il nome dell'istanza per i contatori delle prestazioni nel server. L'impostazione predefinita consiste nell'usare lo stato della connessione client.

### <a name="example"></a>Esempio

Il comando seguente eseguirà il test un sito denominato `pfsignalr` di Azure che ospita un'applicazione sulla porta 8080 con un hub denominato "ControllerHub", utilizzando 100 connessioni.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
