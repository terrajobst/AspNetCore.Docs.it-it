---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Scalabilità orizzontale di SignalR con Redis (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 1aa391aadfada85f90fe37a70a983b2efa3fbb62
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833112"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Scalabilità orizzontale di SignalR con Redis (SignalR 1.x)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In questa esercitazione si userà [Redis](http://redis.io/) distribuire i messaggi in un'applicazione di SignalR che viene distribuita in due istanze separate di IIS.

Redis è un archivio chiave-valore in memoria. Supporta inoltre un sistema di messaggistica con un modello publish/subscribe. Backplane SignalR Redis Usa la funzionalità di pubblicazione/sottoscrizione per inoltrare i messaggi ad altri server.

![](scaleout-with-redis/_static/image1.png)

Per questa esercitazione si userà tre server:

- Due server che eseguono Windows, che verrà usato per distribuire un'applicazione di SignalR.
- Un server che esegue Linux, che verrà usato per l'esecuzione di Redis. Per le schermate contenute in questa esercitazione, ho utilizzato Ubuntu 12.04 TLS.

Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V. Un'altra opzione consiste nel creare le macchine virtuali in Azure.

Anche se in questa esercitazione Usa l'implementazione di Redis ufficiale, è inoltre disponibile un' [porte Windows di Redis](https://github.com/MSOpenTech/redis) da MSOpenTech. Il programma di installazione e configurazione sono diversi, ma in caso contrario, i passaggi sono uguali.

> [!NOTE] 
> 
> Scalabilità orizzontale di SignalR con Redis non supporta i cluster Redis.


## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Installare Redis e avviare il server Redis.
2. Aggiungere i pacchetti NuGet per l'applicazione: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente al Global. asax per configurare il backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu in Hyper-V

Con Windows Hyper-V, è possibile creare facilmente una VM Ubuntu in Windows Server.

Scaricare il file ISO di Ubuntu dalla [ http://www.ubuntu.com ](http://www.ubuntu.com/).

In Hyper-V, aggiungere una nuova macchina virtuale. Nel **connessione disco rigido virtuale** passaggio, seleziona **creare un disco rigido virtuale**.

![](scaleout-with-redis/_static/image2.png)

Nel **opzioni di installazione** passaggio, seleziona **file di immagine (con estensione ISO)**, fare clic su **Sfoglia**e individuare l'ISO di installazione di Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installare Redis

Seguire i passaggi descritti in [ http://redis.io/download ](http://redis.io/download) per scaricare e compilare Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Verranno compilati i file binari di Redis `src` directory.

Per impostazione predefinita, Redis non richiede una password. Per impostare una password, modificare il `redis.conf` file che si trova nella directory radice del codice sorgente. (Eseguire una copia di backup del file prima di modificarlo!) Aggiungere la seguente direttiva a `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Ora avviare il server Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Aprire la porta 6379, ovvero la porta predefinita che Redis è in ascolto su. (È possibile modificare il numero di porta nel file di configurazione).

## <a name="create-the-signalr-application"></a>Creare l'applicazione di SignalR

Creare un'applicazione di SignalR seguendo una di queste esercitazioni:

- [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introduzione a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Successivamente, modifichiamo l'applicazione di chat per supportare scalabilità orizzontale con Redis. In primo luogo, aggiungere il pacchetto SignalR.Redis NuGet al progetto. In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Successivamente, aprire il file Global. asax. Aggiungere il codice seguente per il **Application\_avviare** metodo:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" è il nome del server Redis in esecuzione.
- *porta* è il numero di porta
- "password" è la password che è definito nel file conf.
- "AppName" è qualsiasi stringa. SignalR crea un canale di Redis pub/sub con questo nome.

Ad esempio:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze di Windows Server per distribuire l'applicazione di SignalR.

Aggiungere il ruolo IIS. Include funzionalità di "Sviluppo di applicazioni", tra cui il protocollo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Includere anche il servizio di gestione (elencati in "Strumenti di gestione").

![](scaleout-with-redis/_static/image6.png)

**Installare Web Deploy 3.0.** Quando si esegue Gestione IIS, verrà richiesto di installare piattaforma Web Microsoft oppure è possibile [Scarica il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione della piattaforma, eseguire la ricerca di Web Deploy e installare distribuzione Web 3.0

![](scaleout-with-redis/_static/image7.png)

Verificare che il servizio di gestione Web sia in esecuzione. In caso contrario, avviare il servizio. (Se il servizio di gestione Web non viene visualizzata nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione installato quando è stato aggiunto il ruolo IIS.)

Per impostazione predefinita, il servizio di gestione Web è in ascolto sulla porta TCP 8172. In Windows Firewall, creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172. Per altre informazioni, vedere [configurare le regole del Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Se si ospita le macchine virtuali in Azure, è possibile farlo direttamente nel portale di Azure. Visualizzare [come configurare gli endpoint a una macchina virtuale](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.

Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere i messaggi SignalR da un altro. (Naturalmente, in un ambiente di produzione, i due server sarebbe si trovano dietro un bilanciamento del carico.)

![](scaleout-with-redis/_static/image8.png)

Se si è interessati a visualizzare i messaggi che vengono inviate a Redis, è possibile usare la **redis-cli** client, che viene installato con Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
