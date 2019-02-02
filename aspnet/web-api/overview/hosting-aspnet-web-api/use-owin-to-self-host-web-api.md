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
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="00a39-104">Usare OWIN per l'hosting indipendente di API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00a39-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="00a39-105">Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un'applicazione console, Usa OWIN per l'hosting indipendente il framework API Web.</span><span class="sxs-lookup"><span data-stu-id="00a39-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="00a39-106">[Open Web Interface for .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="00a39-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="00a39-107">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="00a39-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="00a39-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="00a39-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="00a39-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="00a39-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="00a39-110">Web API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="00a39-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="00a39-111">È possibile trovare il codice sorgente completo per questa esercitazione in [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="00a39-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="00a39-112">Creare un'applicazione console</span><span class="sxs-lookup"><span data-stu-id="00a39-112">Create a console application</span></span>

<span data-ttu-id="00a39-113">Nel **File** dal menu **New**, quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="00a39-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="00a39-114">Dal **Installed**, in **Visual C#** , selezionare **Desktop di Windows** e quindi selezionare **App Console (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="00a39-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="00a39-115">Denominare il progetto "OwinSelfhostSample" e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="00a39-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="00a39-116">Aggiungere i pacchetti di API Web e OWIN</span><span class="sxs-lookup"><span data-stu-id="00a39-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="00a39-117">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="00a39-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="00a39-118">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="00a39-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="00a39-119">Verranno installati il pacchetto selfhost WebAPI OWIN e tutti i pacchetti necessari di OWIN.</span><span class="sxs-lookup"><span data-stu-id="00a39-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="00a39-120">Configurare Web API per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="00a39-120">Configure Web API for self-host</span></span>

<span data-ttu-id="00a39-121">In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="00a39-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="00a39-122">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="00a39-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="00a39-123">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="00a39-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="00a39-124">Aggiungere un controller API Web</span><span class="sxs-lookup"><span data-stu-id="00a39-124">Add a Web API controller</span></span>

<span data-ttu-id="00a39-125">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="00a39-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="00a39-126">In Esplora soluzioni fare clic sul progetto e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="00a39-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="00a39-127">Assegnare alla classe il nome `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="00a39-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="00a39-128">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="00a39-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="00a39-129">Avviare l'Host OWIN e inviare una richiesta con HttpClient</span><span class="sxs-lookup"><span data-stu-id="00a39-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="00a39-130">Sostituire tutto il codice boilerplate nel file Program.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="00a39-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="00a39-131">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="00a39-131">Run the application</span></span>

<span data-ttu-id="00a39-132">Per eseguire l'applicazione, premere F5 in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00a39-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="00a39-133">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="00a39-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="00a39-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="00a39-134">Additional resources</span></span>

[<span data-ttu-id="00a39-135">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="00a39-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="00a39-136">Hosting dell'API Web ASP.NET in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="00a39-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
