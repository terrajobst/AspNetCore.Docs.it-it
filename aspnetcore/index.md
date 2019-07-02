---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: index
ms.openlocfilehash: ec2b5b72eeff21d183397284b0d538681282d45b
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399157"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="e6cd1-103">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6cd1-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="e6cd1-104">Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="e6cd1-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="e6cd1-105">ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="e6cd1-106">Con ASP.NET Core, è possibile:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="e6cd1-107">Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="e6cd1-108">Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e6cd1-109">Distribuire nel cloud o in locale.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="e6cd1-110">Eseguire in [.NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="e6cd1-111">Perché scegliere ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="e6cd1-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="e6cd1-112">Milioni di sviluppatori hanno usato, e continuano a usare, [ASP.NET 4.x](/aspnet/overview) per creare app Web.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="e6cd1-113">ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="e6cd1-114">Compilare API web e interfaccia utente web tramite ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e6cd1-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="e6cd1-115">ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/first-web-api) e [app Web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="e6cd1-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="e6cd1-116">Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web testabili.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="e6cd1-117">[Razor Pages](xref:razor-pages/index) è un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="e6cd1-118">[Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="e6cd1-119">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="e6cd1-120">Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="e6cd1-121">L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="e6cd1-122">La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="e6cd1-123">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="e6cd1-123">Client-side development</span></span>

<span data-ttu-id="e6cd1-124">ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="e6cd1-125">Per altre informazioni, vedere <xref:blazor/index> e gli argomenti correlati in *Sviluppo lato client*.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="e6cd1-126">ASP.NET Core per .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e6cd1-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="e6cd1-127">ASP.NET Core 2.x può avere come destinazione .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="e6cd1-128">Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="e6cd1-129">ASP.NET Core 2.x è costituito a livello generale da librerie [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="e6cd1-130">Le librerie scritte con .NET Standard 2.0 supportano l'esecuzione su [qualsiasi piattaforma .NET che implementa .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="e6cd1-131">ASP.NET Core 2.x è supportato nelle versioni di .NET Framework che implementano .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="e6cd1-132">È caldamente consigliata la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="e6cd1-133">.NET Framework 4.6.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="e6cd1-134">L'esecuzione di ASP.NET Core 3.0 e versioni successive sarà consentita solo in .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="e6cd1-135">Per altri dettagli riguardanti questa modifica, vedere [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Una prima occhiata alle modifiche previste per ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="e6cd1-136">Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="e6cd1-137">Alcuni vantaggi di .NET Core in .NET Framework sono:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="e6cd1-138">Funzionamento multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-138">Cross-platform.</span></span> <span data-ttu-id="e6cd1-139">Esecuzione con macOS, Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="e6cd1-140">Miglioramento delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="e6cd1-140">Improved performance</span></span>
* <span data-ttu-id="e6cd1-141">Controllo delle versioni side-by-side</span><span class="sxs-lookup"><span data-stu-id="e6cd1-141">Side-by-side versioning</span></span>
* <span data-ttu-id="e6cd1-142">Nuove API</span><span class="sxs-lookup"><span data-stu-id="e6cd1-142">New APIs</span></span>
* <span data-ttu-id="e6cd1-143">Open source</span><span class="sxs-lookup"><span data-stu-id="e6cd1-143">Open source</span></span>

<span data-ttu-id="e6cd1-144">È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="e6cd1-145">[Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="e6cd1-146">Queste API non erano disponibili in .NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="e6cd1-147">Percorso di apprendimento consigliato</span><span class="sxs-lookup"><span data-stu-id="e6cd1-147">Recommended learning path</span></span>

<span data-ttu-id="e6cd1-148">Per un'introduzione allo sviluppo delle app ASP.NET Core, è consigliabile eseguire la sequenza di esercitazioni e articoli seguente:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="e6cd1-149">Eseguire un'esercitazione relativa al tipo di app che si vuole sviluppare o gestire:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="e6cd1-150">Tipo di app</span><span class="sxs-lookup"><span data-stu-id="e6cd1-150">App type</span></span>  |<span data-ttu-id="e6cd1-151">Scenario</span><span class="sxs-lookup"><span data-stu-id="e6cd1-151">Scenario</span></span>  |<span data-ttu-id="e6cd1-152">Esercitazione</span><span class="sxs-lookup"><span data-stu-id="e6cd1-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="e6cd1-153">App Web</span><span class="sxs-lookup"><span data-stu-id="e6cd1-153">Web app</span></span>       | <span data-ttu-id="e6cd1-154">Per un nuovo sviluppo</span><span class="sxs-lookup"><span data-stu-id="e6cd1-154">For new development</span></span>        |[<span data-ttu-id="e6cd1-155">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e6cd1-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="e6cd1-156">App Web</span><span class="sxs-lookup"><span data-stu-id="e6cd1-156">Web app</span></span>       | <span data-ttu-id="e6cd1-157">Per gestire un'app MVC</span><span class="sxs-lookup"><span data-stu-id="e6cd1-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="e6cd1-158">Introduzione a MVC</span><span class="sxs-lookup"><span data-stu-id="e6cd1-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="e6cd1-159">API Web</span><span class="sxs-lookup"><span data-stu-id="e6cd1-159">Web API</span></span>       |                            |<span data-ttu-id="e6cd1-160">[Creare un'API Web](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="e6cd1-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="e6cd1-161">App in tempo reale</span><span class="sxs-lookup"><span data-stu-id="e6cd1-161">Real-time app</span></span> |                            |[<span data-ttu-id="e6cd1-162">Introduzione a SignalR</span><span class="sxs-lookup"><span data-stu-id="e6cd1-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="e6cd1-163">Eseguire un'esercitazione che illustra la procedura di accesso ai dati di base:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-163">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="e6cd1-164">Scenario</span><span class="sxs-lookup"><span data-stu-id="e6cd1-164">Scenario</span></span>  |<span data-ttu-id="e6cd1-165">Esercitazione</span><span class="sxs-lookup"><span data-stu-id="e6cd1-165">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="e6cd1-166">Per un nuovo sviluppo</span><span class="sxs-lookup"><span data-stu-id="e6cd1-166">For new development</span></span>        |[<span data-ttu-id="e6cd1-167">Razor Pages con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e6cd1-167">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="e6cd1-168">Per gestire un'app MVC</span><span class="sxs-lookup"><span data-stu-id="e6cd1-168">For maintaining an MVC app</span></span> |[<span data-ttu-id="e6cd1-169">MVC con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e6cd1-169">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="e6cd1-170">Leggere una panoramica delle funzionalità di ASP.NET Core applicabili a tutti i tipi di app:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-170">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="e6cd1-171">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="e6cd1-171">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="e6cd1-172">Esplorare il Sommario per cercare altri argomenti di interesse.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-172">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="e6cd1-173">\* È disponibile una nuova [esercitazione sulle API Web da eseguire interamente nel browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core) che non richiede alcuna installazione nell'IDE locale.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-173">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="e6cd1-174">Il codice viene eseguito in un'[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) e per il testing viene usato [curl](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-174">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="e6cd1-175">Come scaricare un esempio</span><span class="sxs-lookup"><span data-stu-id="e6cd1-175">How to download a sample</span></span>

<span data-ttu-id="e6cd1-176">Molti articoli ed esercitazioni includono collegamenti al codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-176">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="e6cd1-177">[Scaricare il file ZIP del repository ASP.NET](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-177">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="e6cd1-178">Decomprimere il file *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-178">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="e6cd1-179">Usare l'URL nel collegamento di esempio per passare alla directory di esempio.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-179">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="e6cd1-180">Direttive per il preprocessore nel codice di esempio</span><span class="sxs-lookup"><span data-stu-id="e6cd1-180">Preprocessor directives in sample code</span></span>

<span data-ttu-id="e6cd1-181">Per illustrare più scenari, le app di esempio usano le istruzioni C# `#define` e `#if-#else/#elif-#endif` per compilare in modo selettivo ed eseguire sezioni diverse del codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-181">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="e6cd1-182">Per gli esempi che usano questo approccio, impostare l'istruzione `#define` nella parte superiore dei file C# sul simbolo associato allo scenario che si vuole eseguire.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-182">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="e6cd1-183">Alcuni esempi richiedono l'impostazione del simbolo nella parte superiore di più file per eseguire uno scenario.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-183">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="e6cd1-184">Ad esempio, l'elenco di simboli `#define` seguente indica che sono disponibili quattro scenari, ovvero uno scenario per simbolo.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-184">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="e6cd1-185">La configurazione di esempio corrente esegue lo scenario `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-185">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="e6cd1-186">Per modificare l'esempio in modo che venga eseguito lo scenario `ExpandDefault`, definire il simbolo `ExpandDefault` e lasciare i simboli rimanenti con commento:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-186">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="e6cd1-187">Per altre informazioni sull'uso delle [direttive del preprocessore C#](/dotnet/csharp/language-reference/preprocessor-directives/) per compilare in modo selettivo le sezioni di codice, vedere [#define (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-187">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="e6cd1-188">Aree del codice di esempio</span><span class="sxs-lookup"><span data-stu-id="e6cd1-188">Regions in sample code</span></span>

<span data-ttu-id="e6cd1-189">Alcune app di esempio contengono sezioni di codice racchiuse tra le istruzioni C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) e [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-189">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="e6cd1-190">Il sistema di compilazione della documentazione inserisce queste aree negli argomenti della documentazione visualizzabile.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-190">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="e6cd1-191">I nomi delle aree in genere contengono la parola "frammento".</span><span class="sxs-lookup"><span data-stu-id="e6cd1-191">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="e6cd1-192">L'esempio seguente mostra un'area denominata `snippet_FilterInCode`:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-192">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="e6cd1-193">Il file markdown dell'argomento fa riferimento al frammento di codice C# precedente con la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-193">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="e6cd1-194">È possibile ignorare o rimuovere le istruzioni `#region` e `#endregion` che racchiudono il codice.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-194">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="e6cd1-195">Non modificare il codice all'interno di queste istruzioni se si prevede di eseguire gli scenari di esempio descritti nell'argomento.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-195">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="e6cd1-196">È possibile modificare il codice durante la sperimentazione con altri scenari.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-196">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="e6cd1-197">Per altre informazioni, vedere [Contribuire alla documentazione ASP.NET: Frammenti di codice](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="e6cd1-197">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6cd1-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e6cd1-198">Next steps</span></span>

<span data-ttu-id="e6cd1-199">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="e6cd1-199">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="e6cd1-200">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6cd1-200">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="e6cd1-201">[La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-201">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="e6cd1-202">Vengono segnalati nuovi blog e software di terze parti.</span><span class="sxs-lookup"><span data-stu-id="e6cd1-202">It features new blogs and third-party software.</span></span>
