---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: fd7fa9dd70502f51222e457dd887ef668d377278
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366659"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="4cdf8-103">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cdf8-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="4cdf8-104">Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="4cdf8-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="4cdf8-105">ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="4cdf8-106">Con ASP.NET Core, è possibile:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="4cdf8-107">Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="4cdf8-108">Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="4cdf8-109">Distribuire nel cloud o in locale.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="4cdf8-110">Eseguire in [.NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="4cdf8-111">Perché scegliere ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="4cdf8-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="4cdf8-112">Milioni di sviluppatori usano o hanno usato [ASP.NET 4. x](/aspnet/overview) per creare app Web.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="4cdf8-113">ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="4cdf8-114">Compilare API web e interfaccia utente web tramite ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4cdf8-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="4cdf8-115">ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/first-web-api) e [app Web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="4cdf8-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="4cdf8-116">Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web testabili.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="4cdf8-117">[Razor Pages](xref:razor-pages/index) è un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="4cdf8-118">[Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="4cdf8-119">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="4cdf8-120">Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="4cdf8-121">L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="4cdf8-122">La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="4cdf8-123">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="4cdf8-123">Client-side development</span></span>

<span data-ttu-id="4cdf8-124">ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="4cdf8-125">Per altre informazioni, vedere <xref:blazor/index> e gli argomenti correlati in *Sviluppo lato client*.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="4cdf8-126">ASP.NET Core per .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4cdf8-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="4cdf8-127">ASP.NET Core 2.x può avere come destinazione .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="4cdf8-128">Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="4cdf8-129">ASP.NET Core 2.x è costituito a livello generale da librerie [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="4cdf8-130">Le librerie scritte con .NET Standard 2.0 supportano l'esecuzione su [qualsiasi piattaforma .NET che implementa .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="4cdf8-131">ASP.NET Core 2.x è supportato nelle versioni di .NET Framework che implementano .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="4cdf8-132">È caldamente consigliata la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="4cdf8-133">.NET Framework 4.6.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="4cdf8-134">L'esecuzione di ASP.NET Core 3.0 e versioni successive sarà consentita solo in .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="4cdf8-135">Per altri dettagli riguardanti questa modifica, vedere [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Una prima occhiata alle modifiche previste per ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="4cdf8-136">Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="4cdf8-137">Alcuni vantaggi di .NET Core in .NET Framework sono:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="4cdf8-138">Funzionamento multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-138">Cross-platform.</span></span> <span data-ttu-id="4cdf8-139">Esecuzione con macOS, Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="4cdf8-140">Prestazioni più elevate</span><span class="sxs-lookup"><span data-stu-id="4cdf8-140">Improved performance</span></span>
* [<span data-ttu-id="4cdf8-141">Controllo delle versioni affiancato</span><span class="sxs-lookup"><span data-stu-id="4cdf8-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="4cdf8-142">Nuove API</span><span class="sxs-lookup"><span data-stu-id="4cdf8-142">New APIs</span></span>
* <span data-ttu-id="4cdf8-143">Aprire origine</span><span class="sxs-lookup"><span data-stu-id="4cdf8-143">Open source</span></span>

<span data-ttu-id="4cdf8-144">È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="4cdf8-145">[Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="4cdf8-146">Queste API non erano disponibili in .NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="4cdf8-147">Percorso di apprendimento consigliato</span><span class="sxs-lookup"><span data-stu-id="4cdf8-147">Recommended learning path</span></span>

<span data-ttu-id="4cdf8-148">Per un'introduzione allo sviluppo delle app ASP.NET Core, è consigliabile eseguire la sequenza di esercitazioni e articoli seguente:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="4cdf8-149">Eseguire un'esercitazione relativa al tipo di app che si vuole sviluppare o gestire:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="4cdf8-150">Tipo di app</span><span class="sxs-lookup"><span data-stu-id="4cdf8-150">App type</span></span>  |<span data-ttu-id="4cdf8-151">Scenario</span><span class="sxs-lookup"><span data-stu-id="4cdf8-151">Scenario</span></span>  |<span data-ttu-id="4cdf8-152">Esercitazione</span><span class="sxs-lookup"><span data-stu-id="4cdf8-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="4cdf8-153">app Web</span><span class="sxs-lookup"><span data-stu-id="4cdf8-153">Web app</span></span>                   | <span data-ttu-id="4cdf8-154">Per un nuovo sviluppo</span><span class="sxs-lookup"><span data-stu-id="4cdf8-154">For new development</span></span>        |[<span data-ttu-id="4cdf8-155">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4cdf8-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="4cdf8-156">app Web</span><span class="sxs-lookup"><span data-stu-id="4cdf8-156">Web app</span></span>                   | <span data-ttu-id="4cdf8-157">Per gestire un'app MVC</span><span class="sxs-lookup"><span data-stu-id="4cdf8-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="4cdf8-158">Introduzione a MVC</span><span class="sxs-lookup"><span data-stu-id="4cdf8-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="4cdf8-159">API Web</span><span class="sxs-lookup"><span data-stu-id="4cdf8-159">Web API</span></span>                   |                            |<span data-ttu-id="4cdf8-160">[Creare un'API Web](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="4cdf8-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="4cdf8-161">App in tempo reale</span><span class="sxs-lookup"><span data-stu-id="4cdf8-161">Real-time app</span></span>             |                            |[<span data-ttu-id="4cdf8-162">Introduzione a SignalR</span><span class="sxs-lookup"><span data-stu-id="4cdf8-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="4cdf8-163">App Blazer</span><span class="sxs-lookup"><span data-stu-id="4cdf8-163">Blazor app</span></span>                |                            |[<span data-ttu-id="4cdf8-164">Introduzione a blazer</span><span class="sxs-lookup"><span data-stu-id="4cdf8-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="4cdf8-165">App RPC (Remote Procedure Call)</span><span class="sxs-lookup"><span data-stu-id="4cdf8-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="4cdf8-166">Introduzione a un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="4cdf8-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="4cdf8-167">Eseguire un'esercitazione che illustra la procedura di accesso ai dati di base:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="4cdf8-168">Scenario</span><span class="sxs-lookup"><span data-stu-id="4cdf8-168">Scenario</span></span>  |<span data-ttu-id="4cdf8-169">Esercitazione</span><span class="sxs-lookup"><span data-stu-id="4cdf8-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="4cdf8-170">Per un nuovo sviluppo</span><span class="sxs-lookup"><span data-stu-id="4cdf8-170">For new development</span></span>        |[<span data-ttu-id="4cdf8-171">Razor Pages con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4cdf8-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="4cdf8-172">Per gestire un'app MVC</span><span class="sxs-lookup"><span data-stu-id="4cdf8-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="4cdf8-173">MVC con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4cdf8-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="4cdf8-174">Leggere una panoramica delle funzionalità di ASP.NET Core applicabili a tutti i tipi di app:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="4cdf8-175">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="4cdf8-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="4cdf8-176">Esplorare il Sommario per cercare altri argomenti di interesse.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="4cdf8-177">\* È disponibile una nuova [esercitazione sulle API Web da eseguire interamente nel browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core) che non richiede alcuna installazione nell'IDE locale.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="4cdf8-178">Il codice viene eseguito in un'[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) e per il testing viene usato [curl](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="4cdf8-179">Migrazione dalla .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4cdf8-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="4cdf8-180">Per una guida di riferimento alla migrazione delle app ASP.NET ai ASP.NET Core, vedere <xref:migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="4cdf8-181">Come scaricare un esempio</span><span class="sxs-lookup"><span data-stu-id="4cdf8-181">How to download a sample</span></span>

<span data-ttu-id="4cdf8-182">Molti articoli ed esercitazioni includono collegamenti al codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="4cdf8-183">[Scaricare il file ZIP del repository ASP.NET](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-183">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="4cdf8-184">Decomprimere il file *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="4cdf8-185">Usare l'URL nel collegamento di esempio per passare alla directory di esempio.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="4cdf8-186">Direttive per il preprocessore nel codice di esempio</span><span class="sxs-lookup"><span data-stu-id="4cdf8-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="4cdf8-187">Per illustrare più scenari, le app di esempio usano le direttive per il preprocessore `#define` e `#if-#else/#elif-#endif` per compilare ed eseguire in modo selettivo sezioni diverse del codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="4cdf8-188">Per gli esempi che usano questo approccio, impostare la direttiva `#define` nella parte superiore dei C# file per definire il simbolo associato allo scenario che si vuole eseguire.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="4cdf8-189">Alcuni esempi richiedono la definizione del simbolo nella parte superiore di più file per eseguire uno scenario.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="4cdf8-190">Ad esempio, l'elenco di simboli `#define` seguente indica che sono disponibili quattro scenari, ovvero uno scenario per simbolo.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="4cdf8-191">La configurazione di esempio corrente esegue lo scenario `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="4cdf8-192">Per modificare l'esempio in modo che venga eseguito lo scenario `ExpandDefault`, definire il simbolo `ExpandDefault` e lasciare i simboli rimanenti con commento:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="4cdf8-193">Per altre informazioni sull'uso delle [direttive del preprocessore C#](/dotnet/csharp/language-reference/preprocessor-directives/) per compilare in modo selettivo le sezioni di codice, vedere [#define (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="4cdf8-194">Aree del codice di esempio</span><span class="sxs-lookup"><span data-stu-id="4cdf8-194">Regions in sample code</span></span>

<span data-ttu-id="4cdf8-195">Alcune app di esempio contengono sezioni di codice racchiuse tra [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) e [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# direttive.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="4cdf8-196">Il sistema di compilazione della documentazione inserisce queste aree negli argomenti della documentazione visualizzabile.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="4cdf8-197">I nomi delle aree in genere contengono la parola "frammento".</span><span class="sxs-lookup"><span data-stu-id="4cdf8-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="4cdf8-198">L'esempio seguente mostra un'area denominata `snippet_WebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="4cdf8-199">Il file markdown dell'argomento fa riferimento al frammento di codice C# precedente con la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="4cdf8-200">È possibile ignorare o rimuovere in modo sicuro le direttive `#region` e `#endregion` che racchiudono il codice.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="4cdf8-201">Non modificare il codice all'interno di queste direttive se si prevede di eseguire gli scenari di esempio descritti nell'argomento.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="4cdf8-202">È possibile modificare il codice durante la sperimentazione con altri scenari.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="4cdf8-203">Per altre informazioni, vedere [Contribuire alla documentazione ASP.NET: Frammenti di codice](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="4cdf8-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cdf8-204">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4cdf8-204">Next steps</span></span>

<span data-ttu-id="4cdf8-205">Per ulteriori informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="4cdf8-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="4cdf8-206">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cdf8-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="4cdf8-207">[La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="4cdf8-208">Vengono segnalati nuovi blog e software di terze parti.</span><span class="sxs-lookup"><span data-stu-id="4cdf8-208">It features new blogs and third-party software.</span></span>
