---
title: Aggregare e minimizzare asset statici in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione Web ASP.NET Core applicando tecniche di aggregazione e minification.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658270"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="a2045-103">Aggregare e minimizzare asset statici in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2045-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="a2045-104">Di [Scott Addie](https://twitter.com/Scott_Addie) e [David Pine](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="a2045-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="a2045-105">Questo articolo illustra i vantaggi dell'applicazione di bundle e minification, incluso il modo in cui queste funzionalità possono essere usate con ASP.NET Core app Web.</span><span class="sxs-lookup"><span data-stu-id="a2045-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="a2045-106">Che cos'è la creazione di bundle e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-106">What is bundling and minification</span></span>

<span data-ttu-id="a2045-107">La creazione di bundle e minification sono due ottimizzazioni delle prestazioni distinte che è possibile applicare in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="a2045-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="a2045-108">Usati insieme, bundleing e minification migliorano le prestazioni riducendo il numero di richieste server e riducendo le dimensioni degli asset statici richiesti.</span><span class="sxs-lookup"><span data-stu-id="a2045-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="a2045-109">La creazione di bundle e minification migliora principalmente il tempo di caricamento della prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="a2045-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="a2045-110">Una volta richiesta una pagina Web, il browser memorizza nella cache gli asset statici (JavaScript, CSS e immagini).</span><span class="sxs-lookup"><span data-stu-id="a2045-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="a2045-111">Di conseguenza, la creazione di bundle e minification non migliora le prestazioni quando si richiede la stessa pagina o pagine nello stesso sito che richiede le stesse risorse.</span><span class="sxs-lookup"><span data-stu-id="a2045-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="a2045-112">Se l'intestazione Expires non è impostata correttamente negli asset e se non si usa la funzionalità di aggregazione e minification, l'euristica di aggiornamento del browser contrassegna gli asset obsoleti dopo alcuni giorni.</span><span class="sxs-lookup"><span data-stu-id="a2045-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="a2045-113">Inoltre, il browser richiede una richiesta di convalida per ogni asset.</span><span class="sxs-lookup"><span data-stu-id="a2045-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="a2045-114">In questo caso, la creazione di bundle e minification fornisce un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="a2045-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="a2045-115">Bundle</span><span class="sxs-lookup"><span data-stu-id="a2045-115">Bundling</span></span>

<span data-ttu-id="a2045-116">La creazione di bundle consente di combinare più file in un unico file.</span><span class="sxs-lookup"><span data-stu-id="a2045-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="a2045-117">La creazione di bundle riduce il numero di richieste del server necessarie per il rendering di un asset Web, ad esempio una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="a2045-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="a2045-118">È possibile creare un numero qualsiasi di bundle individuali in modo specifico per CSS, JavaScript e così via. Un numero inferiore di file indica un minor numero di richieste HTTP dal browser al server o dal servizio che fornisce l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2045-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="a2045-119">Ciò comporta un miglioramento delle prime prestazioni di caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="a2045-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="a2045-120">Minification</span><span class="sxs-lookup"><span data-stu-id="a2045-120">Minification</span></span>

<span data-ttu-id="a2045-121">Minification rimuove i caratteri non necessari dal codice senza alterare le funzionalità.</span><span class="sxs-lookup"><span data-stu-id="a2045-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="a2045-122">Il risultato è una riduzione significativa delle dimensioni negli asset richiesti, ad esempio CSS, immagini e file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2045-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="a2045-123">Gli effetti collaterali comuni di minification includono l'abbreviazione di nomi di variabili a un carattere e la rimozione di commenti e spazi vuoti superflui.</span><span class="sxs-lookup"><span data-stu-id="a2045-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="a2045-124">Si consideri la funzione JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="a2045-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="a2045-125">Minification riduce la funzione a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a2045-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="a2045-126">Oltre a rimuovere i commenti e gli spazi vuoti superflui, i nomi dei parametri e delle variabili seguenti sono stati rinominati come segue:</span><span class="sxs-lookup"><span data-stu-id="a2045-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="a2045-127">Originale</span><span class="sxs-lookup"><span data-stu-id="a2045-127">Original</span></span> | <span data-ttu-id="a2045-128">Ridenominazione</span><span class="sxs-lookup"><span data-stu-id="a2045-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="a2045-129">Effetti della creazione di bundle e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-129">Impact of bundling and minification</span></span>

<span data-ttu-id="a2045-130">Nella tabella seguente vengono descritte le differenze tra il caricamento individuale degli asset e l'utilizzo di bundle e minification:</span><span class="sxs-lookup"><span data-stu-id="a2045-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="a2045-131">Azione</span><span class="sxs-lookup"><span data-stu-id="a2045-131">Action</span></span> | <span data-ttu-id="a2045-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="a2045-132">With B/M</span></span> | <span data-ttu-id="a2045-133">Senza B/M</span><span class="sxs-lookup"><span data-stu-id="a2045-133">Without B/M</span></span> | <span data-ttu-id="a2045-134">Modifica</span><span class="sxs-lookup"><span data-stu-id="a2045-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="a2045-135">Richieste di file</span><span class="sxs-lookup"><span data-stu-id="a2045-135">File Requests</span></span>  | <span data-ttu-id="a2045-136">7</span><span class="sxs-lookup"><span data-stu-id="a2045-136">7</span></span>   | <span data-ttu-id="a2045-137">18</span><span class="sxs-lookup"><span data-stu-id="a2045-137">18</span></span>     | <span data-ttu-id="a2045-138">157%</span><span class="sxs-lookup"><span data-stu-id="a2045-138">157%</span></span>
<span data-ttu-id="a2045-139">KB trasferiti</span><span class="sxs-lookup"><span data-stu-id="a2045-139">KB Transferred</span></span> | <span data-ttu-id="a2045-140">156</span><span class="sxs-lookup"><span data-stu-id="a2045-140">156</span></span> | <span data-ttu-id="a2045-141">264.68</span><span class="sxs-lookup"><span data-stu-id="a2045-141">264.68</span></span> | <span data-ttu-id="a2045-142">70%</span><span class="sxs-lookup"><span data-stu-id="a2045-142">70%</span></span>
<span data-ttu-id="a2045-143">Tempo di caricamento (MS)</span><span class="sxs-lookup"><span data-stu-id="a2045-143">Load Time (ms)</span></span> | <span data-ttu-id="a2045-144">885</span><span class="sxs-lookup"><span data-stu-id="a2045-144">885</span></span> | <span data-ttu-id="a2045-145">2360</span><span class="sxs-lookup"><span data-stu-id="a2045-145">2360</span></span>   | <span data-ttu-id="a2045-146">167%</span><span class="sxs-lookup"><span data-stu-id="a2045-146">167%</span></span>

<span data-ttu-id="a2045-147">I browser sono piuttosto dettagliati rispetto alle intestazioni delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2045-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="a2045-148">La metrica totale dei byte inviati ha rilevato una riduzione significativa durante la creazione del bundle.</span><span class="sxs-lookup"><span data-stu-id="a2045-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="a2045-149">Il tempo di caricamento Mostra un miglioramento significativo, tuttavia questo esempio è stato eseguito localmente.</span><span class="sxs-lookup"><span data-stu-id="a2045-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="a2045-150">Quando si usa la creazione di bundle e minification con asset trasferiti in una rete, si ottengono maggiori vantaggi a livello di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="a2045-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="a2045-151">Scegliere una strategia di raggruppamento e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="a2045-152">I modelli di progetto MVC e Razor Pages offrono una soluzione predefinita per la creazione di bundle e minification costituita da un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="a2045-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="a2045-153">Gli strumenti di terze parti, ad [esempio l'attività](xref:client-side/using-grunt) Runner, eseguono le stesse attività con una maggiore complessità.</span><span class="sxs-lookup"><span data-stu-id="a2045-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="a2045-154">Uno strumento di terze parti è un'ottima soluzione quando il flusso di lavoro di sviluppo richiede l'elaborazione oltre la creazione di bundle e minification&mdash;, ad esempio l'ottimizzazione delle immagini e dei pelucchi.</span><span class="sxs-lookup"><span data-stu-id="a2045-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="a2045-155">Con la creazione di bundle e minification in fase di progettazione, i file minimizzati vengono creati prima della distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="a2045-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="a2045-156">La creazione di bundle e minimizzazione prima della distribuzione offre il vantaggio di ridurre il carico del server.</span><span class="sxs-lookup"><span data-stu-id="a2045-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="a2045-157">Tuttavia, è importante riconoscere che la creazione di bundle in fase di progettazione e minification aumenta la complessità della compilazione e funziona solo con i file statici.</span><span class="sxs-lookup"><span data-stu-id="a2045-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="a2045-158">Configurare la creazione di bundle e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="a2045-159">In ASP.NET Core 2,0 o versioni precedenti, i modelli di progetto MVC e Razor Pages forniscono un file di configurazione *bundleconfig. JSON* che definisce le opzioni per ogni bundle:</span><span class="sxs-lookup"><span data-stu-id="a2045-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a2045-160">In ASP.NET Core 2,1 o versioni successive aggiungere un nuovo file JSON, denominato *bundleconfig. JSON*, alla radice del progetto MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a2045-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="a2045-161">Includere nel file il codice JSON seguente come punto di partenza:</span><span class="sxs-lookup"><span data-stu-id="a2045-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="a2045-162">Il file *bundleconfig. JSON* definisce le opzioni per ogni bundle.</span><span class="sxs-lookup"><span data-stu-id="a2045-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="a2045-163">Nell'esempio precedente, viene definita una singola configurazione di bundle per i file personalizzati JavaScript (*wwwroot/JS/site. js*) e StyleSheet (*wwwroot/CSS/site. CSS*).</span><span class="sxs-lookup"><span data-stu-id="a2045-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="a2045-164">Le opzioni di configurazione possibili sono:</span><span class="sxs-lookup"><span data-stu-id="a2045-164">Configuration options include:</span></span>

* <span data-ttu-id="a2045-165">`outputFileName`: il nome del file di bundle da restituire.</span><span class="sxs-lookup"><span data-stu-id="a2045-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="a2045-166">Può contenere un percorso relativo dal file *bundleconfig. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a2045-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="a2045-167">**obbligatorio**</span><span class="sxs-lookup"><span data-stu-id="a2045-167">**required**</span></span>
* <span data-ttu-id="a2045-168">`inputFiles`: matrice di file da raggruppare insieme.</span><span class="sxs-lookup"><span data-stu-id="a2045-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="a2045-169">Si tratta di percorsi relativi del file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a2045-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="a2045-170">**facoltativo**, \* un valore vuoto restituisce un file di output vuoto.</span><span class="sxs-lookup"><span data-stu-id="a2045-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="a2045-171">sono supportati i modelli [glob](https://www.tldp.org/LDP/abs/html/globbingref.html) .</span><span class="sxs-lookup"><span data-stu-id="a2045-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="a2045-172">`minify`: opzioni minification per il tipo di output.</span><span class="sxs-lookup"><span data-stu-id="a2045-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="a2045-173">**facoltativo**, *`minify: { enabled: true }`predefinito*</span><span class="sxs-lookup"><span data-stu-id="a2045-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="a2045-174">Le opzioni di configurazione sono disponibili per ogni tipo di file di output.</span><span class="sxs-lookup"><span data-stu-id="a2045-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="a2045-175">Minifier CSS</span><span class="sxs-lookup"><span data-stu-id="a2045-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="a2045-176">Minifier JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2045-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="a2045-177">Minifier HTML</span><span class="sxs-lookup"><span data-stu-id="a2045-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="a2045-178">`includeInProject`: flag che indica se aggiungere i file generati al file di progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="a2045-179">**facoltativo**, *valore predefinito-false*</span><span class="sxs-lookup"><span data-stu-id="a2045-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="a2045-180">`sourceMap`: flag che indica se generare una mappa di origine per il file in bundle.</span><span class="sxs-lookup"><span data-stu-id="a2045-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="a2045-181">**facoltativo**, *valore predefinito-false*</span><span class="sxs-lookup"><span data-stu-id="a2045-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="a2045-182">`sourceMapRootPath`: percorso radice per archiviare il file di mapping di origine generato.</span><span class="sxs-lookup"><span data-stu-id="a2045-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="a2045-183">Esecuzione in fase di compilazione della creazione di bundle e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="a2045-184">Il pacchetto NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) consente l'esecuzione di bundle e minification in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="a2045-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="a2045-185">Il pacchetto inserisce [destinazioni MSBuild](/visualstudio/msbuild/msbuild-targets) che vengono eseguite in fase di compilazione e di pulizia.</span><span class="sxs-lookup"><span data-stu-id="a2045-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="a2045-186">Il file *bundleconfig. JSON* viene analizzato dal processo di compilazione per produrre i file di output in base alla configurazione definita.</span><span class="sxs-lookup"><span data-stu-id="a2045-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="a2045-187">BuildBundlerMinifier appartiene a un progetto gestito dalla community su GitHub per cui Microsoft non fornisce alcun supporto.</span><span class="sxs-lookup"><span data-stu-id="a2045-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="a2045-188">I problemi devono essere presentati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="a2045-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a2045-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2045-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2045-190">Aggiungere il pacchetto *BuildBundlerMinifier* al progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="a2045-191">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-191">Build the project.</span></span> <span data-ttu-id="a2045-192">Nella finestra di output verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a2045-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="a2045-193">Pulire il progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-193">Clean the project.</span></span> <span data-ttu-id="a2045-194">Nella finestra di output verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a2045-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-cli"></a>[<span data-ttu-id="a2045-195">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a2045-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a2045-196">Aggiungere il pacchetto *BuildBundlerMinifier* al progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="a2045-197">Se si usa ASP.NET Core 1. x, ripristinare il pacchetto appena aggiunto:</span><span class="sxs-lookup"><span data-stu-id="a2045-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="a2045-198">Compilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="a2045-199">Viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a2045-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="a2045-200">Pulire il progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="a2045-201">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="a2045-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="a2045-202">Esecuzione ad hoc di bundle e minification</span><span class="sxs-lookup"><span data-stu-id="a2045-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="a2045-203">È possibile eseguire le attività di creazione di bundle e minification su base ad hoc, senza compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="a2045-204">Aggiungere il pacchetto NuGet [BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) al progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="a2045-205">BundlerMinifier. Core appartiene a un progetto gestito dalla community in GitHub per cui Microsoft non fornisce alcun supporto.</span><span class="sxs-lookup"><span data-stu-id="a2045-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="a2045-206">I problemi devono essere presentati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="a2045-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="a2045-207">Questo pacchetto estende il interfaccia della riga di comando di .NET Core per includere lo strumento *DotNet-bundle* .</span><span class="sxs-lookup"><span data-stu-id="a2045-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="a2045-208">Il comando seguente può essere eseguito nella finestra console di gestione pacchetti (PMC) o in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="a2045-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="a2045-209">Gestione pacchetti NuGet aggiunge le dipendenze al file \*. csproj come nodi `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="a2045-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="a2045-210">Il comando `dotnet bundle` viene registrato con l'interfaccia della riga di comando di .NET Core solo quando viene usato un nodo `<DotNetCliToolReference />`.</span><span class="sxs-lookup"><span data-stu-id="a2045-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="a2045-211">Modificare di conseguenza il file \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="a2045-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="a2045-212">Aggiunta di file al flusso di lavoro</span><span class="sxs-lookup"><span data-stu-id="a2045-212">Add files to workflow</span></span>

<span data-ttu-id="a2045-213">Si consideri un esempio in cui viene aggiunto un file *CSS personalizzato* aggiuntivo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a2045-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="a2045-214">Per minimizzare *Custom. CSS* e aggregarlo con *site. CSS* in un file *site. min. CSS* , aggiungere il percorso relativo di *bundleconfig. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a2045-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="a2045-215">In alternativa, è possibile usare il modello glob seguente:</span><span class="sxs-lookup"><span data-stu-id="a2045-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="a2045-216">Questo modello glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzati.</span><span class="sxs-lookup"><span data-stu-id="a2045-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="a2045-217">Compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2045-217">Build the application.</span></span> <span data-ttu-id="a2045-218">Aprire *site. min. CSS* e notare che il contenuto di *Custom. CSS* viene aggiunto alla fine del file.</span><span class="sxs-lookup"><span data-stu-id="a2045-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="a2045-219">Creazione di bundle e minification basati su ambiente</span><span class="sxs-lookup"><span data-stu-id="a2045-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="a2045-220">Come procedura consigliata, i file in bundle e minimizzati dell'app devono essere usati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="a2045-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="a2045-221">Durante lo sviluppo, i file originali rendono più semplice il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="a2045-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="a2045-222">Specificare i file da includere nelle pagine usando l' [Helper tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="a2045-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="a2045-223">L'helper tag di ambiente esegue il rendering del relativo contenuto solo quando è in esecuzione in [ambienti](xref:fundamentals/environments)specifici.</span><span class="sxs-lookup"><span data-stu-id="a2045-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a2045-224">Il tag `environment` seguente esegue il rendering dei file CSS non elaborati durante l'esecuzione nell'ambiente `Development`:</span><span class="sxs-lookup"><span data-stu-id="a2045-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="a2045-225">Il tag di `environment` seguente esegue il rendering dei file CSS in bundle e minimizzati quando è in esecuzione in un ambiente diverso da `Development`.</span><span class="sxs-lookup"><span data-stu-id="a2045-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="a2045-226">Ad esempio, l'esecuzione in `Production` o `Staging` attiva il rendering di questi fogli di stile:</span><span class="sxs-lookup"><span data-stu-id="a2045-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="a2045-227">Utilizzare bundleconfig. JSON da Gulp</span><span class="sxs-lookup"><span data-stu-id="a2045-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="a2045-228">Ci sono casi in cui il flusso di lavoro di aggregazione e minification di un'app richiede un'elaborazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="a2045-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="a2045-229">Gli esempi includono l'ottimizzazione delle immagini, la memorizzazione nella cache e l'elaborazione delle risorse della rete CDN.</span><span class="sxs-lookup"><span data-stu-id="a2045-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="a2045-230">Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro di aggregazione e minification per usare Gulp.</span><span class="sxs-lookup"><span data-stu-id="a2045-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="a2045-231">Usare l'estensione Minifier di bundler &</span><span class="sxs-lookup"><span data-stu-id="a2045-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="a2045-232">Visual Studio [bundler & estensione Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) gestisce la conversione in Gulp.</span><span class="sxs-lookup"><span data-stu-id="a2045-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="a2045-233">Il bundler & estensione Minifier appartiene a un progetto gestito dalla community su GitHub per cui Microsoft non fornisce alcun supporto.</span><span class="sxs-lookup"><span data-stu-id="a2045-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="a2045-234">I problemi devono essere presentati [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="a2045-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="a2045-235">Fare clic con il pulsante destro del mouse sul file *bundleconfig. JSON* in Esplora soluzioni e selezionare **bundler & Minifier** > **Converti in Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="a2045-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Voce di menu di scelta rapida Converti in Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="a2045-237">I file *gulpfile. js* e *Package. JSON* vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="a2045-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="a2045-238">Sono installati i pacchetti [NPM](https://www.npmjs.com/) di supporto elencati nella sezione `devDependencies` del file *Package. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a2045-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="a2045-239">Eseguire il comando seguente nella finestra di PMC per installare l'interfaccia della riga di comando di Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="a2045-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="a2045-240">Il file *gulpfile. js* legge il file *bundleconfig. JSON* per gli input, gli output e le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="a2045-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="a2045-241">Converti manualmente</span><span class="sxs-lookup"><span data-stu-id="a2045-241">Convert manually</span></span>

<span data-ttu-id="a2045-242">Se Visual Studio e/o bundler & estensione Minifier non sono disponibili, eseguire la conversione manualmente.</span><span class="sxs-lookup"><span data-stu-id="a2045-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="a2045-243">Aggiungere un file *Package. JSON* , con i `devDependencies`seguenti, alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="a2045-244">Il modulo `gulp-uglify` non supporta ECMAScript (ES) 2015/ES6 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="a2045-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="a2045-245">Installare [Gulp-Terser](https://www.npmjs.com/package/gulp-terser) invece di `gulp-uglify` per usare ES2015/ES6 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="a2045-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="a2045-246">Installare le dipendenze eseguendo il comando seguente allo stesso livello di *Package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a2045-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="a2045-247">Installare l'interfaccia della riga di comando di Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="a2045-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="a2045-248">Copiare il file *gulpfile. js* riportato di seguito nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="a2045-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="a2045-249">Eseguire attività Gulp</span><span class="sxs-lookup"><span data-stu-id="a2045-249">Run Gulp tasks</span></span>

<span data-ttu-id="a2045-250">Per attivare l'attività minification di Gulp prima della compilazione del progetto in Visual Studio, aggiungere la [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) seguente al file \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="a2045-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="a2045-251">In questo esempio, qualsiasi attività definita all'interno della `MyPreCompileTarget` destinazione viene eseguita prima della destinazione `Build` predefinita.</span><span class="sxs-lookup"><span data-stu-id="a2045-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="a2045-252">Viene visualizzato un output simile al seguente nella finestra di output di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a2045-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a2045-253">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a2045-253">Additional resources</span></span>

* [<span data-ttu-id="a2045-254">Usare Grunt</span><span class="sxs-lookup"><span data-stu-id="a2045-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="a2045-255">Usare più ambienti</span><span class="sxs-lookup"><span data-stu-id="a2045-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="a2045-256">Helper tag</span><span class="sxs-lookup"><span data-stu-id="a2045-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
