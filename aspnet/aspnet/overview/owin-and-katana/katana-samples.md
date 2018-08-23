---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Esempi di Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836161"
---
<a name="katana-samples"></a><span data-ttu-id="9366b-102">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="9366b-102">Katana Samples</span></span>
====================
<span data-ttu-id="9366b-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9366b-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="9366b-104">Esempi di Katana</span><span class="sxs-lookup"><span data-stu-id="9366b-104">Katana Samples</span></span>

<span data-ttu-id="9366b-105">**ASP.NET esegue il routing campione** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="9366b-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="9366b-106">In alcune applicazioni si dovrà associare i componenti OWIN nella tabella di route Asp.Net affiancato con componenti non OWIN.</span><span class="sxs-lookup"><span data-stu-id="9366b-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="9366b-107">In questo esempio viene illustrato come utilizzare i metodi di estensione RouteCollection MapOwinPath e MapOwinRoute fornita da systemweb.</span><span class="sxs-lookup"><span data-stu-id="9366b-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="9366b-108">**Creazione di rami pipeline di esempio** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="9366b-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="9366b-109">Le pipeline di elaborazione della richiesta OWIN non deve necessariamente essere lineare, può essere sottoposto a branching per elaborare le richieste in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="9366b-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="9366b-110">Questo esempio viene illustrato come creare una pipeline con rami basata su percorsi di richiesta o altri dati della richiesta, ad esempio le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="9366b-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="9366b-111">Questi componenti sono disponibili nel pacchetto nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="9366b-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="9366b-112">**Esempio di Server personalizzati** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="9366b-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="9366b-113">Viene illustrato come utilizzare un server personalizzato OWIN self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="9366b-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="9366b-114">**Esempio incorporato** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="9366b-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="9366b-115">Alcuni server OWIN può essere eseguito all'interno di un processo personalizzato (&quot;self-hosted&quot;).</span><span class="sxs-lookup"><span data-stu-id="9366b-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="9366b-116">In questo esempio viene illustrato come avviare un'applicazione OWIN usando gli strumenti forniti dal pacchetto nuget di owin.</span><span class="sxs-lookup"><span data-stu-id="9366b-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="9366b-117">**Esempio HelloWorld** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="9366b-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="9366b-118">OWIN è un server HTTP astrazione di API che consente la portabilità delle applicazioni tra server diversi.</span><span class="sxs-lookup"><span data-stu-id="9366b-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="9366b-119">In questo esempio viene illustrato come scrivere un'applicazione Hello World usando alcuni **semplici wrapper** tutto l'astrazione di OWIN e l'esecuzione non elaborato in un server web, ad esempio ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9366b-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="9366b-120">**Esempio Hello World non elaborati OWIN** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="9366b-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="9366b-121">In questo esempio viene illustrato come scrivere un'applicazione Hello World tramite il **raw** astrazione OWIN ed eseguirlo in un server web come Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="9366b-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="9366b-122">**Esempio di SignalR** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="9366b-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="9366b-123">Viene illustrato come self-hosting OWIN tramite SignalR / Katana.</span><span class="sxs-lookup"><span data-stu-id="9366b-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="9366b-124">Per altre informazioni su SignalR self-hosting, vedi [esercitazione: hosting indipendente di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="9366b-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="9366b-125">**Esempio di file statici** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="9366b-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="9366b-126">Viene illustrato come supportare le richieste HTTP per i file statici Usa OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="9366b-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="9366b-127">**API Web** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="9366b-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="9366b-128">Questo esempio viene illustrato come ospitare OWIN in IIS e aggiungere l'API Web alla pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="9366b-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="9366b-129">**Web Socket campione** | [codice sorgente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="9366b-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="9366b-130">Viene illustrato come supportare WebSocket in OWIN usando la [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="9366b-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
