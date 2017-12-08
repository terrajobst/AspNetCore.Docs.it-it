---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana | Documenti Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a><span data-ttu-id="56330-102">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="56330-102">Katana Samples</span></span>
====================
<span data-ttu-id="56330-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="56330-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="56330-104">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="56330-104">Katana Samples</span></span>

<span data-ttu-id="56330-105">**Esegue il routing ASP.NET esempio** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="56330-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="56330-106">In alcune applicazioni è possibile collegare i componenti OWIN nella tabella di routing di Asp.Net in modo affiancato con componenti non OWIN.</span><span class="sxs-lookup"><span data-stu-id="56330-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="56330-107">In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute forniti da systemweb.</span><span class="sxs-lookup"><span data-stu-id="56330-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="56330-108">**Creazione di rami pipeline esempio** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="56330-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="56330-109">Pipeline di elaborazione della richiesta OWIN non deve necessariamente essere lineare, è possibile creare per elaborare le richieste in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="56330-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="56330-110">Questo esempio viene illustrato come creare una pipeline di diramazione in base a percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="56330-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="56330-111">Questi componenti sono disponibili nel pacchetto nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="56330-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="56330-112">**Esempio di Server personalizzati** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="56330-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="56330-113">Viene illustrato come utilizzare un server OWIN personalizzato quando il Self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="56330-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="56330-114">**Esempio incorporato** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="56330-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="56330-115">Alcuni server OWIN può essere eseguito all'interno di un processo personalizzato (&quot;self-hosted&quot;).</span><span class="sxs-lookup"><span data-stu-id="56330-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="56330-116">In questo esempio viene illustrato come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto nuget di owin.</span><span class="sxs-lookup"><span data-stu-id="56330-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="56330-117">**Esempio HelloWorld** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="56330-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="56330-118">OWIN è un server HTTP astrazione di API che consentono la portabilità dell'applicazione tra più server.</span><span class="sxs-lookup"><span data-stu-id="56330-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="56330-119">In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando alcune **semplici wrapper** intorno l'astrazione di OWIN non elaborati e l'esecuzione su un server web come ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="56330-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="56330-120">**Esempio Hello World Raw OWIN** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="56330-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="56330-121">In questo esempio viene illustrato come scrivere un'applicazione Hello World utilizzando il **raw** astrazione OWIN ed eseguirlo in un server web ad Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="56330-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="56330-122">**Esempio di SignalR** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="56330-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="56330-123">Viene illustrato come self-hosting SignalR utilizzando OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="56330-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="56330-124">Per ulteriori informazioni su SignalR self-hosting, vedere [esercitazione: SignalR indipendente](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="56330-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="56330-125">**Esempio di file statici** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="56330-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="56330-126">Viene illustrato come supportare le richieste HTTP per i file statici con OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="56330-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="56330-127">**API Web** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="56330-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="56330-128">In questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere l'API Web per la pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="56330-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="56330-129">**Esempio di Socket Web** | [codice sorgente](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="56330-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="56330-130">Viene illustrato come supportare WebSocket in OWIN tramite il [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="56330-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
