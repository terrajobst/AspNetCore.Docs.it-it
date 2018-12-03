---
title: Scegliere tra ASP.NET 4.x e ASP.NET Core
author: rick-anderson
description: Vengono illustrati ASP.NET Core e ASP.NET 4.x e si spiega come scegliere tra le due soluzioni.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: fa281a068dfdcd8a7c45dd5d0a0c3084c4bc8dbc
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549072"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="ac7ab-103">Scegliere tra ASP.NET 4.x e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="ac7ab-104">ASP.NET Core è una riprogettazione di ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="ac7ab-105">Questo articolo elenca le differenze tra i due.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="ac7ab-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-106">ASP.NET Core</span></span>

<span data-ttu-id="ac7ab-107">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="ac7ab-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="ac7ab-108">ASP.NET 4.x</span></span>

<span data-ttu-id="ac7ab-109">ASP.NET 4.x è un framework consolidato che offre i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="ac7ab-110">Selezione del framework</span><span class="sxs-lookup"><span data-stu-id="ac7ab-110">Framework selection</span></span>

<span data-ttu-id="ac7ab-111">La tabella seguente mette a confronto ASP.NET Core e ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="ac7ab-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-112">ASP.NET Core</span></span> | <span data-ttu-id="ac7ab-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="ac7ab-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="ac7ab-114">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="ac7ab-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="ac7ab-115">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="ac7ab-115">Build for Windows</span></span>|
|<span data-ttu-id="ac7ab-116">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="ac7ab-117">Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ac7ab-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="ac7ab-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="ac7ab-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="ac7ab-119">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="ac7ab-119">Multiple versions per machine</span></span>|<span data-ttu-id="ac7ab-120">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="ac7ab-120">One version per machine</span></span>|
|<span data-ttu-id="ac7ab-121">Sviluppare con Visual Studio, [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="ac7ab-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="ac7ab-122">Sviluppare con Visual Studio tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="ac7ab-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="ac7ab-123">Prestazioni più elevate rispetto ad ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="ac7ab-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="ac7ab-124">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="ac7ab-124">Good performance</span></span>|
|[<span data-ttu-id="ac7ab-125">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="ac7ab-126">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="ac7ab-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="ac7ab-127">Vedere [ASP.NET Core per .NET Framework](xref:index#target-framework) per informazioni sul supporto per ASP.NET Core 2.x in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="ac7ab-128">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="ac7ab-129">[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="ac7ab-130">Siti Web</span><span class="sxs-lookup"><span data-stu-id="ac7ab-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="ac7ab-131">API</span><span class="sxs-lookup"><span data-stu-id="ac7ab-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="ac7ab-132">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="ac7ab-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="ac7ab-133">Distribuire un'app ASP.NET Core in Azure</span><span class="sxs-lookup"><span data-stu-id="ac7ab-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="ac7ab-134">Scenari ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="ac7ab-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="ac7ab-135">Siti Web</span><span class="sxs-lookup"><span data-stu-id="ac7ab-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="ac7ab-136">API</span><span class="sxs-lookup"><span data-stu-id="ac7ab-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="ac7ab-137">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="ac7ab-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="ac7ab-138">Creare un'app Web ASP.NET 4.x in Azure</span><span class="sxs-lookup"><span data-stu-id="ac7ab-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="ac7ab-139">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ac7ab-139">Additional resources</span></span>

* [<span data-ttu-id="ac7ab-140">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ac7ab-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="ac7ab-141">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac7ab-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> <span data-ttu-id="ac7ab-142">È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac7ab-142">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="ac7ab-143">Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="ac7ab-143">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>