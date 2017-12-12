---
uid: single-page-application/overview/templates/emberjs-template
title: Modello EmberJS | Documenti Microsoft
author: xqiu
description: Modello EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a><span data-ttu-id="cc5c3-103">Modello EmberJS</span><span class="sxs-lookup"><span data-stu-id="cc5c3-103">EmberJS template</span></span>
====================
<span data-ttu-id="cc5c3-104">da [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="cc5c3-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="cc5c3-105">Il modello MVC EmberJS scritto da Nathan Totten, Thiago Santos e Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="cc5c3-106">Scaricare il modello MVC EmberJS</span><span class="sxs-lookup"><span data-stu-id="cc5c3-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="cc5c3-107">Il modello di SPA EmberJS è progettato per iniziare a creare rapidamente applicazioni web sul lato client interattive utilizzando EmberJS.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="cc5c3-108">"L'applicazione a pagina singola" (SPA) è il termine generale per un'applicazione web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="cc5c3-109">Dopo il caricamento della pagina iniziale, l'autenticazione SPA comunica con il server tramite le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="cc5c3-110">AJAX è un concetto nuovo, ma esistono oggi Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="cc5c3-111">Inoltre, HTML5 e CSS3 sono rendendo più semplice creare interfacce utente avanzate.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="cc5c3-112">Il modello di SPA EmberJS utilizza il [Ember](http://emberjs.com/) libreria JavaScript di gestire gli aggiornamenti a pagina da richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="cc5c3-113">Ember.js utilizza l'associazione dati per sincronizzare la pagina con i dati più recenti.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="cc5c3-114">In questo modo, non è necessario scrivere codice che esamina i dati JSON e aggiorna il modello DOM.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="cc5c3-115">È invece possibile inserire attributi dichiarativi nel codice HTML che fornisce Ember.js modalità presentare i dati.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="cc5c3-116">Sul lato server, il modello di EmberJS è quasi identica a quella di [modello Knockout.js SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="cc5c3-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="cc5c3-117">Usa ASP.NET MVC per servire i documenti HTML e ASP.NET Web API per gestire le richieste AJAX dal client.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="cc5c3-118">Per ulteriori informazioni su questi aspetti del modello, vedere il [Knockout.js modello](../introduction/knockoutjs-template.md) documentazione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="cc5c3-119">In questo argomento vengono illustrate le differenze tra il modello Knockout e il modello EmberJS.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="cc5c3-120">Creare un progetto di modello SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="cc5c3-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="cc5c3-121">Scaricare e installare il modello fare clic sul pulsante Download.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="cc5c3-122">Si potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="cc5c3-123">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="cc5c3-124">In **Visual c#**selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="cc5c3-125">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="cc5c3-126">Denominare il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="cc5c3-127">Nel **nuovo progetto** procedura guidata, selezionare **Ember.js SPA progetto**.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="cc5c3-128">Cenni preliminari sui modelli SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="cc5c3-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="cc5c3-129">Il modello EmberJS utilizza una combinazione di jQuery, Ember.js, Handlebars.js per creare un'interfaccia utente interattiva uniforme.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="cc5c3-130">Ember.js è una libreria JavaScript che utilizza un modello MVC sul lato client.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="cc5c3-131">Oggetto *modello*scritta nella lingua del modello, i manubri delle descrive l'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="cc5c3-132">In modalità di rilascio, il [compilatore hl](https://github.com/Myslik/csharp-ember-handlebars) viene utilizzato per aggregare e compilare il modello hl.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="cc5c3-133">Oggetto *modello* archivia i dati dell'applicazione che ottiene dal server (elenchi di attività e attività).</span><span class="sxs-lookup"><span data-stu-id="cc5c3-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="cc5c3-134">Oggetto *controller* archivia lo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-134">A *controller* stores application state.</span></span> <span data-ttu-id="cc5c3-135">I controller di presentano i dati del modello per i modelli corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="cc5c3-136">Oggetto *vista* converte gli eventi primitivi dall'applicazione e le passa al controller.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="cc5c3-137">Oggetto *router* gestisce lo stato dell'applicazione, mantenere sincronizzati i modelli e gli URL.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="cc5c3-138">Inoltre, la raccolta dati Ember può essere usata per sincronizzare oggetti JSON (ottenuti dal server tramite un'API RESTful) e i modelli di client.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="cc5c3-139">Il modello di SPA EmberJS organizza gli script in otto livelli:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="cc5c3-140">webapi\_adapter.js, webapi\_serializer.js: estende la libreria Ember dati per l'uso con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="cc5c3-141">Scripts/helpers.js: Definisce nuovi helper i manubri delle Ember.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="cc5c3-142">Scripts/app.js: Crea l'app e configura l'adattatore e il serializzatore.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="cc5c3-143">Gli script o app o imodelli/\*. js: definisce i modelli.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="cc5c3-144">Gli script eapp/viste/\*. js: definisce le viste.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="cc5c3-145">Script/applicazione/controller/\*. js: definisce i controller.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="cc5c3-146">Script/applicazione/route, Scripts/app/router.js: Definisce le route.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="cc5c3-147">Modelli /\*.hbs: definisce i modelli i manubri delle.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="cc5c3-148">Esaminiamo alcuni di questi script in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="cc5c3-149">Modelli</span><span class="sxs-lookup"><span data-stu-id="cc5c3-149">Models</span></span>

<span data-ttu-id="cc5c3-150">I modelli sono definiti nella cartella script o app o i modelli.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="cc5c3-151">Sono disponibili due file di modello: todoItem.js e todoList.js.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="cc5c3-152">**TODO.Model.js** definisce i modelli sul lato client (browser) per gli elenchi di attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="cc5c3-153">Esistono due classi di modello: todoItem e todoList.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="cc5c3-154">In Ember, i modelli sono sottoclassi di dominio Active Directory. Modello.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="cc5c3-155">Un modello può disporre di proprietà con attributi:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="cc5c3-156">I modelli è possono definire relazioni con altri modelli:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="cc5c3-157">I modelli possono sono calcolati delle proprietà associate alle altre:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="cc5c3-158">I modelli possono avere funzioni di osservatore, che vengono richiamate quando viene modificata una proprietà osservata:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="cc5c3-159">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="cc5c3-159">Views</span></span>

<span data-ttu-id="cc5c3-160">Le viste sono definite nella cartella Script/applicazione/visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="cc5c3-161">Una vista converte gli eventi dall'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-161">A view translates events from the application UI.</span></span> <span data-ttu-id="cc5c3-162">Un gestore eventi può richiamare funzioni di controller o semplicemente chiamare direttamente il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="cc5c3-163">Ad esempio, il codice seguente è da views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="cc5c3-164">Definisce la gestione degli eventi per un campo di testo di input.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="cc5c3-165">Controller</span><span class="sxs-lookup"><span data-stu-id="cc5c3-165">Controller</span></span>

<span data-ttu-id="cc5c3-166">I controller sono definiti nella cartella Script/applicazione/controller.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="cc5c3-167">Per rappresentare un singolo modello, è possibile estendere `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="cc5c3-168">Un controller può anche rappresentare una raccolta di modelli tramite l'estensione `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="cc5c3-169">Ad esempio, il TodoListController rappresenta una matrice di `todoList` oggetti.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="cc5c3-170">Il controller vengono ordinati ID elenco attività, in ordine decrescente:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="cc5c3-171">Il controller definisce una funzione denominata `addTodoList`, che crea un nuovo elenco di attività e lo aggiunge alla matrice.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="cc5c3-172">Per visualizzare la modalità con cui questa funzione viene chiamata, aprire il file di modello denominato todoListTemplate.html, nella cartella dei modelli.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="cc5c3-173">Il seguente codice di modello viene associato un pulsante per la `addTodoList` funzione:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="cc5c3-174">Il controller contiene anche un `error` proprietà che contiene un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="cc5c3-175">Di seguito è riportato il codice del modello per visualizzare il messaggio di errore (anche in todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="cc5c3-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="cc5c3-176">Route</span><span class="sxs-lookup"><span data-stu-id="cc5c3-176">Routes</span></span>

<span data-ttu-id="cc5c3-177">Router.js definisce le route e il modello predefinito da visualizzare, impostare lo stato dell'applicazione e corrisponde a URL di route:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="cc5c3-178">Per l'override della funzione setupController TodoListRoute.js carica i dati per il TodoListRoute:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="cc5c3-179">Ember utilizza convenzioni di denominazione per individuare gli URL, i nomi di route, controller e modelli.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="cc5c3-180">Per ulteriori informazioni, vedere [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) la documentazione di EmberJS.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="cc5c3-181">Modelli</span><span class="sxs-lookup"><span data-stu-id="cc5c3-181">Templates</span></span>

<span data-ttu-id="cc5c3-182">La cartella dei modelli contiene quattro modelli:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="cc5c3-183">Application.hbs: il modello predefinito che viene eseguito il rendering quando l'applicazione viene avviata.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="cc5c3-184">About.hbs: il modello per la route "circa /".</span><span class="sxs-lookup"><span data-stu-id="cc5c3-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="cc5c3-185">index.hbs: il modello per la radice route "/".</span><span class="sxs-lookup"><span data-stu-id="cc5c3-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="cc5c3-186">todoList.hbs: il modello per la "/ todo" route.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="cc5c3-187">\_NavBar.hbs: il modello definisce il menu di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="cc5c3-188">Il modello di applicazione funziona come una pagina master.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-188">The application template acts like a master page.</span></span> <span data-ttu-id="cc5c3-189">Contiene un'intestazione, un piè di pagina e "{{presa}}" per inserire gli altri modelli a seconda della route.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="cc5c3-190">Per ulteriori informazioni sui modelli di applicazione in Ember, vedere [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="cc5c3-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="cc5c3-191">Il "/ todoList" modello contiene due espressioni del ciclo.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="cc5c3-192">Il ciclo esterno è `{{#each controller}}`e il ciclo è `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="cc5c3-193">Il codice seguente viene illustrato un oggetto incorporato `Ember.Checkbox` visualizzare, un oggetto personalizzato `App.TodoItemEditView`e un collegamento con un `deleteTodo` azione.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="cc5c3-194">Il `HtmlHelperExtensions` , definita in Controllers/HtmlHelperExensions.cs, definisce un helper funzione memorizzare nella cache e inserire modello file quando **debug** è impostato su **true** nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="cc5c3-195">Questa funzione viene chiamata dal file di visualizzazione ASP.NET MVC definito in Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="cc5c3-196">Chiamato senza argomenti, la funzione esegue il rendering di tutti i file di modello nella cartella dei modelli.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="cc5c3-197">È inoltre possibile specificare una sottocartella o un file di modello specifico.</span><span class="sxs-lookup"><span data-stu-id="cc5c3-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="cc5c3-198">Quando **debug** è **false** in Web. config, l'applicazione include l'elemento di raggruppamento "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="cc5c3-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="cc5c3-199">Questo elemento di raggruppamento viene aggiunto in BundleConfig.cs, utilizzando la libreria del compilatore hl:</span><span class="sxs-lookup"><span data-stu-id="cc5c3-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
