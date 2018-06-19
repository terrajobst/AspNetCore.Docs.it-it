---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utilizzare OWIN per l'hosting indipendente ASP.NET Web API 2 | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come ospitare API Web ASP.NET in un'applicazione console, utilizzando OWIN per l'hosting indipendente framework Web API. Aprire interfaccia Web per .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506970"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Utilizzare OWIN per l'hosting indipendente ASP.NET Web API 2
====================
da [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> In questa esercitazione viene illustrato come ospitare API Web ASP.NET in un'applicazione console, utilizzando OWIN per l'hosting indipendente framework Web API.
> 
> [Aprire l'interfaccia Web per .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funziona anche con Visual Studio 2012)
> - Web API 2


> [!NOTE]
> È possibile trovare il codice sorgente completo per questa esercitazione in [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Creare un'applicazione Console

Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**. Da **modelli installati**, in Visual c#, fare clic su **Windows** e quindi fare clic su **applicazione Console**. Denominare il progetto "OwinSelfhostSample" e fare clic su **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere l'API Web e pacchetti OWIN

Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Verrà installato il pacchetto di selfhost WebAPI OWIN e tutti i pacchetti richiesti OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurare l'API Web per l'hosting indipendente

In Esplora soluzioni, fare clic con il pulsante destro del progetto e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un Controller API Web

Successivamente, aggiungere una classe controller API Web. In Esplora soluzioni, fare clic con il pulsante destro del progetto e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `ValuesController`.

Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Avviare l'Host OWIN ed effettuare una richiesta usando HttpClient

Sostituire tutto il codice boilerplate nel file Program.cs con il seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Per eseguire l'applicazione, premere F5 in Visual Studio. L'output dovrebbe essere simile al seguente:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Una panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hosting ASP.NET Web API a un ruolo di lavoro di Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
