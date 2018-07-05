---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usare OWIN per l'hosting indipendente di API Web ASP.NET 2 | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web. Open Web Interface for .NET (OWIN) d...
ms.author: aspnetcontent
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9fba2774e3873d32115a14fa0c84b99466eda04f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830911"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="8faf4-104">Usare OWIN per l'hosting indipendente di API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="8faf4-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8faf4-105">da [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="8faf4-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="8faf4-106">Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web.</span><span class="sxs-lookup"><span data-stu-id="8faf4-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="8faf4-107">[Open Web Interface for .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="8faf4-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8faf4-108">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="8faf4-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8faf4-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="8faf4-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8faf4-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funziona anche con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="8faf4-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="8faf4-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="8faf4-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="8faf4-112">È possibile trovare il codice sorgente completo per questa esercitazione in [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="8faf4-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="8faf4-113">Creare un'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="8faf4-113">Create a Console Application</span></span>

<span data-ttu-id="8faf4-114">Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="8faf4-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="8faf4-115">Dal **modelli installati**, in Visual c#, fare clic su **Windows** e quindi fare clic su **applicazione Console**.</span><span class="sxs-lookup"><span data-stu-id="8faf4-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="8faf4-116">Denominare il progetto "OwinSelfhostSample" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8faf4-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="8faf4-117">Aggiungere l'API Web e pacchetti OWIN</span><span class="sxs-lookup"><span data-stu-id="8faf4-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="8faf4-118">Dal **degli strumenti** menu, fare clic su **Library Package Manager**, quindi fare clic su **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="8faf4-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="8faf4-119">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="8faf4-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="8faf4-120">Verranno installati il pacchetto selfhost WebAPI OWIN e tutti i pacchetti necessari di OWIN.</span><span class="sxs-lookup"><span data-stu-id="8faf4-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="8faf4-121">Configurare Web API per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="8faf4-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="8faf4-122">In Esplora soluzioni fare clic con il pulsante destro del progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="8faf4-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8faf4-123">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8faf4-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="8faf4-124">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8faf4-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="8faf4-125">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="8faf4-125">Add a Web API Controller</span></span>

<span data-ttu-id="8faf4-126">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="8faf4-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="8faf4-127">In Esplora soluzioni fare clic con il pulsante destro del progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="8faf4-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8faf4-128">Assegnare alla classe il nome `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="8faf4-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="8faf4-129">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8faf4-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="8faf4-130">Avviare l'Host OWIN e creare una richiesta con HttpClient</span><span class="sxs-lookup"><span data-stu-id="8faf4-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="8faf4-131">Sostituire tutto il codice boilerplate nel file Program.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8faf4-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="8faf4-132">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="8faf4-132">Running the Application</span></span>

<span data-ttu-id="8faf4-133">Per eseguire l'applicazione, premere F5 in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8faf4-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="8faf4-134">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8faf4-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="8faf4-135">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8faf4-135">Additional Resources</span></span>

[<span data-ttu-id="8faf4-136">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="8faf4-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="8faf4-137">Hosting dell'API Web ASP.NET in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="8faf4-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
