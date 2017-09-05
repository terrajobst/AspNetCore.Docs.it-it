---
title: Come aggregare e riduzione in ASP.NET Core
author: spboyer
description: 
keywords: ASP.NET Core, aggregazione e minimizzazione CSS, JavaScript, minimizzare, BuildBundlerMinifier
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 810d89798330aeb1c387ec85eb05b1f4efde167a
ms.sourcegitcommit: 275a5381b6172b4f0b5fcd1d252aff03d3dae166
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="2a72a-103">Come aggregare e riduzione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a72a-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="2a72a-104">Creazione di bundle e minimizzazione sono due tecniche è possibile utilizzare in ASP.NET per migliorare le prestazioni di caricamento di pagina per l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="2a72a-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="2a72a-105">Creazione di bundle combina più file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="2a72a-106">Minimizzazione esegue diverse ottimizzazioni di codice diverse per gli script e CSS, determinando di payload più piccoli.</span><span class="sxs-lookup"><span data-stu-id="2a72a-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="2a72a-107">Utilizzati insieme, aggregazione e minimizzazione migliora le prestazioni in fase di carico riducendo il numero di richieste al server e riduzione delle dimensioni degli asset richiesti (ad esempio file CSS e JavaScript).</span><span class="sxs-lookup"><span data-stu-id="2a72a-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="2a72a-108">In questo articolo vengono illustrati i vantaggi dell'utilizzo di aggregazione e riduzione, incluso come queste funzionalità possono essere utilizzate con le applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a72a-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="2a72a-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2a72a-109">Overview</span></span>

<span data-ttu-id="2a72a-110">Nelle applicazioni ASP.NET Core, esistono diverse opzioni per l'aggregazione e la riduzione delle risorse sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2a72a-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="2a72a-111">I modelli di core per MVC forniscono una soluzione di casella utilizzando un file di configurazione e il pacchetto BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a72a-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="2a72a-112">Strumenti di terze parti, ad esempio [Gulp](using-gulp.md) e [Grunt](using-grunt.md) sono disponibili anche per eseguire le stesse attività dei processi richiede ulteriore flusso di lavoro o di complessità.</span><span class="sxs-lookup"><span data-stu-id="2a72a-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="2a72a-113">Usando come aggregare in fase di progettazione e la riduzione, vengono creati i file minimizzati prima della distribuzione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="2a72a-114">Aggiunta e la riduzione prima della distribuzione offre il vantaggio del carico del server ridotto.</span><span class="sxs-lookup"><span data-stu-id="2a72a-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="2a72a-115">Tuttavia, è importante tenere presente che in fase di progettazione di aggregazione e minimizzazione aumenta la complessità di compilazione e funziona solo con file statici.</span><span class="sxs-lookup"><span data-stu-id="2a72a-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="2a72a-116">Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="2a72a-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="2a72a-117">Una volta che una pagina web è stato richiesto, il browser memorizza nella cache le risorse (JavaScript, CSS e immagini) e aggregazione e riduzione non forniscono alcun miglioramento delle prestazioni quando si richiede la stessa pagina oppure pagine nello stesso sito richiede le stesse risorse.</span><span class="sxs-lookup"><span data-stu-id="2a72a-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="2a72a-118">Se non si imposta la scadenza testata correttamente le risorse e non si utilizza come aggregare e minimizzazione, euristica di aggiornamento del browser contrassegnerà l'asset non aggiornati dopo alcuni giorni e il browser richiederà una richiesta di convalida per ogni asset.</span><span class="sxs-lookup"><span data-stu-id="2a72a-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="2a72a-119">In questo caso, aggregazione e minimizzazione forniscono un aumento delle prestazioni anche dopo la prima richiesta di pagina.</span><span class="sxs-lookup"><span data-stu-id="2a72a-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="2a72a-120">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="2a72a-120">Bundling</span></span>

<span data-ttu-id="2a72a-121">Creazione di bundle è una funzionalità che rende più semplice combinare o aggregare più file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="2a72a-122">Poiché l'aggregazione combina più file in un singolo file, riduce il numero di richieste al server in cui sono necessari per recuperare e visualizzare una risorsa web, ad esempio una pagina web.</span><span class="sxs-lookup"><span data-stu-id="2a72a-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="2a72a-123">È possibile creare CSS, JavaScript e altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="2a72a-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="2a72a-124">Un numero inferiore di file significa meno le richieste HTTP dal browser al server o dal servizio fornendo all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="2a72a-125">I risultati miglioramento delle prestazioni di caricamento prima pagina.</span><span class="sxs-lookup"><span data-stu-id="2a72a-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="2a72a-126">Minimizzazione</span><span class="sxs-lookup"><span data-stu-id="2a72a-126">Minification</span></span>

<span data-ttu-id="2a72a-127">Riduzione di eseguire una serie di ottimizzazioni di codice diverso per ridurre le dimensioni di un asset richiesti (ad esempio, CSS, immagini, file JavaScript).</span><span class="sxs-lookup"><span data-stu-id="2a72a-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="2a72a-128">Risultati comuni della minimizzazione includono la rimozione di spazi vuoti non necessari e i commenti e abbreviare i nomi delle variabili per un carattere.</span><span class="sxs-lookup"><span data-stu-id="2a72a-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="2a72a-129">Si consideri la funzione JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="2a72a-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="2a72a-130">Dopo la minimizzazione, la funzione viene ridotto al seguente:</span><span class="sxs-lookup"><span data-stu-id="2a72a-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="2a72a-131">Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i seguenti parametri e i nomi delle variabili sono stati rinominati (abbreviato) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="2a72a-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="2a72a-132">Originale</span><span class="sxs-lookup"><span data-stu-id="2a72a-132">Original</span></span> | <span data-ttu-id="2a72a-133">Ridenominazione</span><span class="sxs-lookup"><span data-stu-id="2a72a-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="2a72a-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="2a72a-134">imageTagAndImageID</span></span> | <span data-ttu-id="2a72a-135">u</span><span class="sxs-lookup"><span data-stu-id="2a72a-135">t</span></span>
<span data-ttu-id="2a72a-136">imageContext</span><span class="sxs-lookup"><span data-stu-id="2a72a-136">imageContext</span></span> | <span data-ttu-id="2a72a-137">a</span><span class="sxs-lookup"><span data-stu-id="2a72a-137">a</span></span>
<span data-ttu-id="2a72a-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="2a72a-138">imageElement</span></span> | <span data-ttu-id="2a72a-139">f</span><span class="sxs-lookup"><span data-stu-id="2a72a-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="2a72a-140">Impatto di aggregazione e di riduzione</span><span class="sxs-lookup"><span data-stu-id="2a72a-140">Impact of bundling and minification</span></span>

<span data-ttu-id="2a72a-141">La tabella seguente illustra alcune importanti differenze tra l'elenco di tutte le risorse singolarmente e con aggregazione e la riduzione in una semplice pagina web:</span><span class="sxs-lookup"><span data-stu-id="2a72a-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="2a72a-142">Azione</span><span class="sxs-lookup"><span data-stu-id="2a72a-142">Action</span></span> | <span data-ttu-id="2a72a-143">Con B/M</span><span class="sxs-lookup"><span data-stu-id="2a72a-143">With B/M</span></span> | <span data-ttu-id="2a72a-144">Senza B/M</span><span class="sxs-lookup"><span data-stu-id="2a72a-144">Without B/M</span></span> | <span data-ttu-id="2a72a-145">Modifica</span><span class="sxs-lookup"><span data-stu-id="2a72a-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="2a72a-146">Richieste di file</span><span class="sxs-lookup"><span data-stu-id="2a72a-146">File Requests</span></span> |<span data-ttu-id="2a72a-147">7</span><span class="sxs-lookup"><span data-stu-id="2a72a-147">7</span></span> | <span data-ttu-id="2a72a-148">18</span><span class="sxs-lookup"><span data-stu-id="2a72a-148">18</span></span> | <span data-ttu-id="2a72a-149">157%</span><span class="sxs-lookup"><span data-stu-id="2a72a-149">157%</span></span>
<span data-ttu-id="2a72a-150">KB trasferiti</span><span class="sxs-lookup"><span data-stu-id="2a72a-150">KB Transferred</span></span> | <span data-ttu-id="2a72a-151">156</span><span class="sxs-lookup"><span data-stu-id="2a72a-151">156</span></span> | <span data-ttu-id="2a72a-152">264.68</span><span class="sxs-lookup"><span data-stu-id="2a72a-152">264.68</span></span> | <span data-ttu-id="2a72a-153">70%</span><span class="sxs-lookup"><span data-stu-id="2a72a-153">70%</span></span>
<span data-ttu-id="2a72a-154">Tempo di caricamento (MS)</span><span class="sxs-lookup"><span data-stu-id="2a72a-154">Load Time (MS)</span></span> | <span data-ttu-id="2a72a-155">885</span><span class="sxs-lookup"><span data-stu-id="2a72a-155">885</span></span> | <span data-ttu-id="2a72a-156">2360</span><span class="sxs-lookup"><span data-stu-id="2a72a-156">2360</span></span> | <span data-ttu-id="2a72a-157">167%</span><span class="sxs-lookup"><span data-stu-id="2a72a-157">167%</span></span>

<span data-ttu-id="2a72a-158">I byte inviati aveva una significativa riduzione con aggregazione come browser sono piuttosto dettagliati con le intestazioni HTTP che si applicano alle richieste.</span><span class="sxs-lookup"><span data-stu-id="2a72a-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="2a72a-159">Il tempo di caricamento Mostra un miglioramento grande, ma in questo esempio è stato eseguito localmente.</span><span class="sxs-lookup"><span data-stu-id="2a72a-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="2a72a-160">Si otterrà un aumento delle prestazioni durante l'utilizzo di aggregazione e minimizzazione asset trasferiti su una rete.</span><span class="sxs-lookup"><span data-stu-id="2a72a-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="2a72a-161">Utilizzo di aggregazione e riduzione in un progetto</span><span class="sxs-lookup"><span data-stu-id="2a72a-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="2a72a-162">Il modello di progetto MVC fornisce un `bundleconfig.json` file di configurazione che definisce le opzioni per ogni pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2a72a-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="2a72a-163">Per impostazione predefinita, viene modificata una configurazione di pacchetto per il codice JavaScript personalizzato (`wwwroot/js/site.js`) e fogli di stile (`wwwroot/css/site.css`) file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

<span data-ttu-id="2a72a-164">[!code-json[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span><span class="sxs-lookup"><span data-stu-id="2a72a-164">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span></span>

<span data-ttu-id="2a72a-165">Opzioni di raggruppamento includono:</span><span class="sxs-lookup"><span data-stu-id="2a72a-165">Bundle options include:</span></span>

* <span data-ttu-id="2a72a-166">outputFileName - nome del file di bundle all'output.</span><span class="sxs-lookup"><span data-stu-id="2a72a-166">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="2a72a-167">Può contenere un percorso relativo di `bundleconfig.json` file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-167">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="2a72a-168">**Obbligatorio**</span><span class="sxs-lookup"><span data-stu-id="2a72a-168">**required**</span></span>
* <span data-ttu-id="2a72a-169">inputFiles - matrice di file da raggruppare.</span><span class="sxs-lookup"><span data-stu-id="2a72a-169">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="2a72a-170">Questi sono i percorsi relativi al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-170">These are relative paths to the configuration file.</span></span> <span data-ttu-id="2a72a-171">**parametro facoltativo**, * comporta un valore vuoto in un file di output vuoto.</span><span class="sxs-lookup"><span data-stu-id="2a72a-171">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="2a72a-172">[il glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.</span><span class="sxs-lookup"><span data-stu-id="2a72a-172">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="2a72a-173">minimizzare - minimizzazione opzioni per l'output di tipo.</span><span class="sxs-lookup"><span data-stu-id="2a72a-173">minify - minification options for the output type.</span></span> <span data-ttu-id="2a72a-174">**parametro facoltativo**, *impostazione predefinita:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="2a72a-174">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="2a72a-175">Opzioni di configurazione sono disponibili per ogni tipo di file di output.</span><span class="sxs-lookup"><span data-stu-id="2a72a-175">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="2a72a-176">Minimizzatore CSS</span><span class="sxs-lookup"><span data-stu-id="2a72a-176">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="2a72a-177">Minimizzatore JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a72a-177">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/jsminifier)
    * [<span data-ttu-id="2a72a-178">Minimizzatore HTML</span><span class="sxs-lookup"><span data-stu-id="2a72a-178">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/htmlminifier)
* <span data-ttu-id="2a72a-179">includeInProject - aggiungere i file generati file di progetto.</span><span class="sxs-lookup"><span data-stu-id="2a72a-179">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="2a72a-180">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="2a72a-180">**optional**, *default - false*</span></span>
* <span data-ttu-id="2a72a-181">mapping di origine - Genera mapping di origine per il file aggregato.</span><span class="sxs-lookup"><span data-stu-id="2a72a-181">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="2a72a-182">**parametro facoltativo**, *predefinito: false*</span><span class="sxs-lookup"><span data-stu-id="2a72a-182">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="2a72a-183">Visual Studio 2015 / 2017</span><span class="sxs-lookup"><span data-stu-id="2a72a-183">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="2a72a-184">Aprire `bundleconfig.json` in Visual Studio, se l'ambiente non ha l'estensione installato; in un prompt dei comandi viene presentato suggerendo che sia presente una che potrebbe consentire a questo tipo di file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-184">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![Estensione BuildBundlerMinifier suggerimento](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="2a72a-186">Visualizzare le estensioni di selezionare e installare il **Bundler & Minimizzatore** estensione (riavviare richiede Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="2a72a-186">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![Estensione BuildBundlerMinifier suggerimento](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="2a72a-188">Una volta completato il riavvio, è necessario configurare la compilazione per eseguire i processi di scena e creazione di bundle di risorse sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2a72a-188">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="2a72a-189">Fare doppio clic su di `bundleconfig.json` file e selezionare *Enable bundle in fase di compilazione...* .</span><span class="sxs-lookup"><span data-stu-id="2a72a-189">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="2a72a-190">Compilare il progetto e `bundleconfig.json` è incluso nel processo di compilazione per generare i file di output in base alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-190">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="2a72a-191">Codice di Visual Studio o di riga di comando</span><span class="sxs-lookup"><span data-stu-id="2a72a-191">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="2a72a-192">Visual Studio e l'estensione guidare il processo di creazione di bundle e riduzione mediante i movimenti GUI; Tuttavia, le stesse funzionalità sono disponibili con il `dotnet` pacchetto CLI e BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a72a-192">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="2a72a-193">Aggiungere il pacchetto NuGet al progetto:</span><span class="sxs-lookup"><span data-stu-id="2a72a-193">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="2a72a-194">Ripristinare le dipendenze:</span><span class="sxs-lookup"><span data-stu-id="2a72a-194">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="2a72a-195">Compilare l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="2a72a-195">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="2a72a-196">I risultati dell'aggregazione in base a quello configurato e/o la minimizzazione illustrato nell'output del comando di compilazione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-196">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="2a72a-197">Aggiunta di file</span><span class="sxs-lookup"><span data-stu-id="2a72a-197">Adding files</span></span>

<span data-ttu-id="2a72a-198">In questo esempio, un file CSS aggiuntivo viene aggiunto chiamato `custom.css` e configurato per la creazione di bundle e minimizzazione con `site.css`, risultante in un singolo `site.min.css`.</span><span class="sxs-lookup"><span data-stu-id="2a72a-198">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="2a72a-199">Custom.CSS</span><span class="sxs-lookup"><span data-stu-id="2a72a-199">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="2a72a-200">Aggiungere il relativo percorso `bundleconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="2a72a-200">Add the relative path to `bundleconfig.json`.</span></span>

<span data-ttu-id="2a72a-201">[!code-json[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span><span class="sxs-lookup"><span data-stu-id="2a72a-201">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span></span>

> [!NOTE]
> <span data-ttu-id="2a72a-202">In alternativa, è Impossibile utilizzare il modello il glob - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` che ottiene CSS tutti i file e consente di escludere il modello di file minimizzata.</span><span class="sxs-lookup"><span data-stu-id="2a72a-202">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="2a72a-203">Compilare l'applicazione e se si apre `site.min.css`, si noterà ora che contenuto del `custom.css` è stato aggiunto alla fine del file.</span><span class="sxs-lookup"><span data-stu-id="2a72a-203">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="2a72a-204">Controllo di aggregazione e riduzione</span><span class="sxs-lookup"><span data-stu-id="2a72a-204">Controlling bundling and minification</span></span>

<span data-ttu-id="2a72a-205">In generale, si desidera utilizzare i file in bundle e minimizzati dell'app solo in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="2a72a-205">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="2a72a-206">Durante lo sviluppo, si desidera utilizzare i file originali, pertanto è più semplice eseguire il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="2a72a-206">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="2a72a-207">È possibile specificare quali script e file CSS da includere nelle pagine utilizzando l'helper di tag di ambiente nelle pagine di layout (vedere [gli helper di Tag](../mvc/views/tag-helpers/index.md)).</span><span class="sxs-lookup"><span data-stu-id="2a72a-207">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="2a72a-208">L'helper di tag di ambiente eseguirà il rendering del relativo contenuto solo quando è in esecuzione in ambienti specifici.</span><span class="sxs-lookup"><span data-stu-id="2a72a-208">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="2a72a-209">Vedere [utilizzo di più ambienti](../fundamentals/environments.md) per informazioni dettagliate su come specificare l'ambiente corrente.</span><span class="sxs-lookup"><span data-stu-id="2a72a-209">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="2a72a-210">Il seguente tag di ambiente verrà eseguito il rendering di file CSS non elaborati durante l'esecuzione nel `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="2a72a-210">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

<span data-ttu-id="2a72a-211">[!code-html[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span><span class="sxs-lookup"><span data-stu-id="2a72a-211">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span></span>

<span data-ttu-id="2a72a-212">Il tag di ambiente verrà eseguito il rendering di file CSS in dotazione e minimizzati solo durante l'esecuzione in `Production` o `Staging`:</span><span class="sxs-lookup"><span data-stu-id="2a72a-212">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

<span data-ttu-id="2a72a-213">[!code-html[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="2a72a-213">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span></span>

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="2a72a-214">Utilizzo di bundleconfig.json da Gulp</span><span class="sxs-lookup"><span data-stu-id="2a72a-214">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="2a72a-215">Se il flusso di lavoro come aggregare e riduzione del app richiede procedure aggiuntive, ad esempio l'elaborazione di immagini, busting cache, l'elaborazione di rete CDN assest, e così via, è possibile convertire il processo di raggruppamento e Minify a Gulp.</span><span class="sxs-lookup"><span data-stu-id="2a72a-215">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="2a72a-216">Opzione di conversione disponibile solo in Visual Studio 2015 e 2017.</span><span class="sxs-lookup"><span data-stu-id="2a72a-216">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="2a72a-217">Fare doppio clic su di `bundleconfig.json` e selezionare **convertire Gulp...** . Verrà generato il `gulpfile.js` e installare i pacchetti necessari npm.</span><span class="sxs-lookup"><span data-stu-id="2a72a-217">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Convertire Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="2a72a-219">Il `gulpfile.js` prodotti letture di `bundleconfig.json` file per la configurazione, pertanto può continuare a essere usata per l'input/output e le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="2a72a-219">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

<span data-ttu-id="2a72a-220">[!code-javascript[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span><span class="sxs-lookup"><span data-stu-id="2a72a-220">[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span></span>

<span data-ttu-id="2a72a-221">Per abilitare Gulp quando si compila il progetto in Visual Studio 2017, aggiungere il file *. csproj quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2a72a-221">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="2a72a-222">Per abilitare Gulp quando si compila il progetto in Visual Studio 2015, aggiungere il comando seguente per il `project.json` file:</span><span class="sxs-lookup"><span data-stu-id="2a72a-222">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="2a72a-223">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2a72a-223">Additional resources</span></span>

* [<span data-ttu-id="2a72a-224">Uso di Gulp</span><span class="sxs-lookup"><span data-stu-id="2a72a-224">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="2a72a-225">Utilizzo di Grunt</span><span class="sxs-lookup"><span data-stu-id="2a72a-225">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="2a72a-226">Utilizzo di più ambienti</span><span class="sxs-lookup"><span data-stu-id="2a72a-226">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="2a72a-227">Helper di tag</span><span class="sxs-lookup"><span data-stu-id="2a72a-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
