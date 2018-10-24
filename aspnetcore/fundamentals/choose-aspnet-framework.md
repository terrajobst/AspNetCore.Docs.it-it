---
title: Scegliere tra ASP.NET 4.x e ASP.NET Core
author: rick-anderson
description: Vengono illustrati ASP.NET Core e ASP.NET 4.x e si spiega come scegliere tra le due soluzioni.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: f046491e2ec68b6beaad581e2b04e6688a81f2d1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911045"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="51713-103">Scegliere tra ASP.NET 4.x e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="51713-104">ASP.NET Core è una riprogettazione di ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="51713-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="51713-105">Questo articolo elenca le differenze tra i due.</span><span class="sxs-lookup"><span data-stu-id="51713-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="51713-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-106">ASP.NET Core</span></span>

<span data-ttu-id="51713-107">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="51713-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="51713-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="51713-108">ASP.NET 4.x</span></span>

<span data-ttu-id="51713-109">ASP.NET 4.x è un framework consolidato che offre i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.</span><span class="sxs-lookup"><span data-stu-id="51713-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="51713-110">Selezione del framework</span><span class="sxs-lookup"><span data-stu-id="51713-110">Framework selection</span></span>

<span data-ttu-id="51713-111">La tabella seguente mette a confronto ASP.NET Core e ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="51713-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="51713-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-112">ASP.NET Core</span></span> | <span data-ttu-id="51713-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="51713-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="51713-114">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="51713-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="51713-115">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="51713-115">Build for Windows</span></span>|
|<span data-ttu-id="51713-116">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="51713-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="51713-117">Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="51713-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="51713-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="51713-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="51713-119">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="51713-119">Multiple versions per machine</span></span>|<span data-ttu-id="51713-120">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="51713-120">One version per machine</span></span>|
|<span data-ttu-id="51713-121">Sviluppare con Visual Studio, [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="51713-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="51713-122">Sviluppare con Visual Studio tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="51713-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="51713-123">Prestazioni più elevate rispetto ad ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="51713-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="51713-124">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="51713-124">Good performance</span></span>|
|[<span data-ttu-id="51713-125">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="51713-126">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="51713-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="51713-127">Vedere [ASP.NET Core per .NET Framework](xref:index#target-framework) per informazioni sul supporto per ASP.NET Core 2.x in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="51713-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="51713-128">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="51713-129">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="51713-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="51713-130">Siti Web</span><span class="sxs-lookup"><span data-stu-id="51713-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="51713-131">API</span><span class="sxs-lookup"><span data-stu-id="51713-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="51713-132">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="51713-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="51713-133">Distribuire un'app ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="51713-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="51713-134">Scenari ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="51713-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="51713-135">Siti Web</span><span class="sxs-lookup"><span data-stu-id="51713-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="51713-136">API</span><span class="sxs-lookup"><span data-stu-id="51713-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="51713-137">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="51713-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="51713-138">Creare un'app Web ASP.NET 4.x in Azure</span><span class="sxs-lookup"><span data-stu-id="51713-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="51713-139">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="51713-139">Additional resources</span></span>

* [<span data-ttu-id="51713-140">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51713-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="51713-141">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51713-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
