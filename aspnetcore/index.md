---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: index
ms.openlocfilehash: 1699acc0086dfd50c573afc239bc8f37eb9e7af9
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569988"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="bebdc-103">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bebdc-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="bebdc-104">Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="bebdc-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="bebdc-105">ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet.</span><span class="sxs-lookup"><span data-stu-id="bebdc-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="bebdc-106">Con ASP.NET Core, è possibile:</span><span class="sxs-lookup"><span data-stu-id="bebdc-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="bebdc-107">Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="bebdc-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="bebdc-108">Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="bebdc-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="bebdc-109">Distribuire nel cloud o in locale.</span><span class="sxs-lookup"><span data-stu-id="bebdc-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="bebdc-110">Eseguire in [.NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="bebdc-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="bebdc-111">Perché usare ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="bebdc-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="bebdc-112">Milioni di sviluppatori hanno usato, e continuano a usare, [ASP.NET 4.x](/aspnet/overview) per creare app Web.</span><span class="sxs-lookup"><span data-stu-id="bebdc-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="bebdc-113">ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.</span><span class="sxs-lookup"><span data-stu-id="bebdc-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="bebdc-114">Compilare API web e interfaccia utente web tramite ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="bebdc-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="bebdc-115">ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/first-web-api) e [app Web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="bebdc-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="bebdc-116">Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web testabili.</span><span class="sxs-lookup"><span data-stu-id="bebdc-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="bebdc-117">[Le pagine Razor](xref:razor-pages/index) (una novità di ASP.NET Core 2.0) sono un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.</span><span class="sxs-lookup"><span data-stu-id="bebdc-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="bebdc-118">[Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="bebdc-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="bebdc-119">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="bebdc-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="bebdc-120">Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="bebdc-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="bebdc-121">L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="bebdc-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="bebdc-122">La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.</span><span class="sxs-lookup"><span data-stu-id="bebdc-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="bebdc-123">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="bebdc-123">Client-side development</span></span>

<span data-ttu-id="bebdc-124">ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="bebdc-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="bebdc-125">Per altre informazioni, vedere [Sviluppo lato client](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="bebdc-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="bebdc-126">ASP.NET Core per .NET Framework</span><span class="sxs-lookup"><span data-stu-id="bebdc-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="bebdc-127">ASP.NET Core 2.x può avere come destinazione .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bebdc-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="bebdc-128">Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="bebdc-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="bebdc-129">ASP.NET Core 2.x è costituito a livello generale da librerie [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="bebdc-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="bebdc-130">Le app scritte con .NET 2.0 Standard funzionano ovunque sia supportato .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="bebdc-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="bebdc-131">ASP.NET Core 2.x è supportato nelle versioni di .NET Framework compatibili con .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="bebdc-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="bebdc-132">È consigliabile usare .NET Framework 4.7.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bebdc-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="bebdc-133">.NET Framework 4.6.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bebdc-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="bebdc-134">L'esecuzione di ASP.NET Core 3.0 e versioni successive sarà consentita solo in .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bebdc-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="bebdc-135">Per altri dettagli riguardanti questa modifica, vedere [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Una prima occhiata alle modifiche previste per ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="bebdc-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="bebdc-136">Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione.</span><span class="sxs-lookup"><span data-stu-id="bebdc-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="bebdc-137">Alcuni vantaggi di .NET Core in .NET Framework sono:</span><span class="sxs-lookup"><span data-stu-id="bebdc-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="bebdc-138">Funzionamento multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="bebdc-138">Cross-platform.</span></span> <span data-ttu-id="bebdc-139">Esecuzione con macOS, Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="bebdc-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="bebdc-140">Miglioramento delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="bebdc-140">Improved performance</span></span>
* <span data-ttu-id="bebdc-141">Controllo delle versioni side-by-side</span><span class="sxs-lookup"><span data-stu-id="bebdc-141">Side-by-side versioning</span></span>
* <span data-ttu-id="bebdc-142">Nuove API</span><span class="sxs-lookup"><span data-stu-id="bebdc-142">New APIs</span></span>
* <span data-ttu-id="bebdc-143">Open source</span><span class="sxs-lookup"><span data-stu-id="bebdc-143">Open source</span></span>

<span data-ttu-id="bebdc-144">È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API.</span><span class="sxs-lookup"><span data-stu-id="bebdc-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="bebdc-145">[Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows.</span><span class="sxs-lookup"><span data-stu-id="bebdc-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="bebdc-146">Queste API non erano disponibili in .NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="bebdc-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="bebdc-147">Come scaricare un esempio</span><span class="sxs-lookup"><span data-stu-id="bebdc-147">How to download a sample</span></span>

<span data-ttu-id="bebdc-148">Molti articoli ed esercitazioni includono collegamenti al codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="bebdc-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="bebdc-149">[Scaricare il file ZIP del repository ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="bebdc-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="bebdc-150">Decomprimere il file *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="bebdc-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="bebdc-151">Usare l'URL nel collegamento di esempio per passare alla directory di esempio.</span><span class="sxs-lookup"><span data-stu-id="bebdc-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

<span data-ttu-id="bebdc-152">Per illustrare più scenari, le app di esempio usano le istruzioni `#define` e `#if-#else/#elif-#endif` C# per compilare in modo selettivo ed eseguire sezioni diverse del codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="bebdc-152">To demonstrate multiple scenarios, sample apps make use of the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="bebdc-153">Per gli esempi che usano questo approccio, impostare l'istruzione `#define` nella parte superiore dei file C# sul simbolo associato allo scenario che si vuole eseguire.</span><span class="sxs-lookup"><span data-stu-id="bebdc-153">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="bebdc-154">Un esempio può richiedere l'impostazione del simbolo nella parte superiore di più file per eseguire uno scenario.</span><span class="sxs-lookup"><span data-stu-id="bebdc-154">A sample may require you to set the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="bebdc-155">Ad esempio, l'elenco di simboli `#define` seguente indica che sono disponibili quattro scenari, ovvero uno scenario per simbolo.</span><span class="sxs-lookup"><span data-stu-id="bebdc-155">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="bebdc-156">La configurazione di esempio corrente esegue lo scenario `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="bebdc-156">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="bebdc-157">Per modificare l'esempio in modo che venga eseguito lo scenario `ExpandDefault`, definire il simbolo `ExpandDefault` e lasciare i simboli rimanenti con commento:</span><span class="sxs-lookup"><span data-stu-id="bebdc-157">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="bebdc-158">Per altre informazioni sull'uso delle [direttive del preprocessore C#](/dotnet/csharp/language-reference/preprocessor-directives/) per compilare in modo selettivo le sezioni di codice, vedere [#define (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="bebdc-158">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bebdc-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bebdc-159">Next steps</span></span>

<span data-ttu-id="bebdc-160">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="bebdc-160">For more information, see the following resources:</span></span>

* [<span data-ttu-id="bebdc-161">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bebdc-161">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="bebdc-162">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bebdc-162">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="bebdc-163">[La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team.</span><span class="sxs-lookup"><span data-stu-id="bebdc-163">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="bebdc-164">Vengono segnalati nuovi blog e software di terze parti.</span><span class="sxs-lookup"><span data-stu-id="bebdc-164">It features new blogs and third-party software.</span></span>
