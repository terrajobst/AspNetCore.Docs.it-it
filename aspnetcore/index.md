---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Offre un'introduzione ad ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="42ec6-104">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42ec6-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="42ec6-105">Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="42ec6-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="42ec6-106">ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="42ec6-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="42ec6-107">Con ASP.NET Core, è possibile:</span><span class="sxs-lookup"><span data-stu-id="42ec6-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="42ec6-108">Compilare app web e servizi, app [IoT](https://www.microsoft.com/en-us/internet-of-things/) e back-end per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="42ec6-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="42ec6-109">Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="42ec6-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="42ec6-110">Distribuisci sul cloud o in locale</span><span class="sxs-lookup"><span data-stu-id="42ec6-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="42ec6-111">Eseguire in [.NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="42ec6-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="42ec6-112">Perché usare ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="42ec6-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="42ec6-113">Milioni di sviluppatori hanno usato, e continuano a usare, ASP.NET per creare app Web.</span><span class="sxs-lookup"><span data-stu-id="42ec6-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="42ec6-114">ASP.NET Core è una riprogettazione di ASP.NET, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.</span><span class="sxs-lookup"><span data-stu-id="42ec6-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="42ec6-115">ASP.NET Core offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="42ec6-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="42ec6-116">Una storia unificata per la compilazione dell'interfaccia utente web e delle API web.</span><span class="sxs-lookup"><span data-stu-id="42ec6-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="42ec6-117">Integrazione di [moderni framework lato client](xref:client-side/index) e di flussi di lavoro di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="42ec6-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="42ec6-118">Un [sistema di configurazione](xref:fundamentals/configuration/index) basato sull'ambiente, pronto per il cloud.</span><span class="sxs-lookup"><span data-stu-id="42ec6-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="42ec6-119">[Inserimento delle dipendenze](xref:fundamentals/dependency-injection) incorporato.</span><span class="sxs-lookup"><span data-stu-id="42ec6-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="42ec6-120">Una pipeline di richiesta HTTP leggera, ad alte prestazioni e modulare.</span><span class="sxs-lookup"><span data-stu-id="42ec6-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="42ec6-121">Capacità di eseguire l'host in IIS o il self-host nel processo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="42ec6-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="42ec6-122">È possibile eseguire in [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), che supporta la versione del'app side-by-side impostata su true.</span><span class="sxs-lookup"><span data-stu-id="42ec6-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="42ec6-123">Gli strumenti che semplificano lo sviluppo del web moderno.</span><span class="sxs-lookup"><span data-stu-id="42ec6-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="42ec6-124">Possibilità di compilare ed eseguire in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="42ec6-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="42ec6-125">Focalizzati su risorse open source e sulle community.</span><span class="sxs-lookup"><span data-stu-id="42ec6-125">Open-source and community-focused.</span></span>

<span data-ttu-id="42ec6-126">ASP.NET Core viene fornito esclusivamente come pacchetti [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="42ec6-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="42ec6-127">Ciò consente di ottimizzare l'app per includere i pacchetti NuGet che sono necessari.</span><span class="sxs-lookup"><span data-stu-id="42ec6-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="42ec6-128">Una riduzione della superficie occupata dall'app offre anche diversi vantaggi, tra cui una maggiore sicurezza, una riduzione delle esigenze di assistenza e un miglioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="42ec6-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="42ec6-129">Compilare API web e interfaccia utente web tramite ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="42ec6-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="42ec6-130">ASP.NET Core MVC fornisce delle funzionalità che consentono di compilare [le API web](xref:tutorials/index#building-web-apis) e [le app web](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="42ec6-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="42ec6-131">Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web [testabili](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="42ec6-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="42ec6-132">[Le pagine Razor](xref:mvc/razor-pages/index) (nuove in 2.0) un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.</span><span class="sxs-lookup"><span data-stu-id="42ec6-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="42ec6-133">[La sintassi Razor](xref:mvc/views/razor) fornisce un linguaggio produttivo per le [pagine Razor](xref:mvc/razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="42ec6-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="42ec6-134">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="42ec6-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="42ec6-135">Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](mvc/models/formatting.md) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="42ec6-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="42ec6-136">[Associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="42ec6-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="42ec6-137">[Convalida modello](xref:mvc/models/validation) esegue automaticamente la convalida del lato server e del lato client.</span><span class="sxs-lookup"><span data-stu-id="42ec6-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="42ec6-138">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="42ec6-138">Client-side development</span></span>

<span data-ttu-id="42ec6-139">ASP.NET Core è progettato per integrarsi perfettamente con un'ampia gamma di Framework, sul lato client, ad esempio [AngularJS](xref:client-side/angular), [Knockout.js](xref:client-side/knockout), e [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="42ec6-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="42ec6-140">Vedere [Sviluppo lato client](client-side/index.md) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="42ec6-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42ec6-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="42ec6-141">Next steps</span></span>

<span data-ttu-id="42ec6-142">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="42ec6-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="42ec6-143">Esercitazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42ec6-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="42ec6-144">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42ec6-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="42ec6-145">[L'impostazione settimanale della community di ASP.NET](https://live.asp.net/) copre lo stato di avanzamento del team e i pianifica e caratterizza i nuovi blog e i software di terze parti.</span><span class="sxs-lookup"><span data-stu-id="42ec6-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
