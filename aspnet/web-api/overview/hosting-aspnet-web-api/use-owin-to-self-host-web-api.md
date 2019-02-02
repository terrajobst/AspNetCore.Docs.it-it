---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usare OWIN per l'hosting indipendente di API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web. Open Web Interface for .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667388"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a>Usare OWIN per l'hosting indipendente di API Web ASP.NET 
====================

> Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web.
>
> [Open Web Interface for .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Web API 5.2.7


> [!NOTE]
> È possibile trovare il codice sorgente completo per questa esercitazione in [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Creare un'applicazione console

Nel **File** dal menu **New**, quindi selezionare **progetto**. Dal **Installed**, in **Visual C#** , selezionare **Desktop di Windows** e quindi selezionare **App Console (.Net Framework)**. Denominare il progetto "OwinSelfhostSample" e selezionare **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere i pacchetti di API Web e OWIN

Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Verranno installati il pacchetto selfhost WebAPI OWIN e tutti i pacchetti necessari di OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurare Web API per l'hosting indipendente

In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Sostituire tutto il codice boilerplate in questo file con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un controller API Web

Successivamente, aggiungere una classe controller API Web. In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `ValuesController`.

Sostituire tutto il codice boilerplate in questo file con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Avviare l'Host OWIN e inviare una richiesta con HttpClient

Sostituire tutto il codice boilerplate nel file Program.cs con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Esecuzione dell'applicazione

Per eseguire l'applicazione, premere F5 in Visual Studio. L'output dovrebbe essere simile al seguente:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hosting dell'API Web ASP.NET in un ruolo di lavoro di Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
