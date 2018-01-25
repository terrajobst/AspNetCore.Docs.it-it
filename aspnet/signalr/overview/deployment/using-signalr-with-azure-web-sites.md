---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Con SignalR App Web nel servizio App di Azure | Documenti Microsoft
author: pfletcher
description: Questo documento viene descritto come configurare un'applicazione di SignalR che viene eseguito su Microsoft Azure. Versioni del software utilizzato nell'esercitazione di Visual Studio 2013 o vis...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Con SignalR App Web nel servizio App di Azure
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo documento viene descritto come configurare un'applicazione di SignalR che viene eseguito su Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) o Visual Studio 2012
> - .NET 4.5
> - SignalR versione 2
> - Azure SDK 2.3 per Visual Studio 2013 o 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o [forum di Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Sommario

- [Introduzione](#introduction)
- [Distribuzione di un'App Web SignalR servizio App di Azure](#deploying)
- [Abilitare i WebSockets sul servizio App di Azure](#websocket)
- [Con il Backplane di Cache Redis di Azure](#backplane)
- [Passaggi successivi](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introduzione

Per visualizzare un nuovo livello di interattività tra server e web oppure i client .NET, è possibile utilizzare ASP.NET SignalR. Quando sono ospitati in Azure, SignalR applicazioni possono sfruttare la disponibilità elevata, scalabili, e ambiente ad alte prestazioni per l'esecuzione nel cloud.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Distribuzione di un'App Web SignalR servizio App di Azure

SignalR non aggiunge le complicazioni particolare per distribuire un'applicazione in Azure e la distribuzione a un server locale. Un'applicazione che utilizza SignalR può essere ospitata in Azure senza apportare modifiche di configurazione o altre impostazioni (anche se per il supporto di WebSocket, vedere [abilitazione WebSocket in Azure App Service](#websocket) sotto.) Per questa esercitazione verrà distribuita l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.

**Prerequisiti**

- Visual Studio 2013. Se non si dispone di Visual Studio, Visual Studio 2013 Express per Web è incluso nell'installazione di Azure SDK.
- [Azure SDK 2.3 per Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2.3 per Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Per completare questa esercitazione, è necessario utilizzare una sottoscrizione di Azure. È possibile [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [iscriversi per una sottoscrizione di valutazione](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Distribuzione di un'app web SignalR in Azure

1. Completare il [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare il progetto finito da [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. In Visual Studio, selezionare **compilare**, **pubblicare SignalR Chat**.
3. Nella finestra di dialogo "Pubblica sul Web", selezionare "siti Web di Windows Azure".

    ![Selezionare i siti Web di Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Se non sei connesso al tuo account Microsoft, fare clic su **Accedi...**  nella finestra di dialogo "Seleziona sito Web" e di accesso.

    ![Selezionare il sito Web esistente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Accedere ad Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Nella finestra di dialogo "Seleziona sito Web", fare clic su **New**.

    ![Nuovo sito Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. Nella finestra di dialogo "Crea sito in Windows Azure", immettere un nome univoco dell'app. Selezionare il paese più vicino all'utente nell'elenco a discesa area. Scegliere **Crea**.

    ![Crea sito in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Nella finestra di dialogo "Pubblica sul Web", fare clic su **pubblica**.

    ![pubblicazione del sito](using-signalr-with-azure-web-sites/_static/image6.png)
8. Quando l'app è stata completata la pubblicazione, l'applicazione di SignalR Chat ospitato in Azure App Service dell'App Web verrà aperto in un browser.

    ![Sito aperto in un browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Abilitazione di WebSocket per le app Web di servizio App di Azure

WebSocket devono essere abilitato in modo esplicito nell'app web da utilizzare in un'applicazione di SignalR. in caso contrario, sarà possibile utilizzare altri protocolli (vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports) per informazioni dettagliate).

Per usare i WebSocket nelle App Web di servizio App di Azure, abilitarla nella sezione di configurazione dell'applicazione web. A tale scopo, aprire l'app web nel [il portale di gestione di Azure](https://manage.windowsazure.com/)e scegliere Configura.

![Scheda Configura](using-signalr-with-azure-web-sites/_static/image8.png)

Nella parte superiore della pagina di configurazione, assicurarsi che .NET 4.5 viene utilizzato per l'app web.

![Impostazione di .NET framework versione 4.5.](using-signalr-with-azure-web-sites/_static/image9.png)

Nella pagina configurazione nel **WebSockets** impostazione, selezionare **su**.

![Impostazione di WebSocket: in](using-signalr-with-azure-web-sites/_static/image10.png)

Nella parte inferiore della pagina di configurazione, selezionare **salvare** per salvare le modifiche.

![Salvare le impostazioni](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Con il Backplane di Cache Redis di Azure

Se si utilizzano più istanze per l'app web e gli utenti di tali istanze devono interagire tra loro (in modo che, ad esempio, i messaggi di chat creati in un'istanza possono raggiungere gli utenti connessi ad altre istanze), il [Cache Redis di Azure backplane](../performance/scaleout-with-redis.md) deve essere implementata nell'applicazione.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sull'App Web nel servizio App di Azure, vedere [Panoramica di App Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
