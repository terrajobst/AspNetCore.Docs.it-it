---
title: Bundle e minifiy asset statici in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione web ASP.NET Core applicando le tecniche di aggregazione e la riduzione.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: a3d49315fbb62eb1a42eb1b30885dc19a81c0a91
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094645"
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a><span data-ttu-id="b6954-103">Bundle e minifiy asset statici in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6954-103">Bundle and minifiy static assets in ASP.NET Core</span></span>

<span data-ttu-id="b6954-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="b6954-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b6954-105">Questo articolo illustra i vantaggi dell'applicazione come aggregare e minimizzazione, incluso come queste funzionalità possono essere utilizzate con le applicazioni web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6954-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="b6954-106">Che cos'è l'aggregazione e minimizzazione?</span><span class="sxs-lookup"><span data-stu-id="b6954-106">What is bundling and minification?</span></span>

<span data-ttu-id="b6954-107">Creazione di bundle e minimizzazione sono due le ottimizzazioni delle prestazioni di distinct che è possibile applicare in un'app web.</span><span class="sxs-lookup"><span data-stu-id="b6954-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="b6954-108">Utilizzati insieme, aggregazione e minimizzazione migliorare le prestazioni riducendo il numero di richieste di server e ridurre le dimensioni delle risorse statiche richieste.</span><span class="sxs-lookup"><span data-stu-id="b6954-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="b6954-109">Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="b6954-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="b6954-110">Una volta che una pagina web è stato richiesto, il browser memorizza nella cache le risorse statiche (JavaScript, CSS e immagini).</span><span class="sxs-lookup"><span data-stu-id="b6954-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="b6954-111">Di conseguenza, aggregazione e riduzione non migliorare le prestazioni quando si richiede la stessa pagina, o le pagine, nello stesso sito richiede le stesse risorse.</span><span class="sxs-lookup"><span data-stu-id="b6954-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="b6954-112">Se la scadenza di intestazione non è impostata correttamente le risorse in e se non viene usata l'aggregazione e riduzione, approccio euristico di aggiornamento del browser contrassegna le risorse non aggiornati dopo alcuni giorni.</span><span class="sxs-lookup"><span data-stu-id="b6954-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="b6954-113">Inoltre, il browser richiede una richiesta di convalida per ogni asset.</span><span class="sxs-lookup"><span data-stu-id="b6954-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="b6954-114">In questo caso, aggregazione e minimizzazione forniscono un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="b6954-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="b6954-115">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="b6954-115">Bundling</span></span>

<span data-ttu-id="b6954-116">Creazione di bundle combina più file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="b6954-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="b6954-117">Creazione di bundle riduce il numero di richieste di server necessari eseguire il rendering di una risorsa web, ad esempio una pagina web.</span><span class="sxs-lookup"><span data-stu-id="b6954-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="b6954-118">È possibile creare un numero qualsiasi di singoli pacchetti in modo specifico per CSS, JavaScript e così via. Un numero inferiore di file significa meno le richieste HTTP dal browser al server o dal servizio fornendo all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="b6954-119">I risultati miglioramento delle prestazioni di caricamento prima pagina.</span><span class="sxs-lookup"><span data-stu-id="b6954-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="b6954-120">Minimizzazione</span><span class="sxs-lookup"><span data-stu-id="b6954-120">Minification</span></span>

<span data-ttu-id="b6954-121">Minimizzazione rimuove i caratteri non necessari dal codice senza modificare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="b6954-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="b6954-122">Il risultato è una riduzione di grandi dimensioni in asset richiesti (ad esempio CSS, immagini e i file JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b6954-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="b6954-123">Gli effetti collaterali di minimizzazione includono abbreviare i nomi delle variabili per un carattere e la rimozione di commenti e gli spazi vuoti non necessari.</span><span class="sxs-lookup"><span data-stu-id="b6954-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="b6954-124">Si consideri la funzione JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="b6954-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="b6954-125">Minimizzazione riduce la funzione per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b6954-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="b6954-126">Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i nomi di parametri e variabili seguenti sono stati rinominati come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b6954-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="b6954-127">Originale</span><span class="sxs-lookup"><span data-stu-id="b6954-127">Original</span></span> | <span data-ttu-id="b6954-128">Ridenominazione</span><span class="sxs-lookup"><span data-stu-id="b6954-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="b6954-129">Impatto di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="b6954-129">Impact of bundling and minification</span></span>

<span data-ttu-id="b6954-130">Nella tabella seguente vengono descritte le differenze tra il caricamento delle risorse singolarmente e utilizzando l'aggregazione e la riduzione:</span><span class="sxs-lookup"><span data-stu-id="b6954-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="b6954-131">Operazione</span><span class="sxs-lookup"><span data-stu-id="b6954-131">Action</span></span> | <span data-ttu-id="b6954-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="b6954-132">With B/M</span></span> | <span data-ttu-id="b6954-133">Senza B/M</span><span class="sxs-lookup"><span data-stu-id="b6954-133">Without B/M</span></span> | <span data-ttu-id="b6954-134">Modifica</span><span class="sxs-lookup"><span data-stu-id="b6954-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="b6954-135">Richieste di file</span><span class="sxs-lookup"><span data-stu-id="b6954-135">File Requests</span></span>  | <span data-ttu-id="b6954-136">7</span><span class="sxs-lookup"><span data-stu-id="b6954-136">7</span></span>   | <span data-ttu-id="b6954-137">18</span><span class="sxs-lookup"><span data-stu-id="b6954-137">18</span></span>     | <span data-ttu-id="b6954-138">157%</span><span class="sxs-lookup"><span data-stu-id="b6954-138">157%</span></span>
<span data-ttu-id="b6954-139">KB trasferiti</span><span class="sxs-lookup"><span data-stu-id="b6954-139">KB Transferred</span></span> | <span data-ttu-id="b6954-140">156</span><span class="sxs-lookup"><span data-stu-id="b6954-140">156</span></span> | <span data-ttu-id="b6954-141">264.68</span><span class="sxs-lookup"><span data-stu-id="b6954-141">264.68</span></span> | <span data-ttu-id="b6954-142">70%</span><span class="sxs-lookup"><span data-stu-id="b6954-142">70%</span></span>
<span data-ttu-id="b6954-143">Tempo di caricamento (ms)</span><span class="sxs-lookup"><span data-stu-id="b6954-143">Load Time (ms)</span></span> | <span data-ttu-id="b6954-144">885</span><span class="sxs-lookup"><span data-stu-id="b6954-144">885</span></span> | <span data-ttu-id="b6954-145">2360</span><span class="sxs-lookup"><span data-stu-id="b6954-145">2360</span></span>   | <span data-ttu-id="b6954-146">167%</span><span class="sxs-lookup"><span data-stu-id="b6954-146">167%</span></span>

<span data-ttu-id="b6954-147">Browser sono piuttosto dettagliati per quanto riguarda le intestazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6954-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="b6954-148">I byte totali inviati metrica visto una significativa riduzione durante l'aggregazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="b6954-149">Il tempo di caricamento Mostra un miglioramento significativo, ma in questo esempio viene eseguito localmente.</span><span class="sxs-lookup"><span data-stu-id="b6954-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="b6954-150">Miglioramenti delle prestazioni superiori vengono realizzati quando l'utilizzo di aggregazione e minimizzazione asset trasferiti su una rete.</span><span class="sxs-lookup"><span data-stu-id="b6954-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="b6954-151">Scegliere una strategia di riduzione e di aggregazione</span><span class="sxs-lookup"><span data-stu-id="b6954-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="b6954-152">I modelli di progetto MVC e pagine Razor forniscono una soluzione di casella per la creazione di bundle e minimizzazione costituito da un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="b6954-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="b6954-153">Strumenti di terze parti, ad esempio il [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) attività canali, eseguire le stesse attività con una maggiore complessità.</span><span class="sxs-lookup"><span data-stu-id="b6954-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="b6954-154">Uno strumento di terze parti è un'ottima soluzione quando il flusso di lavoro di sviluppo richiede l'elaborazione oltre l'aggregazione e minimizzazione&mdash;, ad esempio ottimizzazione linting e immagine.</span><span class="sxs-lookup"><span data-stu-id="b6954-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="b6954-155">Usando come aggregare in fase di progettazione e la riduzione, vengono creati i file minimizzati prima della distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6954-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="b6954-156">Aggiunta e la riduzione prima della distribuzione offre il vantaggio del carico del server ridotto.</span><span class="sxs-lookup"><span data-stu-id="b6954-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="b6954-157">Tuttavia, è importante tenere presente che in fase di progettazione di aggregazione e minimizzazione aumenta la complessità di compilazione e funziona solo con file statici.</span><span class="sxs-lookup"><span data-stu-id="b6954-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="b6954-158">Configurare l'aggregazione e riduzione</span><span class="sxs-lookup"><span data-stu-id="b6954-158">Configure bundling and minification</span></span>

<span data-ttu-id="b6954-159">I modelli di progetto MVC e pagine Razor forniscono un *bundleconfig.json* file di configurazione che definisce le opzioni per ogni pacchetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="b6954-160">Per impostazione predefinita, viene modificata una configurazione di pacchetto per il codice JavaScript personalizzato (*wwwroot/js/site.js*) e fogli di stile (*wwwroot/css/site.css*) file:</span><span class="sxs-lookup"><span data-stu-id="b6954-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="b6954-161">Opzioni di configurazione includono:</span><span class="sxs-lookup"><span data-stu-id="b6954-161">Configuration options include:</span></span>

* <span data-ttu-id="b6954-162">`outputFileName`: Il nome del file di bundle all'output.</span><span class="sxs-lookup"><span data-stu-id="b6954-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="b6954-163">Può contenere un percorso relativo di *bundleconfig.json* file.</span><span class="sxs-lookup"><span data-stu-id="b6954-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="b6954-164">**Obbligatorio**</span><span class="sxs-lookup"><span data-stu-id="b6954-164">**required**</span></span>
* <span data-ttu-id="b6954-165">`inputFiles`: Una matrice di file da raggruppare.</span><span class="sxs-lookup"><span data-stu-id="b6954-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="b6954-166">Questi sono i percorsi relativi al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="b6954-167">**parametro facoltativo**, \* comporta un valore vuoto in un file di output vuoto.</span><span class="sxs-lookup"><span data-stu-id="b6954-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="b6954-168">[il glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.</span><span class="sxs-lookup"><span data-stu-id="b6954-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="b6954-169">`minify`: Le opzioni di riduzione per il tipo di output.</span><span class="sxs-lookup"><span data-stu-id="b6954-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="b6954-170">**parametro facoltativo**, *impostazione predefinita: `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="b6954-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="b6954-171">Opzioni di configurazione sono disponibili per ogni tipo di file di output.</span><span class="sxs-lookup"><span data-stu-id="b6954-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="b6954-172">Minimizzatore CSS</span><span class="sxs-lookup"><span data-stu-id="b6954-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="b6954-173">Minimizzatore JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6954-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="b6954-174">Minimizzatore HTML</span><span class="sxs-lookup"><span data-stu-id="b6954-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="b6954-175">`includeInProject`: Flag che indica se aggiungere i file generati file di progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="b6954-176">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="b6954-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="b6954-177">`sourceMap`: Flag che indica se generare una mappa di origine per il file aggregato.</span><span class="sxs-lookup"><span data-stu-id="b6954-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="b6954-178">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="b6954-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="b6954-179">`sourceMapRootPath`: Il percorso radice per l'archiviazione dei file di mappa di origine generato.</span><span class="sxs-lookup"><span data-stu-id="b6954-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="b6954-180">Esecuzione della fase di compilazione di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="b6954-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="b6954-181">Il [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacchetto NuGet consente l'esecuzione dell'aggregazione e riduzione in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="b6954-182">Inserisce il pacchetto [destinazioni di MSBuild](/visualstudio/msbuild/msbuild-targets) che eseguita in compilazione e l'ora pulita.</span><span class="sxs-lookup"><span data-stu-id="b6954-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="b6954-183">Il *bundleconfig.json* file viene analizzato dal processo di compilazione per generare i file di output in base alla configurazione definita.</span><span class="sxs-lookup"><span data-stu-id="b6954-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="b6954-184">BuildBundlerMinifier appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="b6954-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b6954-185">Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="b6954-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b6954-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6954-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b6954-187">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="b6954-188">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-188">Build the project.</span></span> <span data-ttu-id="b6954-189">Di seguito viene visualizzata nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="b6954-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="b6954-190">Pulire il progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-190">Clean the project.</span></span> <span data-ttu-id="b6954-191">Di seguito viene visualizzata nella finestra di Output:</span><span class="sxs-lookup"><span data-stu-id="b6954-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b6954-192">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="b6954-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b6954-193">Aggiungere il *BuildBundlerMinifier* pacchetto al progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="b6954-194">Se si utilizza ASP.NET Core 1. x, ripristinare il pacchetto appena aggiunto:</span><span class="sxs-lookup"><span data-stu-id="b6954-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="b6954-195">Compilare il progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="b6954-196">Viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b6954-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="b6954-197">Pulire il progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="b6954-198">Viene visualizzato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="b6954-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="b6954-199">Esecuzione ad hoc di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="b6954-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="b6954-200">È possibile eseguire le attività di creazione di bundle e riduzione in base ad hoc, senza compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="b6954-201">Aggiungere il [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacchetto NuGet al progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="b6954-202">BundlerMinifier.Core appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="b6954-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b6954-203">Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="b6954-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="b6954-204">Questo pacchetto estende l'interfaccia CLI Core .NET per includere il *dotnet bundle* strumento.</span><span class="sxs-lookup"><span data-stu-id="b6954-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="b6954-205">Nella finestra della Console di gestione di pacchetti (PMC) o in una shell dei comandi, è possibile eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6954-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="b6954-206">Gestione pacchetti NuGet aggiunge le dipendenze per il file \*. csproj come `<PackageReference />` nodi.</span><span class="sxs-lookup"><span data-stu-id="b6954-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="b6954-207">Il `dotnet bundle` comando è registrato con l'interfaccia CLI Core .NET solo quando un `<DotNetCliToolReference />` viene utilizzato il nodo.</span><span class="sxs-lookup"><span data-stu-id="b6954-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="b6954-208">Modificare di conseguenza il file \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="b6954-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="b6954-209">Aggiungere file al flusso di lavoro</span><span class="sxs-lookup"><span data-stu-id="b6954-209">Add files to workflow</span></span>

<span data-ttu-id="b6954-210">Si consideri un esempio in cui un ulteriore *custom.css* aggiunta di file simile a quello riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b6954-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="b6954-211">Per minimizzare *custom.css* e aggregare con *site.css* in un *site.min.css* file, aggiungere il relativo percorso *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="b6954-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="b6954-212">In alternativa, può essere utilizzato il modello il glob seguente:</span><span class="sxs-lookup"><span data-stu-id="b6954-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="b6954-213">Questo modello il glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzata.</span><span class="sxs-lookup"><span data-stu-id="b6954-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="b6954-214">Compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-214">Build the application.</span></span> <span data-ttu-id="b6954-215">Aprire *site.min.css* e osservare il contenuto di *custom.css* viene aggiunto alla fine del file.</span><span class="sxs-lookup"><span data-stu-id="b6954-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="b6954-216">Basato sull'ambiente come aggregare e riduzione</span><span class="sxs-lookup"><span data-stu-id="b6954-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="b6954-217">Come procedura consigliata, i file di bundle e minimizzati dell'app devono essere utilizzati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="b6954-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="b6954-218">Durante lo sviluppo, i file originali apportare per semplificare il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6954-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="b6954-219">Specificare i file da includere nelle pagine usando il [Helper di Tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="b6954-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="b6954-220">L'Helper di Tag di ambiente solo viene eseguito il rendering del relativo contenuto durante l'esecuzione di specifiche [ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="b6954-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="b6954-221">Nell'esempio `environment` tag esegue il rendering di file CSS non elaborati durante l'esecuzione nel `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="b6954-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6954-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6954-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6954-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6954-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="b6954-224">Nell'esempio `environment` tag esegue il rendering di file CSS in dotazione e minimizzati durante l'esecuzione in un ambiente diverso da `Development`.</span><span class="sxs-lookup"><span data-stu-id="b6954-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="b6954-225">Ad esempio, in esecuzione in `Production` o `Staging` attiva il rendering di tali fogli di stile:</span><span class="sxs-lookup"><span data-stu-id="b6954-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6954-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6954-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6954-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6954-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="b6954-228">Utilizzare bundleconfig.json da Gulp</span><span class="sxs-lookup"><span data-stu-id="b6954-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="b6954-229">Vi sono casi in cui come aggregare e minimizzazione flusso di lavoro un'app richiede un'ulteriore elaborazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="b6954-230">Sono esempi di ottimizzazione dell'immagine, busting cache e l'elaborazione di risorse della rete CDN.</span><span class="sxs-lookup"><span data-stu-id="b6954-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="b6954-231">Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro come aggregare e riduzione per l'utilizzo di Gulp.</span><span class="sxs-lookup"><span data-stu-id="b6954-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="b6954-232">Utilizzare l'estensione di Bundler & Minimizzatore</span><span class="sxs-lookup"><span data-stu-id="b6954-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="b6954-233">Visual Studio [Bundler & Minimizzatore](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) estensione gestisce la conversione a Gulp.</span><span class="sxs-lookup"><span data-stu-id="b6954-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="b6954-234">L'estensione di Bundler & Minimizzatore appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto.</span><span class="sxs-lookup"><span data-stu-id="b6954-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b6954-235">Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="b6954-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="b6954-236">Fare doppio clic su di *bundleconfig.json* file in Esplora soluzioni e selezionare **Bundler & Minimizzatore** > **convertire a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="b6954-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Menu di scelta rapida per convertire Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="b6954-238">Il *gulpfile.js* e *package. JSON* file vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="b6954-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="b6954-239">Supporto [npm](https://www.npmjs.com/) pacchetti elencati nella *package. JSON* file `devDependencies` sezione vengono installati.</span><span class="sxs-lookup"><span data-stu-id="b6954-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="b6954-240">Nella finestra PMC per installare l'interfaccia CLI Gulp come una dipendenza globale, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6954-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="b6954-241">Il *gulpfile.js* file legge il *bundleconfig.json* file per l'input, output e le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="b6954-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="b6954-242">Convertire manualmente</span><span class="sxs-lookup"><span data-stu-id="b6954-242">Convert manually</span></span>

<span data-ttu-id="b6954-243">Se Visual Studio e/o l'estensione di Bundler & Minimizzatore non è disponibile, convertire manualmente.</span><span class="sxs-lookup"><span data-stu-id="b6954-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="b6954-244">Aggiungere un *package. JSON* file, con i seguenti `devDependencies`, alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="b6954-245">Installare le dipendenze eseguendo il comando seguente allo stesso livello *package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b6954-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="b6954-246">Installare l'interfaccia CLI Gulp come dipendenza globale:</span><span class="sxs-lookup"><span data-stu-id="b6954-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="b6954-247">Copia il *gulpfile.js* file sotto la radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="b6954-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="b6954-248">Esecuzione di attività Gulp</span><span class="sxs-lookup"><span data-stu-id="b6954-248">Run Gulp tasks</span></span>

<span data-ttu-id="b6954-249">Per attivare l'attività di riduzione Gulp prima del progetto in Visual Studio, aggiungere il seguente [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) al file \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="b6954-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="b6954-250">In questo esempio, tutte le attività definite all'interno di `MyPreCompileTarget` destinazione eseguire prima predefiniti `Build` destinazione.</span><span class="sxs-lookup"><span data-stu-id="b6954-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="b6954-251">Output simile al seguente viene visualizzato nella finestra di Output di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b6954-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="b6954-252">In alternativa, Esplora esecuzione attività di Visual Studio può essere utilizzato per associare attività Gulp a eventi specifici di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6954-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="b6954-253">Vedere [esecuzione di attività predefinito](xref:client-side/using-gulp#running-default-tasks) per istruzioni sull'esecuzione di operazioni con.</span><span class="sxs-lookup"><span data-stu-id="b6954-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6954-254">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b6954-254">Additional resources</span></span>

* [<span data-ttu-id="b6954-255">Usare Gulp</span><span class="sxs-lookup"><span data-stu-id="b6954-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="b6954-256">Usare Grunt</span><span class="sxs-lookup"><span data-stu-id="b6954-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="b6954-257">Usare più ambienti</span><span class="sxs-lookup"><span data-stu-id="b6954-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="b6954-258">Helper tag</span><span class="sxs-lookup"><span data-stu-id="b6954-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
