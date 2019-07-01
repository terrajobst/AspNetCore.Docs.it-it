---
title: Scegliere tra ASP.NET 4.x e ASP.NET Core
author: rick-anderson
description: Vengono illustrati ASP.NET Core e ASP.NET 4.x e si spiega come scegliere tra le due soluzioni.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/02/2019
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 97d65df8df1e27694d818098958288874e8ab92e
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399172"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="69443-103">Scegliere tra ASP.NET 4.x e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="69443-104">ASP.NET Core è una riprogettazione di ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="69443-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="69443-105">Questo articolo elenca le differenze tra i due.</span><span class="sxs-lookup"><span data-stu-id="69443-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="69443-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-106">ASP.NET Core</span></span>

<span data-ttu-id="69443-107">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="69443-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="69443-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="69443-108">ASP.NET 4.x</span></span>

<span data-ttu-id="69443-109">ASP.NET 4.x è un framework consolidato che offre i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.</span><span class="sxs-lookup"><span data-stu-id="69443-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="69443-110">Selezione del framework</span><span class="sxs-lookup"><span data-stu-id="69443-110">Framework selection</span></span>

<span data-ttu-id="69443-111">La tabella seguente mette a confronto ASP.NET Core e ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="69443-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="69443-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-112">ASP.NET Core</span></span> | <span data-ttu-id="69443-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="69443-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="69443-114">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="69443-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="69443-115">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="69443-115">Build for Windows</span></span>|
|<span data-ttu-id="69443-116">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="69443-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="69443-117">Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="69443-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="69443-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="69443-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="69443-119">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="69443-119">Multiple versions per machine</span></span>|<span data-ttu-id="69443-120">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="69443-120">One version per machine</span></span>|
|<span data-ttu-id="69443-121">Sviluppare con [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="69443-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="69443-122">Sviluppare con [Visual Studio](https://visualstudio.microsoft.com/vs/) tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="69443-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="69443-123">Prestazioni più elevate rispetto ad ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="69443-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="69443-124">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="69443-124">Good performance</span></span>|
|[<span data-ttu-id="69443-125">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="69443-126">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="69443-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="69443-127">Vedere [ASP.NET Core per .NET Framework](xref:index#target-framework) per informazioni sul supporto per ASP.NET Core 2.x in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="69443-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="69443-128">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="69443-129">Siti Web</span><span class="sxs-lookup"><span data-stu-id="69443-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="69443-130">API</span><span class="sxs-lookup"><span data-stu-id="69443-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="69443-131">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="69443-131">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="69443-132">Distribuire un'app ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="69443-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="69443-133">Scenari ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="69443-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="69443-134">Siti Web</span><span class="sxs-lookup"><span data-stu-id="69443-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="69443-135">API</span><span class="sxs-lookup"><span data-stu-id="69443-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="69443-136">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="69443-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="69443-137">Creare un'app Web ASP.NET 4.x in Azure</span><span class="sxs-lookup"><span data-stu-id="69443-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="69443-138">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="69443-138">Additional resources</span></span>

* [<span data-ttu-id="69443-139">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="69443-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="69443-140">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69443-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
