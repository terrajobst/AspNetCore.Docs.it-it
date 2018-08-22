---
uid: single-page-application/overview/templates/hottowel-template
title: Modello di hot Towel | Microsoft Docs
author: madskristensen
description: Modello HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823838"
---
<a name="hot-towel-template"></a><span data-ttu-id="8fc5c-103">Modello hot Towel</span><span class="sxs-lookup"><span data-stu-id="8fc5c-103">Hot Towel template</span></span>
====================
<span data-ttu-id="8fc5c-104">da [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="8fc5c-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="8fc5c-105">Il modello di MVC Hot Towel è scritto da John Papa</span><span class="sxs-lookup"><span data-stu-id="8fc5c-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="8fc5c-106">Scegliere la versione da scaricare:</span><span class="sxs-lookup"><span data-stu-id="8fc5c-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="8fc5c-107">Hot Towel modello MVC per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8fc5c-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="8fc5c-108">Hot Towel modello MVC per Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8fc5c-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="8fc5c-109">Hot Towel: Perché non si vuole passare per l'applicazione a singola pagina senza uno!</span><span class="sxs-lookup"><span data-stu-id="8fc5c-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="8fc5c-110">Per compilare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare?</span><span class="sxs-lookup"><span data-stu-id="8fc5c-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="8fc5c-111">Usare Hot Towel e in pochi secondi si otterrà un'applicazione a singola pagina e tutti gli strumenti che necessari per creare su di esso.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="8fc5c-112">Hot Towel crea un ottimo punto di partenza per la creazione di un applicazione a pagina singola (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="8fc5c-113">Impostazione predefinita si fornisce una struttura modulare per codice, navigazione nella visualizzazione, l'associazione dati, gestione avanzata dei dati e lo stile semplice ma elegante.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="8fc5c-114">Hot Towel fornisce tutto ciò che occorre per compilare un'applicazione a singola pagina, è possibile concentrarsi sull'app, non le operazioni di base.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="8fc5c-115">Altre informazioni sulla creazione di un'applicazione a singola pagina dalla [John Papa video, esercitazioni e i corsi Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="8fc5c-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="8fc5c-116">Struttura dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="8fc5c-116">Application Structure</span></span>

<span data-ttu-id="8fc5c-117">Hot Towel SPA fornisce una cartella dell'App che contiene i file JavaScript e HTML che definiscono l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="8fc5c-118">All'interno della cartella di App:</span><span class="sxs-lookup"><span data-stu-id="8fc5c-118">Inside the App folder:</span></span>

- <span data-ttu-id="8fc5c-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="8fc5c-119">durandal</span></span>
- <span data-ttu-id="8fc5c-120">servizi</span><span class="sxs-lookup"><span data-stu-id="8fc5c-120">services</span></span>
- <span data-ttu-id="8fc5c-121">ViewModel</span><span class="sxs-lookup"><span data-stu-id="8fc5c-121">viewmodels</span></span>
- <span data-ttu-id="8fc5c-122">visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="8fc5c-122">views</span></span>

<span data-ttu-id="8fc5c-123">La cartella dell'App contiene una raccolta di moduli.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="8fc5c-124">Questi moduli includono funzionalità e dichiarano le dipendenze su altri moduli.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="8fc5c-125">La cartella views conterrà il codice HTML per l'applicazione e la cartella ViewModel contiene la logica di presentazione per le viste (un pattern MVVM comune).</span><span class="sxs-lookup"><span data-stu-id="8fc5c-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="8fc5c-126">La cartella services è ideale per l'inserimento di tutti i servizi comuni, ad esempio il recupero dei dati HTTP o l'interazione dell'archiviazione locale potrebbe essere necessario all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="8fc5c-127">È comune per più ViewModel per riutilizzare il codice dei moduli del servizio.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="8fc5c-128">Struttura delle applicazioni lato Server ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8fc5c-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="8fc5c-129">Hot Towel si basa sulla struttura di ASP.NET MVC potente e familiare.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="8fc5c-130">App\_Start</span><span class="sxs-lookup"><span data-stu-id="8fc5c-130">App\_Start</span></span>
- <span data-ttu-id="8fc5c-131">Content</span><span class="sxs-lookup"><span data-stu-id="8fc5c-131">Content</span></span>
- <span data-ttu-id="8fc5c-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="8fc5c-132">Controllers</span></span>
- <span data-ttu-id="8fc5c-133">Modelli</span><span class="sxs-lookup"><span data-stu-id="8fc5c-133">Models</span></span>
- <span data-ttu-id="8fc5c-134">Script</span><span class="sxs-lookup"><span data-stu-id="8fc5c-134">Scripts</span></span>
- <span data-ttu-id="8fc5c-135">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="8fc5c-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="8fc5c-136">Librerie in primo piano</span><span class="sxs-lookup"><span data-stu-id="8fc5c-136">Featured Libraries</span></span>

- <span data-ttu-id="8fc5c-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8fc5c-137">ASP.NET MVC</span></span>
- <span data-ttu-id="8fc5c-138">API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8fc5c-138">ASP.NET Web API</span></span>
- <span data-ttu-id="8fc5c-139">Ottimizzazione Web ASP.NET - creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="8fc5c-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="8fc5c-140">[Breeze.js](http://Breezejs.com) -gestione avanzata dei dati</span><span class="sxs-lookup"><span data-stu-id="8fc5c-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="8fc5c-141">[Durandal](http://Durandaljs.com) -navigazione e composizione della vista</span><span class="sxs-lookup"><span data-stu-id="8fc5c-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="8fc5c-142">[Knockout. js](http://Knockoutjs.com) -associazioni dati</span><span class="sxs-lookup"><span data-stu-id="8fc5c-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="8fc5c-143">[Require](http://requirejs.org) -modularità con processori AMD e ottimizzazione</span><span class="sxs-lookup"><span data-stu-id="8fc5c-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="8fc5c-144">[Toastr.js](http://jpapa.me/c7toastr) -messaggi popup</span><span class="sxs-lookup"><span data-stu-id="8fc5c-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="8fc5c-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) : applicazione di stili CSS affidabile</span><span class="sxs-lookup"><span data-stu-id="8fc5c-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="8fc5c-146">Installazione tramite il modello di applicazione a singola pagina di Visual Studio 2012 Hot Towel</span><span class="sxs-lookup"><span data-stu-id="8fc5c-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="8fc5c-147">Hot Towel può essere installato come un modello di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="8fc5c-148">Fare semplicemente clic `File`  |  `New Project` e scegliere `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="8fc5c-149">Quindi selezionare il ' applicazione a pagina singola Towel Hot "modello ed eseguire.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="8fc5c-150">Installazione tramite il pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="8fc5c-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="8fc5c-151">Hot Towel è anche un pacchetto NuGet che aggiunge un progetto MVC ASP.NET vuoto esistente.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="8fc5c-152">È sufficiente installare tramite Nuget e quindi eseguire.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="8fc5c-153">Come creare in Hot Towel?</span><span class="sxs-lookup"><span data-stu-id="8fc5c-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="8fc5c-154">È sufficiente iniziare ad aggiungere codice!</span><span class="sxs-lookup"><span data-stu-id="8fc5c-154">Simply start adding code!</span></span>

1. <span data-ttu-id="8fc5c-155">Aggiungere il codice del lato server, preferibilmente Entity Framework e API Web (che caratterizzano realmente con Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="8fc5c-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="8fc5c-156">Aggiungere visualizzazioni al `App/views` cartella</span><span class="sxs-lookup"><span data-stu-id="8fc5c-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="8fc5c-157">Aggiungere ViewModel di `App/viewmodels` cartella</span><span class="sxs-lookup"><span data-stu-id="8fc5c-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="8fc5c-158">Aggiungere HTML e Knockout associazioni dati per le nuove viste</span><span class="sxs-lookup"><span data-stu-id="8fc5c-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="8fc5c-159">Aggiornare le route di navigazione in `shell.js`</span><span class="sxs-lookup"><span data-stu-id="8fc5c-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="8fc5c-160">Procedura dettagliata di HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="8fc5c-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="8fc5c-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8fc5c-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="8fc5c-162">index. cshtml è la route e visualizzazione per l'applicazione MVC inizio.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="8fc5c-163">Contiene tutte le tag meta standard, css i collegamenti e riferimenti di JavaScript che ci si aspetta.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="8fc5c-164">Il corpo contiene un singolo `<div>` che è in tutto il contenuto (visualizzazioni) di cui verrà inserito quando vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="8fc5c-165">Il `@Scripts.Render` Usa Require. js per eseguire il punto di ingresso per il codice dell'applicazione, contenuta nel `main.js` file.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="8fc5c-166">La schermata iniziale viene fornita per illustrare come creare una schermata mentre caricate dall'app.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="8fc5c-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="8fc5c-167">App/main.js</span></span>

<span data-ttu-id="8fc5c-168">Il `main.js` file contiene il codice che verrà eseguito non appena viene caricato l'app.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="8fc5c-169">Si tratta in cui si desidera definire le route di navigazione, impostare l'avvio di visualizzazioni ed eseguire qualsiasi programma di installazione/di avvio automatico, ad esempio l'inizializzazione dei dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="8fc5c-170">Il `main.js` file definiti diversi attributi dei moduli del durandal per avviare l'inizio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="8fc5c-171">L'istruzione di definizione consente di risolvere le dipendenze di moduli in modo che siano disponibili per la funzione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="8fc5c-172">Prima di tutto i messaggi di debug sono abilitati, quali invio di messaggi sui quali eventi eseguite dall'applicazione alla finestra della console.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="8fc5c-173">Il codice di App indica durandal framework per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="8fc5c-174">Le convenzioni vengono impostate in modo che durandal conosce tutte le visualizzazioni e ViewModel sono contenuti nelle stesse cartelle denominate, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="8fc5c-175">Infine, il `app.setRoot` dà il via a carichi le `shell` usando un oggetto predefinito `entrance` animazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="8fc5c-176">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="8fc5c-176">Views</span></span>

<span data-ttu-id="8fc5c-177">Le visualizzazioni si trovano nella `App/views` cartella.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="8fc5c-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="8fc5c-178">shell.html</span></span>

<span data-ttu-id="8fc5c-179">Il `shell.html` contiene layout master per il codice HTML.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="8fc5c-180">Tutte le altre visualizzazioni verranno combinate in una posizione sul lato del `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="8fc5c-181">Hot Towel fornisce un `shell` con queste tre aree: un'intestazione, un'area di contenuto e un piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="8fc5c-182">Ognuna di queste aree è dotata di contenuto formare altre viste quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="8fc5c-183">Il `compose` associazioni per l'intestazione e piè di pagina sono hardcoded in Hot Towel in modo che punti la `nav` e `footer` Visualizza, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="8fc5c-184">L'associazione di compose per la sezione `#content` è associato il `router` elemento attivo del modulo.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="8fc5c-185">In altre parole, quando si fa clic su un collegamento di navigazione è visualizzazione corrispondente viene caricato in questa area.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="8fc5c-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="8fc5c-186">nav.html</span></span>

<span data-ttu-id="8fc5c-187">Il `nav.html` contiene i collegamenti di navigazione per l'applicazione a singola pagina.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="8fc5c-188">Si tratta in cui può essere inserita nella struttura di menu, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="8fc5c-189">Spesso si tratta di dati associati (usando Knockout) per il `router` modulo per visualizzare la navigazione è definito nel `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="8fc5c-190">Knockout Azure cerca di associare i dati degli attributi e associa quelli per il `shell` viewmodel per visualizzare le route di navigazione e per mostrare un progressbar (tramite Twitter Bootstrap) se il `router` modulo è in corso lo spostamento da una visualizzazione a un altro (vedere `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="8fc5c-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="8fc5c-191">Home. HTML e details.html</span><span class="sxs-lookup"><span data-stu-id="8fc5c-191">home.html and details.html</span></span>

<span data-ttu-id="8fc5c-192">Queste viste contengono HTML per le visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="8fc5c-193">Quando il `home` clic sul collegamento nella `nav` si fa clic sul menu della visualizzazione, il `home` verrà inserita nell'area del contenuto della visualizzazione il `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="8fc5c-194">Queste viste possono essere aumentate o sostituite con visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="8fc5c-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="8fc5c-195">footer.html</span></span>

<span data-ttu-id="8fc5c-196">Il `footer.html` contiene codice HTML visualizzato nel piè di pagina, in fondo il `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="8fc5c-197">ViewModel</span><span class="sxs-lookup"><span data-stu-id="8fc5c-197">ViewModels</span></span>

<span data-ttu-id="8fc5c-198">ViewModel vengono trovati nel `App/viewmodels` cartella.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="8fc5c-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="8fc5c-199">shell.js</span></span>

<span data-ttu-id="8fc5c-200">Il `shell` viewmodel contiene le proprietà e funzioni che sono associate ai `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="8fc5c-201">Spesso si tratta di dove si trovano le associazioni di navigazione di menu (vedere il `router.mapNav` per la logica).</span><span class="sxs-lookup"><span data-stu-id="8fc5c-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="8fc5c-202">details.js e js</span><span class="sxs-lookup"><span data-stu-id="8fc5c-202">home.js and details.js</span></span>

<span data-ttu-id="8fc5c-203">Tali ViewModel contengono le proprietà e funzioni che sono associate ai `home` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="8fc5c-204">inoltre contiene la logica di presentazione per la visualizzazione ed è l'associazione tra i dati e la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="8fc5c-205">Servizi</span><span class="sxs-lookup"><span data-stu-id="8fc5c-205">Services</span></span>

<span data-ttu-id="8fc5c-206">I servizi sono disponibili nella cartella/servizi App.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="8fc5c-207">In teoria i servizi futuri, ad esempio un modulo dataservice, che è responsabile per il recupero e la registrazione dei dati remota, è possibile attivare.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="8fc5c-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="8fc5c-208">logger.js</span></span>

<span data-ttu-id="8fc5c-209">Hot Towel fornisce un `logger` modulo nella cartella dei servizi.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="8fc5c-210">Il `logger` modulo è ideale per i messaggi di registrazione nella console e per l'utente nella finestra popup gli avvisi popup.</span><span class="sxs-lookup"><span data-stu-id="8fc5c-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
