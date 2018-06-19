---
title: Scegliere tra ASP.NET e ASP.NET Core
author: rick-anderson
description: Informazioni su come scegliere tra ASP.NET e ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094435"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="a91b5-103">Scegliere tra ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="a91b5-104">Indipendentemente dall'app Web che si sta creando, ASP.NET dispone di una soluzione per tutti: da app Web aziendali destinate a Windows Server a piccoli contenitori di Linux destinati ai microservizi, passando per tutto ciò che è compreso tra i due elementi.</span><span class="sxs-lookup"><span data-stu-id="a91b5-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="a91b5-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-105">ASP.NET Core</span></span>

<span data-ttu-id="a91b5-106">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="a91b5-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="a91b5-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a91b5-107">ASP.NET</span></span>

<span data-ttu-id="a91b5-108">ASP.NET è un framework consolidato che offre tutti i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.</span><span class="sxs-lookup"><span data-stu-id="a91b5-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="a91b5-109">Selezione del framework</span><span class="sxs-lookup"><span data-stu-id="a91b5-109">Framework selection</span></span>

<span data-ttu-id="a91b5-110">Esaminare la tabella seguente per determinare quale framework è più adatto alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="a91b5-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="a91b5-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-111">ASP.NET Core</span></span> | <span data-ttu-id="a91b5-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a91b5-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="a91b5-113">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="a91b5-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="a91b5-114">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="a91b5-114">Build for Windows</span></span>|
|<span data-ttu-id="a91b5-115">[Razor Pages](xref:mvc/razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="a91b5-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="a91b5-116">Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="a91b5-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="a91b5-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="a91b5-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="a91b5-118">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="a91b5-118">Multiple versions per machine</span></span>|<span data-ttu-id="a91b5-119">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="a91b5-119">One version per machine</span></span>|
|<span data-ttu-id="a91b5-120">Sviluppare con Visual Studio, [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="a91b5-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="a91b5-121">Sviluppare con Visual Studio tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="a91b5-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="a91b5-122">Prestazioni più elevate rispetto ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a91b5-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="a91b5-123">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="a91b5-123">Good performance</span></span>|
|[<span data-ttu-id="a91b5-124">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="a91b5-125">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a91b5-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="a91b5-126">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="a91b5-127">[Razor Pages](xref:mvc/razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="a91b5-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="a91b5-128">Siti Web</span><span class="sxs-lookup"><span data-stu-id="a91b5-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="a91b5-129">API</span><span class="sxs-lookup"><span data-stu-id="a91b5-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="a91b5-130">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="a91b5-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="a91b5-131">Scenari ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a91b5-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="a91b5-132">Siti Web</span><span class="sxs-lookup"><span data-stu-id="a91b5-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="a91b5-133">API</span><span class="sxs-lookup"><span data-stu-id="a91b5-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="a91b5-134">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="a91b5-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="a91b5-135">Risorse</span><span class="sxs-lookup"><span data-stu-id="a91b5-135">Resources</span></span>

* [<span data-ttu-id="a91b5-136">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a91b5-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="a91b5-137">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a91b5-137">Introduction to ASP.NET Core</span></span>](xref:index)
