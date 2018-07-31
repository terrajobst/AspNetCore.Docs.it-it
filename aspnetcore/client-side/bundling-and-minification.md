---
title: Aggregare e minimizzare asset statici in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione web ASP.NET Core applicando tecniche di creazione di bundle e minimizzazione.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: bab2f288f3c6956e44ff929bfd2e257301a5806a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356701"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="dea4b-103">Aggregare e minimizzare asset statici in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dea4b-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="dea4b-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dea4b-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dea4b-105">Questo articolo illustra i vantaggi dell'applicazione e minimizzazione, tra cui come queste funzionalità possono essere utilizzate con le app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dea4b-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="dea4b-106">Che cos'è l'aggregazione e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-106">What is bundling and minification</span></span>

<span data-ttu-id="dea4b-107">Creazione di bundle e minimizzazione sono due le ottimizzazioni delle prestazioni distinct che è possibile applicare in un'app web.</span><span class="sxs-lookup"><span data-stu-id="dea4b-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="dea4b-108">Usati insieme, creazione di bundle e minimizzazione migliorare le prestazioni riducendo il numero di richieste al server e ridurre le dimensioni dei asset statici richiesti.</span><span class="sxs-lookup"><span data-stu-id="dea4b-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="dea4b-109">Creazione di bundle e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="dea4b-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="dea4b-110">Una volta che una pagina web è stato richiesto, il browser memorizza nella cache gli asset statici (JavaScript, CSS e immagini).</span><span class="sxs-lookup"><span data-stu-id="dea4b-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="dea4b-111">Di conseguenza, creazione di bundle e minimizzazione non migliorare le prestazioni quando si richiede la stessa pagina, o pagine, nello stesso sito richiede le stesse risorse.</span><span class="sxs-lookup"><span data-stu-id="dea4b-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="dea4b-112">Se la scadenza dell'intestazione non è impostato correttamente per le risorse e se non viene usata l'aggregazione e minimizzazione, l'euristica di aggiornamento del browser, contrassegnare gli asset non aggiornato dopo alcuni giorni.</span><span class="sxs-lookup"><span data-stu-id="dea4b-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="dea4b-113">Inoltre, il browser richiede una richiesta di convalida per ogni asset.</span><span class="sxs-lookup"><span data-stu-id="dea4b-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="dea4b-114">Creazione di bundle e minimizzazione in questo caso, fornire un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="dea4b-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="dea4b-115">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="dea4b-115">Bundling</span></span>

<span data-ttu-id="dea4b-116">Creazione di bundle di combinare più file in un unico file.</span><span class="sxs-lookup"><span data-stu-id="dea4b-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="dea4b-117">Creazione di bundle consente di ridurre il numero di richieste di server che sono necessari per eseguire il rendering di un asset web, ad esempio una pagina web.</span><span class="sxs-lookup"><span data-stu-id="dea4b-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="dea4b-118">È possibile creare un numero qualsiasi di singoli pacchetti in modo specifico per CSS, JavaScript e così via. Minor numero di file significa meno le richieste HTTP dal browser al server o dal servizio che fornisce all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="dea4b-119">Il risultato è migliorate le prestazioni di carico prima pagina.</span><span class="sxs-lookup"><span data-stu-id="dea4b-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="dea4b-120">Minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-120">Minification</span></span>

<span data-ttu-id="dea4b-121">Minimizzazione rimuove caratteri non necessari dal codice senza modificare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="dea4b-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="dea4b-122">Il risultato è una riduzione delle dimensioni significative asset richiesti (ad esempio CSS, immagini e file JavaScript).</span><span class="sxs-lookup"><span data-stu-id="dea4b-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="dea4b-123">Gli effetti collaterali di minimizzazione includono ad abbreviare i nomi delle variabili per un carattere e la rimozione di commenti e spazi vuoti non necessari.</span><span class="sxs-lookup"><span data-stu-id="dea4b-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="dea4b-124">Si consideri la seguente funzione di JavaScript:</span><span class="sxs-lookup"><span data-stu-id="dea4b-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="dea4b-125">Minimizzazione riduce la funzione al seguente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="dea4b-126">Oltre a rimuovere i commenti e spazi vuoti non necessari, i nomi di parametro e variabile seguenti sono stati rinominati come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dea4b-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="dea4b-127">Originale</span><span class="sxs-lookup"><span data-stu-id="dea4b-127">Original</span></span> | <span data-ttu-id="dea4b-128">Ridenominazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="dea4b-129">Impatto della creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-129">Impact of bundling and minification</span></span>

<span data-ttu-id="dea4b-130">La tabella seguente descrive le differenze tra il caricamento di asset e utilizzo di creazione di bundle e minimizzazione singolarmente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="dea4b-131">Operazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-131">Action</span></span> | <span data-ttu-id="dea4b-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="dea4b-132">With B/M</span></span> | <span data-ttu-id="dea4b-133">Senza B/M</span><span class="sxs-lookup"><span data-stu-id="dea4b-133">Without B/M</span></span> | <span data-ttu-id="dea4b-134">Modifica</span><span class="sxs-lookup"><span data-stu-id="dea4b-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="dea4b-135">Richieste di file</span><span class="sxs-lookup"><span data-stu-id="dea4b-135">File Requests</span></span>  | <span data-ttu-id="dea4b-136">7</span><span class="sxs-lookup"><span data-stu-id="dea4b-136">7</span></span>   | <span data-ttu-id="dea4b-137">18</span><span class="sxs-lookup"><span data-stu-id="dea4b-137">18</span></span>     | <span data-ttu-id="dea4b-138">157%</span><span class="sxs-lookup"><span data-stu-id="dea4b-138">157%</span></span>
<span data-ttu-id="dea4b-139">KB trasferiti</span><span class="sxs-lookup"><span data-stu-id="dea4b-139">KB Transferred</span></span> | <span data-ttu-id="dea4b-140">156</span><span class="sxs-lookup"><span data-stu-id="dea4b-140">156</span></span> | <span data-ttu-id="dea4b-141">264.68</span><span class="sxs-lookup"><span data-stu-id="dea4b-141">264.68</span></span> | <span data-ttu-id="dea4b-142">70%</span><span class="sxs-lookup"><span data-stu-id="dea4b-142">70%</span></span>
<span data-ttu-id="dea4b-143">Tempo di caricamento (ms)</span><span class="sxs-lookup"><span data-stu-id="dea4b-143">Load Time (ms)</span></span> | <span data-ttu-id="dea4b-144">885</span><span class="sxs-lookup"><span data-stu-id="dea4b-144">885</span></span> | <span data-ttu-id="dea4b-145">2360</span><span class="sxs-lookup"><span data-stu-id="dea4b-145">2360</span></span>   | <span data-ttu-id="dea4b-146">167%</span><span class="sxs-lookup"><span data-stu-id="dea4b-146">167%</span></span>

<span data-ttu-id="dea4b-147">I browser sono piuttosto dettagliati per quanto riguarda le intestazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dea4b-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="dea4b-148">I byte totali inviati metrica compariva una riduzione significativa della creazione di bundle.</span><span class="sxs-lookup"><span data-stu-id="dea4b-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="dea4b-149">Il tempo di caricamento Mostra un miglioramento significativo, ma in questo esempio viene eseguita in locale.</span><span class="sxs-lookup"><span data-stu-id="dea4b-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="dea4b-150">Miglioramenti delle prestazioni maggiori si concretizzano con gli asset di creazione di bundle e minimizzazione trasferiti su una rete.</span><span class="sxs-lookup"><span data-stu-id="dea4b-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="dea4b-151">Scegliere una strategia di creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="dea4b-152">I modelli di progetto MVC e Razor Pages forniscono una soluzione di out-of-the-box per la creazione di bundle e minimizzazione costituito da un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="dea4b-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="dea4b-153">Strumenti di terze parti, ad esempio la [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) gli strumenti di esecuzione di attività, eseguire le stesse attività con un po' più complesso.</span><span class="sxs-lookup"><span data-stu-id="dea4b-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="dea4b-154">Uno strumento di terze parti può essere un candidato ideale quando il flusso di lavoro di sviluppo richiede ulteriori elaborazioni, oltre alla creazione di bundle e minimizzazione&mdash;, come l'ottimizzazione di Lint e l'immagine.</span><span class="sxs-lookup"><span data-stu-id="dea4b-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="dea4b-155">Usando in fase di progettazione e minimizzazione, vengono creati i file minimizzati prima della distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dea4b-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="dea4b-156">Creazione di bundle e minimizzazione prima della distribuzione offre il vantaggio di carico del server ridotto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="dea4b-157">Tuttavia, è importante tenere presente che in fase di progettazione creazione di bundle e minimizzazione aumenta la complessità di compilazione e funziona solo con i file statici.</span><span class="sxs-lookup"><span data-stu-id="dea4b-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="dea4b-158">Configurare la creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-158">Configure bundling and minification</span></span>

<span data-ttu-id="dea4b-159">I modelli di progetto MVC e Razor Pages forniscono un *bundleconfig.json* file di configurazione che definisce le opzioni per ogni aggregazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="dea4b-160">Per impostazione predefinita, una configurazione singola aggregazione è definita per il codice JavaScript personalizzato (*wwwroot/js/site.js*) e fogli di stile (*wwwroot/css/site.css*) i file:</span><span class="sxs-lookup"><span data-stu-id="dea4b-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="dea4b-161">Opzioni di configurazione includono:</span><span class="sxs-lookup"><span data-stu-id="dea4b-161">Configuration options include:</span></span>

* <span data-ttu-id="dea4b-162">`outputFileName`: Nome del file di bundle per l'output.</span><span class="sxs-lookup"><span data-stu-id="dea4b-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="dea4b-163">Può contenere un percorso relativo di *bundleconfig.json* file.</span><span class="sxs-lookup"><span data-stu-id="dea4b-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="dea4b-164">**Obbligatorio**</span><span class="sxs-lookup"><span data-stu-id="dea4b-164">**required**</span></span>
* <span data-ttu-id="dea4b-165">`inputFiles`: Una matrice di file da raccogliere.</span><span class="sxs-lookup"><span data-stu-id="dea4b-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="dea4b-166">Questi sono i percorsi relativi nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="dea4b-167">**facoltativo**, \* restituisce un valore vuoto in un file di output vuoto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="dea4b-168">[glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.</span><span class="sxs-lookup"><span data-stu-id="dea4b-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="dea4b-169">`minify`: Le opzioni di minimizzazione per il tipo di output.</span><span class="sxs-lookup"><span data-stu-id="dea4b-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="dea4b-170">**facoltativo**, *default - `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="dea4b-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="dea4b-171">Opzioni di configurazione sono disponibili per ogni tipo di file di output.</span><span class="sxs-lookup"><span data-stu-id="dea4b-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="dea4b-172">Minimizzatore CSS</span><span class="sxs-lookup"><span data-stu-id="dea4b-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="dea4b-173">Minimizzatore JavaScript</span><span class="sxs-lookup"><span data-stu-id="dea4b-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="dea4b-174">Minimizzatore di HTML</span><span class="sxs-lookup"><span data-stu-id="dea4b-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="dea4b-175">`includeInProject`: Flag che indica se aggiungere i file generati da file di progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="dea4b-176">**facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="dea4b-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="dea4b-177">`sourceMap`: Flag che indica se generare una mappa di origine per il file nel bundle.</span><span class="sxs-lookup"><span data-stu-id="dea4b-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="dea4b-178">**facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="dea4b-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="dea4b-179">`sourceMapRootPath`: Il percorso radice per l'archiviazione file di mappa di origine generato.</span><span class="sxs-lookup"><span data-stu-id="dea4b-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="dea4b-180">Esecuzione fase di compilazione di creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="dea4b-181">Il [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacchetto NuGet consente l'esecuzione di creazione di bundle e minimizzazione in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="dea4b-182">Il pacchetto inserisce [destinazioni di MSBuild](/visualstudio/msbuild/msbuild-targets) che Esegui in compilazione e fase di pulizia.</span><span class="sxs-lookup"><span data-stu-id="dea4b-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="dea4b-183">Il *bundleconfig.json* file viene analizzato dal processo di compilazione per generare i file di output in base alla configurazione definita.</span><span class="sxs-lookup"><span data-stu-id="dea4b-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="dea4b-184">BuildBundlerMinifier appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community.</span><span class="sxs-lookup"><span data-stu-id="dea4b-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="dea4b-185">Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="dea4b-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dea4b-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dea4b-186">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dea4b-187">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="dea4b-188">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-188">Build the project.</span></span> <span data-ttu-id="dea4b-189">Di seguito viene visualizzato nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="dea4b-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="dea4b-190">Pulire il progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-190">Clean the project.</span></span> <span data-ttu-id="dea4b-191">Di seguito viene visualizzato nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="dea4b-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dea4b-192">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="dea4b-192">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dea4b-193">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="dea4b-194">Se si usa ASP.NET Core 1.x, ripristinare il pacchetto appena aggiunto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="dea4b-195">Compilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="dea4b-196">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dea4b-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="dea4b-197">Pulire il progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="dea4b-198">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="dea4b-199">L'esecuzione ad hoc di creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="dea4b-200">È possibile eseguire le attività di creazione di bundle e minimizzazione in base ad hoc, senza compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="dea4b-201">Aggiungere il [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacchetto NuGet al progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="dea4b-202">BundlerMinifier.Core appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community.</span><span class="sxs-lookup"><span data-stu-id="dea4b-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="dea4b-203">Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="dea4b-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="dea4b-204">Questo pacchetto estende la CLI di .NET Core per includere il *dotnet bundle* dello strumento.</span><span class="sxs-lookup"><span data-stu-id="dea4b-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="dea4b-205">Nella finestra della Console di gestione pacchetti (PMC) o in una shell dei comandi, è possibile eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="dea4b-206">Gestione pacchetti NuGet aggiunge le dipendenze al file csproj come `<PackageReference />` nodi.</span><span class="sxs-lookup"><span data-stu-id="dea4b-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="dea4b-207">Il `dotnet bundle` comando è registrato con la CLI di .NET Core solo quando un `<DotNetCliToolReference />` viene utilizzato il nodo.</span><span class="sxs-lookup"><span data-stu-id="dea4b-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="dea4b-208">Modificare di conseguenza il file \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="dea4b-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="dea4b-209">Aggiungere file al flusso di lavoro</span><span class="sxs-lookup"><span data-stu-id="dea4b-209">Add files to workflow</span></span>

<span data-ttu-id="dea4b-210">Si consideri un esempio in cui un'ulteriore *Custom. CSS* aggiunta di file simile a quello riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dea4b-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="dea4b-211">Per minimizzare *Custom. CSS* e creare un bundle con *Site. CSS* in un *site.min.css* Aggiungi il percorso relativo *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="dea4b-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="dea4b-212">In alternativa, può essere utilizzato il seguente criterio glob:</span><span class="sxs-lookup"><span data-stu-id="dea4b-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="dea4b-213">Questo criterio glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzati.</span><span class="sxs-lookup"><span data-stu-id="dea4b-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="dea4b-214">Compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-214">Build the application.</span></span> <span data-ttu-id="dea4b-215">Aprire *site.min.css* e notare che il contenuto del *Custom. CSS* viene aggiunto alla fine del file.</span><span class="sxs-lookup"><span data-stu-id="dea4b-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="dea4b-216">Basato sull'ambiente di creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="dea4b-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="dea4b-217">Come procedura consigliata, i file in bundle e minimizzati dell'app devono essere utilizzati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="dea4b-218">Durante lo sviluppo, i file originali apportare per semplificare il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="dea4b-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="dea4b-219">Specificare i file da includere nelle pagine usando il [Helper Tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle viste.</span><span class="sxs-lookup"><span data-stu-id="dea4b-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="dea4b-220">L'Helper Tag di ambiente il rendering solo il contenuto durante l'esecuzione in specifici [ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="dea4b-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="dea4b-221">Quanto segue `environment` tag esegue il rendering i file non elaborati di CSS quando si esegue il `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dea4b-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dea4b-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dea4b-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dea4b-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="dea4b-224">Quanto segue `environment` tag esegue il rendering di file CSS in bundle e minimizzati durante l'esecuzione in un ambiente diverso da `Development`.</span><span class="sxs-lookup"><span data-stu-id="dea4b-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="dea4b-225">Ad esempio, in esecuzione in `Production` o `Staging` attiva il rendering di tali fogli di stile:</span><span class="sxs-lookup"><span data-stu-id="dea4b-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dea4b-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dea4b-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dea4b-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dea4b-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="dea4b-228">Utilizzare bundleconfig.json da Gulp</span><span class="sxs-lookup"><span data-stu-id="dea4b-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="dea4b-229">Vi sono casi in cui e minimizzazione flusso di lavoro un'app richiede un'ulteriore elaborazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="dea4b-230">Sono esempi di ottimizzazione dell'immagine, cache busting ed elaborazione di asset della rete CDN.</span><span class="sxs-lookup"><span data-stu-id="dea4b-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="dea4b-231">Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro di creazione di bundle e minimizzazione per l'uso di Gulp.</span><span class="sxs-lookup"><span data-stu-id="dea4b-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="dea4b-232">Usare l'estensione di Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="dea4b-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="dea4b-233">Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) estensione gestisce la conversione a Gulp.</span><span class="sxs-lookup"><span data-stu-id="dea4b-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="dea4b-234">L'estensione Bundler & Minifier appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community.</span><span class="sxs-lookup"><span data-stu-id="dea4b-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="dea4b-235">Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="dea4b-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="dea4b-236">Fare doppio clic il *bundleconfig.json* del file in Esplora soluzioni e selezionare **Bundler & Minifier** > **convertire a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="dea4b-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Convertire a Gulp menu di scelta rapida](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="dea4b-238">Il *gulpfile. js* e *package. JSON* file vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="dea4b-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="dea4b-239">Il tipo di supporto [npm](https://www.npmjs.com/) i pacchetti elencati nel *package. JSON* del file `devDependencies` sezione installate.</span><span class="sxs-lookup"><span data-stu-id="dea4b-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="dea4b-240">Eseguire il comando seguente nella finestra della console di gestione pacchetti per installare l'interfaccia della riga di comando Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="dea4b-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="dea4b-241">Il *gulpfile. js* letture di file le *bundleconfig.json* file per l'input, output e le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="dea4b-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="dea4b-242">Convertire manualmente</span><span class="sxs-lookup"><span data-stu-id="dea4b-242">Convert manually</span></span>

<span data-ttu-id="dea4b-243">Se Visual Studio e/o l'estensione Bundler & Minifier non è disponibile, convertire manualmente.</span><span class="sxs-lookup"><span data-stu-id="dea4b-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="dea4b-244">Aggiungere un *package. JSON* file, con il codice seguente `devDependencies`, alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="dea4b-245">Installare le dipendenze eseguendo il comando seguente allo stesso livello *package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="dea4b-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="dea4b-246">Installare l'interfaccia della riga di comando Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="dea4b-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="dea4b-247">Copia il *gulpfile. js* file sotto la radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="dea4b-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="dea4b-248">Eseguire le attività Gulp</span><span class="sxs-lookup"><span data-stu-id="dea4b-248">Run Gulp tasks</span></span>

<span data-ttu-id="dea4b-249">Per attivare l'attività di minimizzazione Gulp prima il progetto viene compilato in Visual Studio, aggiungere quanto segue [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) al file \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="dea4b-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="dea4b-250">In questo esempio, tutte le attività definiti all'interno di `MyPreCompileTarget` destinazione eseguire prima dell'oggetto predefinito `Build` destinazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="dea4b-251">Nella finestra di Output di Visual Studio viene visualizzato output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dea4b-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="dea4b-252">In alternativa, Visual Studio Task Runner Explorer può essere utilizzato per associare le attività Gulp a eventi specifici di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dea4b-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="dea4b-253">Visualizzare [esecuzione di attività predefinito](xref:client-side/using-gulp#running-default-tasks) per istruzioni su come questa operazione.</span><span class="sxs-lookup"><span data-stu-id="dea4b-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dea4b-254">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dea4b-254">Additional resources</span></span>

* [<span data-ttu-id="dea4b-255">Usare Gulp</span><span class="sxs-lookup"><span data-stu-id="dea4b-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="dea4b-256">Usare Grunt</span><span class="sxs-lookup"><span data-stu-id="dea4b-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="dea4b-257">Usare più ambienti</span><span class="sxs-lookup"><span data-stu-id="dea4b-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="dea4b-258">Helper tag</span><span class="sxs-lookup"><span data-stu-id="dea4b-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
