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
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="984df-104">Utilizzare OWIN per l'hosting indipendente ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="984df-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="984df-105">da [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="984df-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="984df-106">In questa esercitazione viene illustrato come ospitare API Web ASP.NET in un'applicazione console, utilizzando OWIN per l'hosting indipendente framework Web API.</span><span class="sxs-lookup"><span data-stu-id="984df-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="984df-107">[Aprire l'interfaccia Web per .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="984df-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="984df-108">OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN.</span><span class="sxs-lookup"><span data-stu-id="984df-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="984df-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="984df-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="984df-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funziona anche con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="984df-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="984df-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="984df-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="984df-112">È possibile trovare il codice sorgente completo per questa esercitazione in [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="984df-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="984df-113">Creare un'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="984df-113">Create a Console Application</span></span>

<span data-ttu-id="984df-114">Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="984df-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="984df-115">Da **modelli installati**, in Visual c#, fare clic su **Windows** e quindi fare clic su **applicazione Console**.</span><span class="sxs-lookup"><span data-stu-id="984df-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="984df-116">Denominare il progetto "OwinSelfhostSample" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="984df-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="984df-117">Aggiungere l'API Web e pacchetti OWIN</span><span class="sxs-lookup"><span data-stu-id="984df-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="984df-118">Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="984df-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="984df-119">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="984df-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="984df-120">Verrà installato il pacchetto di selfhost WebAPI OWIN e tutti i pacchetti richiesti OWIN.</span><span class="sxs-lookup"><span data-stu-id="984df-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="984df-121">Configurare l'API Web per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="984df-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="984df-122">In Esplora soluzioni, fare clic con il pulsante destro del progetto e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="984df-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="984df-123">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="984df-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="984df-124">Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="984df-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="984df-125">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="984df-125">Add a Web API Controller</span></span>

<span data-ttu-id="984df-126">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="984df-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="984df-127">In Esplora soluzioni, fare clic con il pulsante destro del progetto e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="984df-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="984df-128">Assegnare alla classe il nome `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="984df-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="984df-129">Sostituire tutto il codice boilerplate in questo file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="984df-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="984df-130">Avviare l'Host OWIN ed effettuare una richiesta usando HttpClient</span><span class="sxs-lookup"><span data-stu-id="984df-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="984df-131">Sostituire tutto il codice boilerplate nel file Program.cs con il seguente:</span><span class="sxs-lookup"><span data-stu-id="984df-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="984df-132">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="984df-132">Running the Application</span></span>

<span data-ttu-id="984df-133">Per eseguire l'applicazione, premere F5 in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="984df-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="984df-134">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="984df-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="984df-135">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="984df-135">Additional Resources</span></span>

[<span data-ttu-id="984df-136">Una panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="984df-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="984df-137">Hosting ASP.NET Web API a un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="984df-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
