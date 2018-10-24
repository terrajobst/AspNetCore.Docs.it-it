---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 3bb86fa255548ff66592ac14c1020e0c6b47959c
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391157"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="c79c0-103">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79c0-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="c79c0-104">Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="c79c0-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="c79c0-105">ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="c79c0-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="c79c0-106">Con ASP.NET Core, è possibile:</span><span class="sxs-lookup"><span data-stu-id="c79c0-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="c79c0-107">Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="c79c0-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="c79c0-108">Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="c79c0-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="c79c0-109">Distribuire nel cloud o in locale.</span><span class="sxs-lookup"><span data-stu-id="c79c0-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="c79c0-110">Eseguire in [.NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="c79c0-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="c79c0-111">Perché usare ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="c79c0-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="c79c0-112">Milioni di sviluppatori hanno usato, e continuano a usare, [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) per creare app Web.</span><span class="sxs-lookup"><span data-stu-id="c79c0-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="c79c0-113">ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.</span><span class="sxs-lookup"><span data-stu-id="c79c0-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="c79c0-114">Compilare API web e interfaccia utente web tramite ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c79c0-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="c79c0-115">ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/index#build-web-apis) e [app Web](xref:tutorials/index#build-web-apps):</span><span class="sxs-lookup"><span data-stu-id="c79c0-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="c79c0-116">Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web [testabili](xref:test/index).</span><span class="sxs-lookup"><span data-stu-id="c79c0-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="c79c0-117">[Le pagine Razor](xref:razor-pages/index) (una novità di ASP.NET Core 2.0) sono un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.</span><span class="sxs-lookup"><span data-stu-id="c79c0-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="c79c0-118">[Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="c79c0-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="c79c0-119">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="c79c0-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="c79c0-120">Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="c79c0-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="c79c0-121">L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c79c0-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="c79c0-122">La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c79c0-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="c79c0-123">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="c79c0-123">Client-side development</span></span>

<span data-ttu-id="c79c0-124">ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="c79c0-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="c79c0-125">Per altre informazioni, vedere [Sviluppo lato client](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="c79c0-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="c79c0-126">ASP.NET Core per .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c79c0-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="c79c0-127">ASP.NET Core può avere come destinazione .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c79c0-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="c79c0-128">Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="c79c0-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="c79c0-129">Non è prevista la rimozione del supporto di ASP.NET Core per .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c79c0-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="c79c0-130">ASP.NET Core è in genere costituito da librerie [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="c79c0-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="c79c0-131">Le app scritte con .NET 2.0 Standard funzionano ovunque sia supportato .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="c79c0-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="c79c0-132">ASP.NET Core 2.x è supportato nelle versioni di .NET Framework compatibili con .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="c79c0-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="c79c0-133">È consigliabile usare .NET Framework 4.7.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c79c0-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="c79c0-134">.NET Framework 4.6.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c79c0-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="c79c0-135">Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione.</span><span class="sxs-lookup"><span data-stu-id="c79c0-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="c79c0-136">Alcuni vantaggi di .NET Core in .NET Framework sono:</span><span class="sxs-lookup"><span data-stu-id="c79c0-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="c79c0-137">Funzionamento multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c79c0-137">Cross-platform.</span></span> <span data-ttu-id="c79c0-138">Esecuzione con macOS, Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="c79c0-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="c79c0-139">Miglioramento delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="c79c0-139">Improved performance</span></span>
* <span data-ttu-id="c79c0-140">Controllo delle versioni side-by-side</span><span class="sxs-lookup"><span data-stu-id="c79c0-140">Side-by-side versioning</span></span>
* <span data-ttu-id="c79c0-141">Nuove API</span><span class="sxs-lookup"><span data-stu-id="c79c0-141">New APIs</span></span>
* <span data-ttu-id="c79c0-142">Open source</span><span class="sxs-lookup"><span data-stu-id="c79c0-142">Open source</span></span>

<span data-ttu-id="c79c0-143">È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API.</span><span class="sxs-lookup"><span data-stu-id="c79c0-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="c79c0-144">[Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows.</span><span class="sxs-lookup"><span data-stu-id="c79c0-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="c79c0-145">Queste API non erano disponibili in .NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="c79c0-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c79c0-146">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c79c0-146">Next steps</span></span>

<span data-ttu-id="c79c0-147">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="c79c0-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c79c0-148">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c79c0-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="c79c0-149">Esercitazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79c0-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="c79c0-150">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79c0-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="c79c0-151">[La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team.</span><span class="sxs-lookup"><span data-stu-id="c79c0-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="c79c0-152">Vengono segnalati nuovi blog e software di terze parti.</span><span class="sxs-lookup"><span data-stu-id="c79c0-152">It features new blogs and third-party software.</span></span>
