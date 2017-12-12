---
title: Come aggregare e riduzione in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione web ASP.NET Core applicando le tecniche di aggregazione e la riduzione.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="dee66-103">Come aggregare e riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-103">Bundling and minification</span></span>

<span data-ttu-id="dee66-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dee66-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dee66-105">Questo articolo illustra i vantaggi dell'applicazione come aggregare e minimizzazione, incluso come queste funzionalità possono essere utilizzate con le applicazioni web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dee66-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="dee66-106">Che cos'è l'aggregazione e minimizzazione?</span><span class="sxs-lookup"><span data-stu-id="dee66-106">What is bundling and minification?</span></span>

<span data-ttu-id="dee66-107">Creazione di bundle e minimizzazione sono due le ottimizzazioni delle prestazioni di distinct che è possibile applicare in un'app web.</span><span class="sxs-lookup"><span data-stu-id="dee66-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="dee66-108">Utilizzati insieme, aggregazione e minimizzazione migliorare le prestazioni riducendo il numero di richieste di server e ridurre le dimensioni delle risorse statiche richieste.</span><span class="sxs-lookup"><span data-stu-id="dee66-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="dee66-109">Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="dee66-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="dee66-110">Una volta che una pagina web è stato richiesto, il browser memorizza nella cache le risorse statiche (JavaScript, CSS e immagini).</span><span class="sxs-lookup"><span data-stu-id="dee66-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="dee66-111">Di conseguenza, aggregazione e riduzione non migliorare le prestazioni quando si richiede la stessa pagina, o le pagine, nello stesso sito richiede le stesse risorse.</span><span class="sxs-lookup"><span data-stu-id="dee66-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="dee66-112">Se non si imposta la scadenza testata correttamente le risorse, e se non si utilizza come aggregare e riduzione, euristica di aggiornamento del browser contrassegna le risorse non aggiornati dopo alcuni giorni.</span><span class="sxs-lookup"><span data-stu-id="dee66-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="dee66-113">Inoltre, il browser richiede una richiesta di convalida per ogni asset.</span><span class="sxs-lookup"><span data-stu-id="dee66-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="dee66-114">In questo caso, aggregazione e minimizzazione forniscono un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="dee66-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="dee66-115">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="dee66-115">Bundling</span></span>

<span data-ttu-id="dee66-116">Creazione di bundle combina più file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="dee66-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="dee66-117">Creazione di bundle riduce il numero di richieste di server necessari eseguire il rendering di una risorsa web, ad esempio una pagina web.</span><span class="sxs-lookup"><span data-stu-id="dee66-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="dee66-118">È possibile creare un numero qualsiasi di singoli pacchetti in modo specifico per CSS, JavaScript e così via. Un numero inferiore di file significa meno le richieste HTTP dal browser al server o dal servizio fornendo all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="dee66-119">I risultati miglioramento delle prestazioni di caricamento prima pagina.</span><span class="sxs-lookup"><span data-stu-id="dee66-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="dee66-120">Minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dee66-120">Minification</span></span>

<span data-ttu-id="dee66-121">Minimizzazione rimuove i caratteri non necessari dal codice senza modificare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="dee66-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="dee66-122">Il risultato è una riduzione di grandi dimensioni in asset richiesti (ad esempio CSS, immagini e i file JavaScript).</span><span class="sxs-lookup"><span data-stu-id="dee66-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="dee66-123">Gli effetti collaterali di minimizzazione includono abbreviare i nomi delle variabili per un carattere e la rimozione di commenti e gli spazi vuoti non necessari.</span><span class="sxs-lookup"><span data-stu-id="dee66-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="dee66-124">Si consideri la funzione JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="dee66-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="dee66-125">Minimizzazione riduce la funzione per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dee66-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="dee66-126">Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i nomi di parametri e variabili seguenti sono stati rinominati come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dee66-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="dee66-127">Originale</span><span class="sxs-lookup"><span data-stu-id="dee66-127">Original</span></span> | <span data-ttu-id="dee66-128">Ridenominazione</span><span class="sxs-lookup"><span data-stu-id="dee66-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="dee66-129">Impatto di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-129">Impact of bundling and minification</span></span>

<span data-ttu-id="dee66-130">Nella tabella seguente vengono descritte le differenze tra il caricamento delle risorse singolarmente e utilizzando l'aggregazione e la riduzione:</span><span class="sxs-lookup"><span data-stu-id="dee66-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="dee66-131">Azione</span><span class="sxs-lookup"><span data-stu-id="dee66-131">Action</span></span> | <span data-ttu-id="dee66-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="dee66-132">With B/M</span></span> | <span data-ttu-id="dee66-133">Senza B/M</span><span class="sxs-lookup"><span data-stu-id="dee66-133">Without B/M</span></span> | <span data-ttu-id="dee66-134">Modifica</span><span class="sxs-lookup"><span data-stu-id="dee66-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="dee66-135">Richieste di file</span><span class="sxs-lookup"><span data-stu-id="dee66-135">File Requests</span></span>  | <span data-ttu-id="dee66-136">7</span><span class="sxs-lookup"><span data-stu-id="dee66-136">7</span></span>   | <span data-ttu-id="dee66-137">18</span><span class="sxs-lookup"><span data-stu-id="dee66-137">18</span></span>     | <span data-ttu-id="dee66-138">157%</span><span class="sxs-lookup"><span data-stu-id="dee66-138">157%</span></span>
<span data-ttu-id="dee66-139">KB trasferiti</span><span class="sxs-lookup"><span data-stu-id="dee66-139">KB Transferred</span></span> | <span data-ttu-id="dee66-140">156</span><span class="sxs-lookup"><span data-stu-id="dee66-140">156</span></span> | <span data-ttu-id="dee66-141">264.68</span><span class="sxs-lookup"><span data-stu-id="dee66-141">264.68</span></span> | <span data-ttu-id="dee66-142">70%</span><span class="sxs-lookup"><span data-stu-id="dee66-142">70%</span></span>
<span data-ttu-id="dee66-143">Tempo di caricamento (ms)</span><span class="sxs-lookup"><span data-stu-id="dee66-143">Load Time (ms)</span></span> | <span data-ttu-id="dee66-144">885</span><span class="sxs-lookup"><span data-stu-id="dee66-144">885</span></span> | <span data-ttu-id="dee66-145">2360</span><span class="sxs-lookup"><span data-stu-id="dee66-145">2360</span></span>   | <span data-ttu-id="dee66-146">167%</span><span class="sxs-lookup"><span data-stu-id="dee66-146">167%</span></span>

<span data-ttu-id="dee66-147">Browser sono piuttosto dettagliati per quanto riguarda le intestazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dee66-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="dee66-148">I byte totali inviati metrica visto una significativa riduzione durante l'aggregazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="dee66-149">Il tempo di caricamento Mostra un miglioramento significativo, ma in questo esempio viene eseguito localmente.</span><span class="sxs-lookup"><span data-stu-id="dee66-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="dee66-150">Miglioramenti delle prestazioni superiori vengono realizzati quando l'utilizzo di aggregazione e minimizzazione asset trasferiti su una rete.</span><span class="sxs-lookup"><span data-stu-id="dee66-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="dee66-151">Scegliere una strategia di riduzione e di aggregazione</span><span class="sxs-lookup"><span data-stu-id="dee66-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="dee66-152">I modelli di progetto MVC e pagine Razor forniscono una soluzione di casella per la creazione di bundle e minimizzazione costituito da un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="dee66-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="dee66-153">Strumenti di terze parti, ad esempio il [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) attività canali, eseguire le stesse attività con una maggiore complessità.</span><span class="sxs-lookup"><span data-stu-id="dee66-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="dee66-154">Uno strumento di terze parti è un'ottima soluzione quando il flusso di lavoro di sviluppo richiede l'elaborazione oltre l'aggregazione e minimizzazione&mdash;, ad esempio ottimizzazione linting e immagine.</span><span class="sxs-lookup"><span data-stu-id="dee66-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="dee66-155">Usando come aggregare in fase di progettazione e la riduzione, vengono creati i file minimizzati prima della distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dee66-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="dee66-156">Aggiunta e la riduzione prima della distribuzione offre il vantaggio del carico del server ridotto.</span><span class="sxs-lookup"><span data-stu-id="dee66-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="dee66-157">Tuttavia, è importante tenere presente che in fase di progettazione di aggregazione e minimizzazione aumenta la complessità di compilazione e funziona solo con file statici.</span><span class="sxs-lookup"><span data-stu-id="dee66-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="dee66-158">Configurare l'aggregazione e riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-158">Configure bundling and minification</span></span>

<span data-ttu-id="dee66-159">I modelli di progetto MVC e pagine Razor forniscono un *bundleconfig.json* file di configurazione che definisce le opzioni per ogni pacchetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="dee66-160">Per impostazione predefinita, viene modificata una configurazione di pacchetto per il codice JavaScript personalizzato (*wwwroot/js/site.js*) e fogli di stile (*wwwroot/css/site.css*) file:</span><span class="sxs-lookup"><span data-stu-id="dee66-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="dee66-161">Opzioni di raggruppamento includono:</span><span class="sxs-lookup"><span data-stu-id="dee66-161">Bundle options include:</span></span>

* <span data-ttu-id="dee66-162">`outputFileName`: Il nome del file di bundle all'output.</span><span class="sxs-lookup"><span data-stu-id="dee66-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="dee66-163">Può contenere un percorso relativo di *bundleconfig.json* file.</span><span class="sxs-lookup"><span data-stu-id="dee66-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="dee66-164">**Obbligatorio**</span><span class="sxs-lookup"><span data-stu-id="dee66-164">**required**</span></span>
* <span data-ttu-id="dee66-165">`inputFiles`: Una matrice di file da raggruppare.</span><span class="sxs-lookup"><span data-stu-id="dee66-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="dee66-166">Questi sono i percorsi relativi al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="dee66-167">**parametro facoltativo**, * comporta un valore vuoto in un file di output vuoto.</span><span class="sxs-lookup"><span data-stu-id="dee66-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="dee66-168">[il glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.</span><span class="sxs-lookup"><span data-stu-id="dee66-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="dee66-169">`minify`: Le opzioni di riduzione per il tipo di output.</span><span class="sxs-lookup"><span data-stu-id="dee66-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="dee66-170">**parametro facoltativo**, *impostazione predefinita:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="dee66-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="dee66-171">Opzioni di configurazione sono disponibili per ogni tipo di file di output.</span><span class="sxs-lookup"><span data-stu-id="dee66-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="dee66-172">Minimizzatore CSS</span><span class="sxs-lookup"><span data-stu-id="dee66-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="dee66-173">Minimizzatore JavaScript</span><span class="sxs-lookup"><span data-stu-id="dee66-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="dee66-174">Minimizzatore HTML</span><span class="sxs-lookup"><span data-stu-id="dee66-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="dee66-175">`includeInProject`: Flag che indica se aggiungere i file generati file di progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="dee66-176">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="dee66-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="dee66-177">`sourceMap`: Flag che indica se generare una mappa di origine per il file aggregato.</span><span class="sxs-lookup"><span data-stu-id="dee66-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="dee66-178">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="dee66-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="dee66-179">`sourceMapRootPath`: Il percorso radice per l'archiviazione dei file di mappa di origine generato.</span><span class="sxs-lookup"><span data-stu-id="dee66-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="dee66-180">Esecuzione della fase di compilazione di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="dee66-181">Il [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacchetto NuGet consente l'esecuzione dell'aggregazione e riduzione in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="dee66-182">Inserisce il pacchetto [destinazioni di MSBuild](/visualstudio/msbuild/msbuild-targets) che eseguita in compilazione e l'ora pulita.</span><span class="sxs-lookup"><span data-stu-id="dee66-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="dee66-183">Il *bundleconfig.json* file viene analizzato dal processo di compilazione per generare i file di output in base alla configurazione definita.</span><span class="sxs-lookup"><span data-stu-id="dee66-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dee66-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dee66-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="dee66-185">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="dee66-186">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-186">Build the project.</span></span> <span data-ttu-id="dee66-187">Di seguito viene visualizzata nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="dee66-187">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="dee66-188">Pulire il progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-188">Clean the project.</span></span> <span data-ttu-id="dee66-189">Di seguito viene visualizzata nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="dee66-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dee66-190">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="dee66-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="dee66-191">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="dee66-192">Se si utilizza ASP.NET Core 1. x, ripristinare il pacchetto appena aggiunto:</span><span class="sxs-lookup"><span data-stu-id="dee66-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="dee66-193">Compilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="dee66-194">Viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dee66-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="dee66-195">Pulire il progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="dee66-196">Viene visualizzato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="dee66-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="dee66-197">Esecuzione ad hoc di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="dee66-198">È possibile eseguire le attività di creazione di bundle e riduzione in base ad hoc, senza compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="dee66-199">Aggiungere il [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacchetto NuGet al progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="dee66-200">Questo pacchetto estende l'interfaccia CLI Core .NET per includere il *dotnet bundle* strumento.</span><span class="sxs-lookup"><span data-stu-id="dee66-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="dee66-201">Nella finestra della Console di gestione di pacchetti (PMC) o in una shell dei comandi, è possibile eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dee66-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="dee66-202">Gestione pacchetti NuGet aggiunge le dipendenze per il file *. csproj come `<PackageReference />` nodi.</span><span class="sxs-lookup"><span data-stu-id="dee66-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="dee66-203">Il `dotnet bundle` comando è registrato con l'interfaccia CLI Core .NET solo quando un `<DotNetCliToolReference />` viene utilizzato il nodo.</span><span class="sxs-lookup"><span data-stu-id="dee66-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="dee66-204">Modificare di conseguenza il file *. csproj.</span><span class="sxs-lookup"><span data-stu-id="dee66-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="dee66-205">Aggiungere file al flusso di lavoro</span><span class="sxs-lookup"><span data-stu-id="dee66-205">Add files to workflow</span></span>

<span data-ttu-id="dee66-206">Si consideri un esempio in cui un ulteriore *custom.css* aggiunta di file simile a quello riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dee66-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="dee66-207">Per minimizzare *custom.css* e aggregare con *site.css* in un *site.min.css* file, aggiungere il relativo percorso *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="dee66-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="dee66-208">In alternativa, può essere utilizzato il modello il glob seguente:</span><span class="sxs-lookup"><span data-stu-id="dee66-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="dee66-209">Questo modello il glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzata.</span><span class="sxs-lookup"><span data-stu-id="dee66-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="dee66-210">Compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-210">Build the application.</span></span> <span data-ttu-id="dee66-211">Aprire *site.min.css* e osservare il contenuto di *custom.css* viene aggiunto alla fine del file.</span><span class="sxs-lookup"><span data-stu-id="dee66-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="dee66-212">Basato sull'ambiente come aggregare e riduzione</span><span class="sxs-lookup"><span data-stu-id="dee66-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="dee66-213">Come procedura consigliata, i file di bundle e minimizzati dell'app devono essere utilizzati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="dee66-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="dee66-214">Durante lo sviluppo, i file originali apportare per semplificare il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="dee66-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="dee66-215">Specificare i file da includere nelle pagine usando il [Helper di Tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="dee66-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="dee66-216">L'Helper di Tag di ambiente solo viene eseguito il rendering del relativo contenuto durante l'esecuzione di specifiche [ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="dee66-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="dee66-217">Nell'esempio `environment` tag esegue il rendering di file CSS non elaborati durante l'esecuzione nel `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="dee66-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dee66-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dee66-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dee66-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dee66-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="dee66-220">Nell'esempio `environment` tag esegue il rendering di file CSS in dotazione e minimizzati durante l'esecuzione in un ambiente diverso da `Development`.</span><span class="sxs-lookup"><span data-stu-id="dee66-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="dee66-221">Ad esempio, in esecuzione in `Production` o `Staging` attiva il rendering di tali fogli di stile:</span><span class="sxs-lookup"><span data-stu-id="dee66-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dee66-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dee66-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dee66-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dee66-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="dee66-224">Utilizzare bundleconfig.json da Gulp</span><span class="sxs-lookup"><span data-stu-id="dee66-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="dee66-225">Vi sono casi in cui come aggregare e minimizzazione flusso di lavoro un'app richiede un'ulteriore elaborazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="dee66-226">Sono esempi di ottimizzazione dell'immagine, busting cache e l'elaborazione di risorse della rete CDN.</span><span class="sxs-lookup"><span data-stu-id="dee66-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="dee66-227">Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro come aggregare e riduzione per l'utilizzo di Gulp.</span><span class="sxs-lookup"><span data-stu-id="dee66-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="dee66-228">Utilizzare l'estensione di Bundler & Minimizzatore</span><span class="sxs-lookup"><span data-stu-id="dee66-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="dee66-229">Visual Studio [Bundler & Minimizzatore](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) estensione gestisce la conversione a Gulp.</span><span class="sxs-lookup"><span data-stu-id="dee66-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="dee66-230">Fare doppio clic su di *bundleconfig.json* file in Esplora soluzioni e selezionare **Bundler & Minimizzatore** > **convertire a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="dee66-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Menu di scelta rapida per convertire Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="dee66-232">Il *gulpfile.js* e *package. JSON* file vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="dee66-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="dee66-233">Supporto [npm](https://www.npmjs.com/) pacchetti elencati nella *package. JSON* file `devDependencies` sezione vengono installati.</span><span class="sxs-lookup"><span data-stu-id="dee66-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="dee66-234">Nella finestra PMC per installare l'interfaccia CLI Gulp come una dipendenza globale, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dee66-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="dee66-235">Il *gulpfile.js* file legge il *bundleconfig.json* file per l'input, output e le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="dee66-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="dee66-236">Convertire manualmente</span><span class="sxs-lookup"><span data-stu-id="dee66-236">Convert manually</span></span>

<span data-ttu-id="dee66-237">Se Visual Studio e/o l'estensione di Bundler & Minimizzatore non è disponibile, convertire manualmente.</span><span class="sxs-lookup"><span data-stu-id="dee66-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="dee66-238">Aggiungere un *package. JSON* file, con i seguenti `devDependencies`, alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="dee66-239">Installare le dipendenze eseguendo il comando seguente allo stesso livello *package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="dee66-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="dee66-240">Installare l'interfaccia CLI Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="dee66-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="dee66-241">Copia il *gulpfile.js* file sotto la radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="dee66-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="dee66-242">Esecuzione di attività Gulp</span><span class="sxs-lookup"><span data-stu-id="dee66-242">Run Gulp tasks</span></span>

<span data-ttu-id="dee66-243">Per attivare l'attività di riduzione Gulp prima del progetto in Visual Studio, aggiungere il seguente [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) al file *. csproj:</span><span class="sxs-lookup"><span data-stu-id="dee66-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="dee66-244">In questo esempio, tutte le attività definite all'interno di `MyPreCompileTarget` destinazione eseguire prima predefiniti `Build` destinazione.</span><span class="sxs-lookup"><span data-stu-id="dee66-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="dee66-245">Output simile al seguente viene visualizzato nella finestra di Output di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dee66-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="dee66-246">In alternativa, Esplora esecuzione attività di Visual Studio può essere utilizzato per associare attività Gulp a eventi specifici di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dee66-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="dee66-247">Vedere [esecuzione di attività predefinito](xref:client-side/using-gulp#running-default-tasks) per istruzioni sull'esecuzione di operazioni con.</span><span class="sxs-lookup"><span data-stu-id="dee66-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dee66-248">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dee66-248">Additional resources</span></span>

* [<span data-ttu-id="dee66-249">Uso di Gulp</span><span class="sxs-lookup"><span data-stu-id="dee66-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="dee66-250">Uso di Grunt</span><span class="sxs-lookup"><span data-stu-id="dee66-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="dee66-251">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="dee66-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="dee66-252">Helper tag</span><span class="sxs-lookup"><span data-stu-id="dee66-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
