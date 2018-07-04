---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web. Open Web Interface per de .NET (OWIN)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: a370f3bea74332d47e9132206c25d1be4211772c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395571"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web.
> 
> [Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN – ad esempio, all'interno di un ruolo di lavoro di Azure.
> 
> In questa esercitazione si userà il pacchetto Microsoft.Owin.Host.HttpListener, che fornisce un server HTTP che consente di self-hosting delle applicazioni OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - API Web 2
> - [Azure SDK per .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. Sono necessari privilegi di amministratore per eseguire il debug in locale, l'applicazione usa l'emulatore di calcolo di Azure.

Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**. Dal **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Nel **nuovo servizio Cloud di Windows Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio aggiunge un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creato contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione di Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, sebbene questa esercitazione Usa un singolo ruolo.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere l'API Web e pacchetti OWIN

Dal **degli strumenti** menu, fare clic su **Library Package Manager**, quindi fare clic su **Package Manager Console**.

Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un HTTP Endpoint

In Esplora soluzioni espandere il progetto AzureApp. Espandere il nodo ruoli, selezionare e fare doppio clic su WorkerRole1 **proprietà**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Fare clic su **endpoint**, quindi fare clic su **Add Endpoint**.

Nel **protocollo** dall'elenco a discesa selezionare "http". Nelle **porta pubblica** e **porta privata**, digitare 80. Questi numeri di porta possono essere diversi. La porta pubblica è ciò che i client usano quando si invia una richiesta al ruolo.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurare Web API per l'hosting indipendente

In Esplora soluzioni fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Add** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Sostituire tutto il codice boilerplate in questo file con il codice seguente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un Controller API Web

Successivamente, aggiungere una classe controller API Web. Fare clic sul progetto WorkerRole1 e selezionare **Add** / **classe**. Nome della classe TestController. Sostituire tutto il codice boilerplate in questo file con il codice seguente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Per semplicità, questo controller definisce solo due metodi GET che restituiscono testo normale.

## <a name="start-the-owin-host"></a>Avviare l'Host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere la seguente istruzione using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Aggiungere un **IDisposable** membro per il `WorkerRole` classe:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Il **WebApp.Start** metodo avvia l'host OWIN. Il nome del `Startup` classe è un parametro di tipo al metodo. Per convenzione, l'host chiamerà il `Configure` overload di questa classe.

Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione in locale nell'emulatore di calcolo di Azure. A seconda delle impostazioni di firewall, potrebbe essere necessario consentire all'emulatore attraverso il firewall.

> [!NOTE]
> Se si verifica un'eccezione simile al seguente, vedi [questo post di blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa. "Impossibile caricare il file o l'assembly ' owin, versione = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. Definizione del manifesto dell'assembly individuato non corrisponde il riferimento all'assembly. (Eccezione da HRESULT: 0x80131040) "


L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint. È possibile trovare l'indirizzo IP, visualizzare l'interfaccia utente emulatore di calcolo. Fare doppio clic sull'icona dell'emulatore nella barra area di notifica delle attività e selezionare **Mostra interfaccia emulatore di calcolo**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Trovare l'indirizzo IP sotto le distribuzioni del servizio, la distribuzione [id], i dettagli del servizio. Aprire un web browser e passare a http://<em>indirizzi</em>/test/1, dove <em>indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`. Verrà visualizzata la risposta dal controller API Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account Azure. Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni fare clic sul progetto AzureApp. Selezionare **Pubblica**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se non è stato eseguito l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Dopo che è stato effettuato, scegliere una sottoscrizione e fare clic su **successivo**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Fare clic su **Pubblica**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando l'app viene distribuita, passa a http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana Project su GitHub](https://github.com/aspnet/AspNetKatana)
