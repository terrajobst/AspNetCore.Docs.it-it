---
uid: single-page-application/overview/templates/hottowel-template
title: Modello degli asciugamani a caldo | Documenti Microsoft
author: madskristensen
description: Modello HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a><span data-ttu-id="568ba-103">Modello degli asciugamani a caldo</span><span class="sxs-lookup"><span data-stu-id="568ba-103">Hot Towel template</span></span>
====================
<span data-ttu-id="568ba-104">da [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="568ba-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="568ba-105">Il modello di MVC degli asciugamani a caldo è scritto da John Papa</span><span class="sxs-lookup"><span data-stu-id="568ba-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="568ba-106">Scegliere la versione da scaricare:</span><span class="sxs-lookup"><span data-stu-id="568ba-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="568ba-107">Modello MVC per accesso frequente asciugamani per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="568ba-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="568ba-108">Modello MVC per accesso frequente asciugamani per Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="568ba-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="568ba-109">Scelta degli asciugamani: Perché non si desidera utilizzare l'autenticazione SPA senza uno!</span><span class="sxs-lookup"><span data-stu-id="568ba-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="568ba-110">Per compilare un SPA ma non è possibile decidere dove iniziare?</span><span class="sxs-lookup"><span data-stu-id="568ba-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="568ba-111">Utilizzare degli asciugamani attivo e in secondi è necessario un SPA e tutti gli strumenti che necessari per creare su di esso!</span><span class="sxs-lookup"><span data-stu-id="568ba-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="568ba-112">Scelta degli asciugamani Crea punto di partenza ideale per la creazione di un'applicazione a pagina singola (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="568ba-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="568ba-113">All'esterno della casella si fornisce una struttura modulare per codice, la visualizzazione, associazione dati, gestione di dati complessi e lo stile semplice ma elegante.</span><span class="sxs-lookup"><span data-stu-id="568ba-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="568ba-114">Scelta degli asciugamani offre tutto ciò che occorre per compilare un SPA, è possibile concentrarsi sull'applicazione, non l'impianto.</span><span class="sxs-lookup"><span data-stu-id="568ba-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="568ba-115">Ulteriori informazioni sulla compilazione di un SPA da [John Papa video, esercitazioni e corsi Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="568ba-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="568ba-116">Struttura dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="568ba-116">Application Structure</span></span>

<span data-ttu-id="568ba-117">Scelta degli asciugamani SPA fornisce una cartella di App che contiene i file JavaScript e HTML che definiscono l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="568ba-118">All'interno della cartella di App:</span><span class="sxs-lookup"><span data-stu-id="568ba-118">Inside the App folder:</span></span>

- <span data-ttu-id="568ba-119">durandal</span><span class="sxs-lookup"><span data-stu-id="568ba-119">durandal</span></span>
- <span data-ttu-id="568ba-120">servizi</span><span class="sxs-lookup"><span data-stu-id="568ba-120">services</span></span>
- <span data-ttu-id="568ba-121">ViewModel</span><span class="sxs-lookup"><span data-stu-id="568ba-121">viewmodels</span></span>
- <span data-ttu-id="568ba-122">visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="568ba-122">views</span></span>

<span data-ttu-id="568ba-123">La cartella di App contiene una raccolta di moduli.</span><span class="sxs-lookup"><span data-stu-id="568ba-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="568ba-124">Questi moduli incapsulano la funzionalità e dichiarano dipendenze in altri moduli.</span><span class="sxs-lookup"><span data-stu-id="568ba-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="568ba-125">La cartella views contiene il codice HTML per l'applicazione e la cartella ViewModel contiene la logica di presentazione per le viste (un comune modello MVVM).</span><span class="sxs-lookup"><span data-stu-id="568ba-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="568ba-126">La cartella servizi è ideale per l'inserimento di tutti i servizi comuni che l'applicazione potrebbe essere necessario, ad esempio il recupero dei dati HTTP o l'interazione di archiviazione locale.</span><span class="sxs-lookup"><span data-stu-id="568ba-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="568ba-127">È comune per più ViewModel di riutilizzare codice dei moduli del servizio.</span><span class="sxs-lookup"><span data-stu-id="568ba-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="568ba-128">Struttura delle applicazioni lato Server ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="568ba-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="568ba-129">Scelta degli asciugamani compila in base alla struttura di ASP.NET MVC potente e familiare.</span><span class="sxs-lookup"><span data-stu-id="568ba-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="568ba-130">App\_avviare</span><span class="sxs-lookup"><span data-stu-id="568ba-130">App\_Start</span></span>
- <span data-ttu-id="568ba-131">Content</span><span class="sxs-lookup"><span data-stu-id="568ba-131">Content</span></span>
- <span data-ttu-id="568ba-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="568ba-132">Controllers</span></span>
- <span data-ttu-id="568ba-133">Modelli</span><span class="sxs-lookup"><span data-stu-id="568ba-133">Models</span></span>
- <span data-ttu-id="568ba-134">Script</span><span class="sxs-lookup"><span data-stu-id="568ba-134">Scripts</span></span>
- <span data-ttu-id="568ba-135">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="568ba-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="568ba-136">Librerie in primo piano</span><span class="sxs-lookup"><span data-stu-id="568ba-136">Featured Libraries</span></span>

- <span data-ttu-id="568ba-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="568ba-137">ASP.NET MVC</span></span>
- <span data-ttu-id="568ba-138">API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="568ba-138">ASP.NET Web API</span></span>
- <span data-ttu-id="568ba-139">Ottimizzazione di ASP.NET Web - creazione di bundle e riduzione</span><span class="sxs-lookup"><span data-stu-id="568ba-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="568ba-140">[Breeze.js](http://Breezejs.com) -gestione di dati complessi</span><span class="sxs-lookup"><span data-stu-id="568ba-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="568ba-141">[Durandal.js](http://Durandaljs.com) -navigazione e composizione della vista</span><span class="sxs-lookup"><span data-stu-id="568ba-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="568ba-142">[Knockout.js](http://Knockoutjs.com) -i data binding</span><span class="sxs-lookup"><span data-stu-id="568ba-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="568ba-143">[Require](http://requirejs.org) -modularità con AMD e ottimizzazione</span><span class="sxs-lookup"><span data-stu-id="568ba-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="568ba-144">[Toastr.js](http://jpapa.me/c7toastr) -messaggi popup</span><span class="sxs-lookup"><span data-stu-id="568ba-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="568ba-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - applicazione di stili CSS affidabile</span><span class="sxs-lookup"><span data-stu-id="568ba-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="568ba-146">L'installazione tramite il modello SPA degli asciugamani a caldo di Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="568ba-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="568ba-147">Scelta degli asciugamani possono essere installato come un modello di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="568ba-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="568ba-148">Fare clic su `File`  |  `New Project` e scegliere `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="568ba-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="568ba-149">Selezionare quindi il ' applicazione a pagina singola degli asciugamani a caldo "modello ed eseguire!</span><span class="sxs-lookup"><span data-stu-id="568ba-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="568ba-150">L'installazione tramite il pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="568ba-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="568ba-151">Scelta degli asciugamani sono anche un pacchetto NuGet che aumenta di un progetto ASP.NET MVC vuoto esistente.</span><span class="sxs-lookup"><span data-stu-id="568ba-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="568ba-152">È sufficiente installare tramite Nuget e quindi eseguire!</span><span class="sxs-lookup"><span data-stu-id="568ba-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="568ba-153">Come creare su asciugamani frequente?</span><span class="sxs-lookup"><span data-stu-id="568ba-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="568ba-154">Iniziare semplicemente l'aggiunta di codice.</span><span class="sxs-lookup"><span data-stu-id="568ba-154">Simply start adding code!</span></span>

1. <span data-ttu-id="568ba-155">Aggiungere il codice lato server, preferibilmente Entity Framework e WebAPI (che caratterizzano effettivamente con Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="568ba-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="568ba-156">Aggiungere visualizzazioni per il `App/views` cartella</span><span class="sxs-lookup"><span data-stu-id="568ba-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="568ba-157">Aggiungere ViewModel per il `App/viewmodels` cartella</span><span class="sxs-lookup"><span data-stu-id="568ba-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="568ba-158">Aggiungere codice HTML e Knockout associazioni di dati alle nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="568ba-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="568ba-159">Aggiornare le route di navigazione in `shell.js`</span><span class="sxs-lookup"><span data-stu-id="568ba-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="568ba-160">Procedura dettagliata del codice HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="568ba-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="568ba-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="568ba-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="568ba-162">cshtml è la route iniziale e visualizzazione per l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="568ba-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="568ba-163">Contiene tutti i tag meta standard, i collegamenti di css e JavaScript riferimenti che previsto.</span><span class="sxs-lookup"><span data-stu-id="568ba-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="568ba-164">Il corpo contiene un singolo `<div>` che è in tutto il contenuto (visualizzazioni) di cui verrà inserito quando vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="568ba-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="568ba-165">Il `@Scripts.Render` Usa Require.js per eseguire il punto di ingresso per il codice dell'applicazione, contenuta nella `main.js` file.</span><span class="sxs-lookup"><span data-stu-id="568ba-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="568ba-166">La schermata iniziale viene fornita per illustrare come creare una schermata durante il caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="568ba-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="568ba-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="568ba-167">App/main.js</span></span>

<span data-ttu-id="568ba-168">Il `main.js` file contiene il codice che verrà eseguito non appena viene caricata l'app.</span><span class="sxs-lookup"><span data-stu-id="568ba-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="568ba-169">Si tratta in cui si desidera definire i percorsi di navigazione, impostare l'avvio delle viste e di eseguire qualsiasi programma di installazione o l'avvio, ad esempio priming i dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="568ba-170">Il `main.js` file definisce molti dei moduli del durandal per avviare kick dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="568ba-171">L'istruzione di definizione consente di risolvere le dipendenze di moduli in modo che siano disponibili per la funzione.</span><span class="sxs-lookup"><span data-stu-id="568ba-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="568ba-172">Innanzitutto, sono abilitati i messaggi di debug quale inviare messaggi sugli eventi che sta eseguendo l'applicazione alla finestra della console.</span><span class="sxs-lookup"><span data-stu-id="568ba-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="568ba-173">Il codice dell'App indica durandal framework per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="568ba-174">Le convenzioni vengono impostate in modo che durandal conosce tutte le visualizzazioni e ViewModel contenuti nelle stesse cartelle denominate, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="568ba-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="568ba-175">Infine, il `app.setRoot` avvia carica il `shell` utilizzando un `entrance` animazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="568ba-176">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="568ba-176">Views</span></span>

<span data-ttu-id="568ba-177">Viste, vedere il `App/views` cartella.</span><span class="sxs-lookup"><span data-stu-id="568ba-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="568ba-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="568ba-178">shell.html</span></span>

<span data-ttu-id="568ba-179">Il `shell.html` contiene il layout dello schema per il codice HTML.</span><span class="sxs-lookup"><span data-stu-id="568ba-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="568ba-180">Tutte le altre viste verranno create in un punto sul lato del `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="568ba-181">Scelta degli asciugamani fornisce un `shell` con tre di queste aree: un'intestazione, un'area di contenuto e un piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="568ba-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="568ba-182">Ognuna di queste aree è caricato con contenuto modulo altre viste quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="568ba-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="568ba-183">Il `compose` binding per l'intestazione e piè di pagina sono codificati in asciugamani frequente in modo che punti al `nav` e `footer` viste, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="568ba-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="568ba-184">L'associazione di composizione per la sezione `#content` è associato il `router` elemento attivo del modulo.</span><span class="sxs-lookup"><span data-stu-id="568ba-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="568ba-185">In altre parole, quando fa clic su un collegamento di navigazione è vista corrispondente viene caricato in questa area.</span><span class="sxs-lookup"><span data-stu-id="568ba-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="568ba-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="568ba-186">nav.html</span></span>

<span data-ttu-id="568ba-187">Il `nav.html` contiene i collegamenti di navigazione per l'autenticazione SPA.</span><span class="sxs-lookup"><span data-stu-id="568ba-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="568ba-188">Si tratta di dove sia possibile posizionare la struttura di menu, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="568ba-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="568ba-189">Spesso si tratta di dati associati (tramite Knockout) per il `router` modulo per visualizzare la navigazione in cui è definito il `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="568ba-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="568ba-190">Knockout ricerca per l'associazione di dati degli attributi e associa questi il `shell` viewmodel per visualizzare gli itinerari di navigazione e per mostrare un progressbar (tramite Twitter Bootstrap) se il `router` modulo è impegnato passare da una vista a altra (vedere `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="568ba-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="568ba-191">Home.HTML e details.html</span><span class="sxs-lookup"><span data-stu-id="568ba-191">home.html and details.html</span></span>

<span data-ttu-id="568ba-192">Queste viste contengono HTML per visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="568ba-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="568ba-193">Quando il `home` collegare il `nav` si fa clic sul menu della visualizzazione, il `home` vista verrà posizionata nell'area del contenuto del `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="568ba-194">Queste viste possono essere aumentate o sostituite con visualizzazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="568ba-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="568ba-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="568ba-195">footer.html</span></span>

<span data-ttu-id="568ba-196">Il `footer.html` contiene codice HTML visualizzato nel piè di pagina, nella parte inferiore del `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="568ba-197">ViewModel</span><span class="sxs-lookup"><span data-stu-id="568ba-197">ViewModels</span></span>

<span data-ttu-id="568ba-198">ViewModel presenti nel `App/viewmodels` cartella.</span><span class="sxs-lookup"><span data-stu-id="568ba-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="568ba-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="568ba-199">shell.js</span></span>

<span data-ttu-id="568ba-200">Il `shell` viewmodel contiene proprietà e funzioni che sono associate ai `shell` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="568ba-201">Spesso si tratta in cui si trovano le associazioni di navigazione di menu (vedere il `router.mapNav` logica).</span><span class="sxs-lookup"><span data-stu-id="568ba-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="568ba-202">Home.js e details.js</span><span class="sxs-lookup"><span data-stu-id="568ba-202">home.js and details.js</span></span>

<span data-ttu-id="568ba-203">Questi ViewModel contengono le proprietà e funzioni che sono associate ai `home` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="568ba-204">inoltre contiene la logica di presentazione per la visualizzazione ed è l'associazione tra i dati e la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="568ba-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="568ba-205">Servizi</span><span class="sxs-lookup"><span data-stu-id="568ba-205">Services</span></span>

<span data-ttu-id="568ba-206">I servizi si trovano nella cartella di App e servizi.</span><span class="sxs-lookup"><span data-stu-id="568ba-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="568ba-207">In teoria è stato possibile collocare i servizi, ad esempio un modulo dataservice, che è responsabile per ottenere e inviare i dati remoti, future.</span><span class="sxs-lookup"><span data-stu-id="568ba-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="568ba-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="568ba-208">logger.js</span></span>

<span data-ttu-id="568ba-209">Scelta degli asciugamani fornisce un `logger` modulo nella cartella servizi.</span><span class="sxs-lookup"><span data-stu-id="568ba-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="568ba-210">Il `logger` modulo è ideale per la registrazione messaggi per la console e per l'utente nel pop-up avvisi popup.</span><span class="sxs-lookup"><span data-stu-id="568ba-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
