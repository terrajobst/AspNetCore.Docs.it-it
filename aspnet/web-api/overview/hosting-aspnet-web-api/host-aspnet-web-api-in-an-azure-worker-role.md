---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hosting ASP.NET Web API 2 a un ruolo di lavoro di Azure | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come ospitare API Web ASP.NET in un ruolo di lavoro di Azure, utilizzando OWIN per l'hosting indipendente framework Web API. Aprire interfaccia Web per la Germania .NET (OWIN)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 9a7f8242bf482e81513accfe05e10a64ae0ca0b2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hosting ASP.NET Web API 2 a un ruolo di lavoro di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In questa esercitazione viene illustrato come ospitare API Web ASP.NET in un ruolo di lavoro di Azure, utilizzando OWIN per l'hosting indipendente framework Web API.
> 
> [Aprire l'interfaccia Web per .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN: ad esempio, all'interno di un ruolo di lavoro di Azure.
> 
> In questa esercitazione si utilizzerà il pacchetto di HttpListener, che fornisce un server HTTP utilizzato per l'hosting indipendente applicazioni OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [Azure SDK per .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. Sono necessari privilegi di amministratore per eseguire il debug dell'applicazione localmente, utilizzando l'emulatore di calcolo di Azure.

Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**. Da **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Nel **nuovo servizio Cloud Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio viene aggiunto un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creato contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e configurazione per l'applicazione Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene utilizzato un solo ruolo.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere l'API Web e pacchetti OWIN

Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**.

Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un HTTP Endpoint

In Esplora soluzioni, espandere il progetto AzureApp. Espandere il nodo ruoli, fare doppio clic su WorkerRole1 e selezionare **proprietà**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Fare clic su **endpoint**, quindi fare clic su **Aggiungi Endpoint**.

Nel **protocollo** elenco a discesa, selezionare "http". In **porta pubblica** e **porta privata**, digitare 80. I numeri di porta possono essere diversi. La porta pubblica è ciò che usano i client che inviano una richiesta per il ruolo.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurare l'API Web per l'hosting indipendente

In Esplora soluzioni, fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un Controller API Web

Successivamente, aggiungere una classe controller API Web. Fare clic sul progetto WorkerRole1 e selezionare **Aggiungi** / **classe**. Nome della classe TestController. Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Per semplicità, il controller definisce solo due metodi GET che restituiscono testo normale.

## <a name="start-the-owin-host"></a>Avviare l'Host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere la seguente istruzione using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Aggiungere un **IDisposable** membro per la `WorkerRole` classe:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Il **WebApp** metodo avvia l'host OWIN. Il nome della `Startup` classe è un parametro di tipo al metodo. Per convenzione, l'host chiamerà il `Configure` metodo di questa classe.

Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure. A seconda delle impostazioni del firewall, potrebbe essere necessario consentire l'emulatore attraverso il firewall.

> [!NOTE]
> Se si verifica un'eccezione simile al seguente, vedere [questo post di blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa. "Impossibile caricare il file o l'assembly ' italiano, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle sue dipendenze. Definizione del manifesto dell'assembly il non corrisponde il riferimento all'assembly. (Eccezione da HRESULT: 0x80131040) "


L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint. È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente emulatore di calcolo. Fare doppio clic sull'icona dell'emulatore nell'area di notifica della barra attività e selezionare **Mostra interfaccia utente di emulatore di calcolo**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Individuare l'indirizzo IP in distribuzioni del servizio, la distribuzione [id], i dettagli del servizio. Aprire un web browser e passare a http://*indirizzo*/test/1, dove *indirizzo* è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`. È necessario visualizzare la risposta dal controller API Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account di Azure. Se non hai già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni, fare clic sul progetto AzureApp. Selezionare **Pubblica**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Dopo che si è connessi, scegliere una sottoscrizione e fare clic su **Avanti**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Fare clic su **Pubblica**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando l'applicazione viene distribuita, passare a http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Progetto Katana su GitHub](https://github.com/aspnet/AspNetKatana)
