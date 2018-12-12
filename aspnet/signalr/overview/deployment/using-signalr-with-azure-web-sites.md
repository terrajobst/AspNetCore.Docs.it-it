---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Uso di SignalR con App Web nel servizio App di Azure | Microsoft Docs
author: pfletcher
description: Questo documento descrive come configurare un'applicazione di SignalR in esecuzione su Microsoft Azure. Le versioni del software utilizzato nell'esercitazione di Visual Studio 2013 o vis...
ms.author: riande
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: da69e1aba1b56d69ad8e710cddd2b492168f1255
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287756"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Uso di SignalR con App Web nel servizio App di Azure
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento descrive come configurare un'applicazione di SignalR in esecuzione su Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012
> - .NET 4.5
> - SignalR versione 2
> - Azure SDK 2.3 per Visual Studio 2013 o 2012
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), o il [forum di Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Sommario

- [Introduzione](#introduction)
- [Distribuzione di un'App Web di SignalR in servizio App di Azure](#deploying)
- [Abilitazione di WebSocket nel servizio App di Azure](#websocket)
- [Usando il Backplane di Cache Redis di Azure](#backplane)
- [Passaggi successivi](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introduzione

ASP.NET SignalR è utilizzabile per visualizzare un nuovo livello di interattività tra i server e web o i client .NET. Quando ospitata in Azure, SignalR le applicazioni possono sfruttare la disponibilità elevata, scalabili, e ambiente ad alte prestazioni per l'esecuzione nel cloud.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Distribuzione di un'App Web di SignalR in servizio App di Azure

SignalR non aggiunge le complicazioni particolare alla distribuzione di un'applicazione in Azure rispetto alla distribuzione a un server in locale. Un'applicazione che usa SignalR può essere ospitata in Azure senza apportare modifiche di configurazione o altre impostazioni (anche se per il supporto di WebSocket, vedere [abilitazione di WebSocket nel servizio App di Azure](#websocket) sotto.) Per questa esercitazione si distribuirà l'applicazione creata nel [esercitazione introduttiva su](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.

**Prerequisiti**

- Visual Studio 2013. Se non hai Visual Studio, Visual Studio 2013 Express per Web è incluso nell'installazione di Azure SDK.
- [Azure SDK 2.3 per Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) oppure [Azure SDK 2.3 per Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Per completare questa esercitazione, è necessaria una sottoscrizione di Azure. È possibile [attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), o [iscriversi per una sottoscrizione di valutazione](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Distribuzione di un'app web di SignalR in Azure

1. Completare la [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare il progetto completato dal [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. In Visual Studio, selezionare **compilare**, **pubblicare SignalR Chat**.
3. Nella finestra di dialogo "Pubblica sul Web", selezionare "siti Web di Windows Azure".

    ![Selezionare i siti Web di Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Se si non sono connessi al tuo account Microsoft, fare clic su **Accedi...**  nella finestra di dialogo "selezionare le sito Web esistente" e di accesso.

    ![Selezionare il sito Web esistente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Accedere ad Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Nella finestra di dialogo "selezionare le sito Web esistente", fare clic su **New**.

    ![Nuovo sito Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. Nella finestra di dialogo "Crea sito in Windows Azure", immettere un nome univoco dell'app. Selezionare l'area più vicina all'utente nell'elenco a discesa area. Scegliere **Crea**.

    ![Crea sito in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Nella finestra di dialogo "Pubblica sul Web", fare clic su **pubblica**.

    ![Pubblicazione del sito](using-signalr-with-azure-web-sites/_static/image6.png)
8. Quando l'app è stata completata la pubblicazione, l'applicazione di SignalR Chat ospitata in App Web di servizio App di Azure verrà aperta in un browser.

    ![Sito aperto in un browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Abilitazione di protocolli WebSocket in App Web di servizio App di Azure

WebSocket deve essere abilitata in modo esplicito nell'app web da utilizzare in un'applicazione di SignalR. in caso contrario, sarà possibile usare altri protocolli (vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports) per informazioni dettagliate).

Per poter usare i WebSocket in App Web di servizio App di Azure, attivarlo nella sezione di configurazione dell'app web. A tale scopo, aprire l'app web nel [portale di gestione Azure](https://manage.windowsazure.com/)e scegliere Configura.

![Scheda Configura](using-signalr-with-azure-web-sites/_static/image8.png)

Nella parte superiore della pagina di configurazione, assicurarsi che .NET 4.5 viene utilizzato per l'app web.

![Impostazione di .NET framework versione 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

Nella pagina di configurazione, nelle **WebSockets** impostazione, selezionare **su**.

![Impostazione WebSocket: Attivato](using-signalr-with-azure-web-sites/_static/image10.png)

Nella parte inferiore della pagina di configurazione, selezionare **salvare** per salvare le modifiche.

![Salvare le impostazioni](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Usando il Backplane di Cache Redis di Azure

Se si usano più istanze per l'app web e gli utenti di tali istanze devono interagire tra loro (in modo che, ad esempio, i messaggi di chat creati in una singola istanza possono raggiungere gli utenti connessi alle altre istanze), il [Cache Redis di Azure backplane](../performance/scaleout-with-redis.md) devono essere implementati nell'applicazione.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle App Web in servizio App di Azure, vedere [Panoramica di App Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
