---
uid: signalr/overview/performance/scaleout-with-redis
title: "Scalabilità orizzontale SignalR con Redis | Documenti Microsoft"
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 965c32a4e2f2c9c4bd457d0c13ae99c1378c22c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis"></a>Scalabilità orizzontale SignalR con Redis
====================
da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


In questa esercitazione si utilizzerà [Redis](http://redis.io/) per distribuire i messaggi tra un'applicazione di SignalR che viene distribuita in due istanze separate di IIS.

Redis è un archivio chiave-valore in memoria. Supporta inoltre un sistema di messaggistica con un modello di pubblicazione/sottoscrizione. Backplane SignalR Redis Usa la funzionalità di pubblicazione/sottoscrizione per inoltrare i messaggi ad altri server.

![](scaleout-with-redis/_static/image1.png)

Per questa esercitazione si utilizzerà tre server:

- Due server che eseguono Windows, che verrà utilizzato per distribuire un'applicazione di SignalR.
- Un server che eseguono Linux, che verrà utilizzato per l'esecuzione di Redis. Per le schermate in questa esercitazione, si usa Ubuntu 12.04 TLS.

Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V. Un'altra opzione consiste nel creare macchine virtuali in Azure.

Anche se in questa esercitazione viene utilizzato l'implementazione di Redis ufficiale, è inoltre disponibile un [Windows porta di Redis](https://github.com/MSOpenTech/redis) da MSOpenTech. Il programma di installazione e configurazione sono diversi, ma in caso contrario, i passaggi sono uguali.

> [!NOTE] 
> 
> Scalabilità orizzontale SignalR con Redis non supporta i cluster Redis.


## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Installare Redis e avviare il server Redis.
2. Aggiungere questi pacchetti NuGet per l'applicazione: 

    - [Microsoft. Aspnet](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Italiano](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente al Startup.cs configurare backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu in Hyper-V

Con Hyper-V di Windows, è possibile creare facilmente una VM Ubuntu in Windows Server.

Scaricare il file ISO Ubuntu dalla [http://www.ubuntu.com](http://www.ubuntu.com/).

In Hyper-V, aggiungere una nuova macchina virtuale. Nel **connessione disco rigido virtuale** passaggio seleziona **creare un disco rigido virtuale**.

![](scaleout-with-redis/_static/image2.png)

Nel **opzioni di installazione** passaggio seleziona **il file di immagine (ISO)**, fare clic su **Sfoglia**e passare all'installazione Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installare Redis

Seguire i passaggi in [http://redis.io/download](http://redis.io/download) per scaricare e compilare Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Verranno compilati i file binari Redis `src` directory.

Per impostazione predefinita, Redis non richiede una password. Per impostare una password, modificare il `redis.conf` file, che si trova nella directory radice del codice sorgente. (Creare una copia di backup del file prima di modificarlo!) Aggiungere la seguente direttiva di `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Ora avviare il server Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Aprire la porta 6379, ovvero la porta predefinita Redis in ascolto. (È possibile modificare il numero di porta nel file di configurazione).

## <a name="create-the-signalr-application"></a>Creare l'applicazione di SignalR

Creare un'applicazione di SignalR una di queste esercitazioni:

- [Introduzione a SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Guida introduttiva a SignalR 2.0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Successivamente, verrà modificata l'applicazione di chat per il supporto di scalabilità orizzontale con Redis. In primo luogo, aggiungere il pacchetto SignalR.Redis NuGet al progetto. In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Successivamente, aprire il file Startup.cs. Aggiungere il codice seguente per il **configurazione** metodo:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" è il nome del server che esegue Redis.
- *porta* è il numero di porta
- "password" è la password che è definito nel file conf.
- "AppName" è una stringa. SignalR crea un canale di pubblicazione/sottoscrizione di Redis con questo nome.

Ad esempio:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze del Server di Windows per distribuire l'applicazione di SignalR.

Aggiungere il ruolo IIS. Includere le funzionalità di "Sviluppo di applicazioni", inclusi il protocollo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Includere anche il servizio di gestione (elencati in "Strumenti di gestione").

![](scaleout-with-redis/_static/image6.png)

**Installare Web Deploy 3.0.** Quando si esegue Gestione IIS, verrà chiesto di installare una piattaforma Web Microsoft oppure è possibile [scaricare il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione di piattaforma, eseguire la ricerca di distribuzione Web e installare distribuzione Web 3.0

![](scaleout-with-redis/_static/image7.png)

Verificare che il servizio gestione Web è in esecuzione. In caso contrario, avviare il servizio. (Se il servizio di gestione Web non viene visualizzato nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione è installato quando si aggiunge il ruolo IIS.)

Per impostazione predefinita, il servizio gestione Web è in ascolto sulla porta TCP 8172. In Windows Firewall, creare una nuova regola in entrata per consentire il traffico TCP sulla porta 8172. Per ulteriori informazioni, vedere [configurazione delle regole Firewall](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx). (Se si ospita le macchine virtuali di Azure, è possibile farlo direttamente nel portale di Azure. Vedere [come configurare gli endpoint a una macchina virtuale](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.

Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere messaggi SignalR da altro. (Naturalmente, in un ambiente di produzione, i due server sarebbe sit dietro il bilanciamento del carico.)

![](scaleout-with-redis/_static/image8.png)

Se si desidera visualizzare i messaggi inviati a Redis, è possibile utilizzare il **redis-cli** client, che viene installato con Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
