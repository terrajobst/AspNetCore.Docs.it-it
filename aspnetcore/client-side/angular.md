---
title: Utilizzo di AngularJS per applicazioni a pagina singola (stabilimenti termali)
author: rick-anderson
description: Informazioni su come creare un'applicazione ASP.NET SPA stile con AngularJS
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50d2e76c472e67c26238abee4f7b0ed64cd043ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="915d3-104">Utilizzo di AngularJS per applicazioni a pagina singola (stabilimenti termali) con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="915d3-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="915d3-105">Da [Venkata Koppaka](http://blog.falafel.com/author/venkata-koppaka/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="915d3-105">By [Venkata Koppaka](http://blog.falafel.com/author/venkata-koppaka/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="915d3-106">In questo articolo si apprenderà come compilare un'applicazione ASP.NET SPA stile con AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

[<span data-ttu-id="915d3-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="915d3-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)

## <a name="what-is-angularjs"></a><span data-ttu-id="915d3-108">Che cos'è AngularJS?</span><span class="sxs-lookup"><span data-stu-id="915d3-108">What is AngularJS?</span></span>

<span data-ttu-id="915d3-109">[AngularJS](http://angularjs.org/) è un moderno framework JavaScript da Google comunemente utilizzato per l'utilizzo con applicazioni a pagina singola (stabilimenti termali).</span><span class="sxs-lookup"><span data-stu-id="915d3-109">[AngularJS](http://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="915d3-110">AngularJS è open source con licenza MIT e può essere seguito sullo sviluppo di AngularJS [relativo repository GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="915d3-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="915d3-111">La raccolta viene denominata angolare perché HTML Usa parentesi angolare a forma di.</span><span class="sxs-lookup"><span data-stu-id="915d3-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="915d3-112">AngularJS non è una libreria di manipolazione di DOM come jQuery, ma usa un sottoinsieme di jQuery chiamato jQLite.</span><span class="sxs-lookup"><span data-stu-id="915d3-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="915d3-113">AngularJS è basato principalmente su attributi dichiarativi di HTML che è possibile aggiungere i tag HTML.</span><span class="sxs-lookup"><span data-stu-id="915d3-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="915d3-114">È possibile provare AngularJS nel browser utilizzando il [sito Web dell'istituto di istruzione di codice](https://www.codeschool.com/courses/shaping-up-with-angular-js) o [sito Web W3Schools](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="915d3-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angular-js) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="915d3-115">Questo articolo è incentrato su AngularJS con alcune note su diretto in cui angolare.</span><span class="sxs-lookup"><span data-stu-id="915d3-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="915d3-116">Per iniziare</span><span class="sxs-lookup"><span data-stu-id="915d3-116">Getting started</span></span>

<span data-ttu-id="915d3-117">Per iniziare a utilizzare AngularJS nell'applicazione ASP.NET, è necessario installarlo come parte del progetto o farvi riferimento da una rete di distribuzione di contenuti (CDN).</span><span class="sxs-lookup"><span data-stu-id="915d3-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="915d3-118">Installazione</span><span class="sxs-lookup"><span data-stu-id="915d3-118">Installation</span></span>

<span data-ttu-id="915d3-119">Esistono diversi modi per aggiungere AngularJS all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="915d3-120">Se si inizia una nuova applicazione web ASP.NET Core in Visual Studio, è possibile aggiungere AngularJS utilizzando l'elemento predefinito [Bower](bower.md) supportano.</span><span class="sxs-lookup"><span data-stu-id="915d3-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="915d3-121">Aprire *bower. JSON*e aggiungere una voce per il `dependencies` proprietà:</span><span class="sxs-lookup"><span data-stu-id="915d3-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name=angular-bower-json></a>

<span data-ttu-id="915d3-122">[!code-json[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="915d3-122">[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]</span></span>

<span data-ttu-id="915d3-123">Al momento del salvataggio il *bower. JSON* angolare dei file di installazione del progetto *wwwroot/lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="915d3-123">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="915d3-124">Inoltre, verrà elencato all'interno di `Dependencies/Bower` cartella.</span><span class="sxs-lookup"><span data-stu-id="915d3-124">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="915d3-125">Vedere la schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="915d3-125">See the screenshot below.</span></span>

![AngularJS progetto in Esplora soluzioni](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="915d3-127">Successivamente, aggiungere un `<script>` riferimento a fondo il `<body>` sezione della pagina HTML o *layout. cshtml* file, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="915d3-127">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

<span data-ttu-id="915d3-128">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]</span><span class="sxs-lookup"><span data-stu-id="915d3-128">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]</span></span>

<span data-ttu-id="915d3-129">È consigliabile che le applicazioni di produzione usano CDN per le librerie comuni, ad esempio AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-129">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="915d3-130">È possibile fare riferimento a AngularJS da una delle diverse reti CDN, ad esempio questa:</span><span class="sxs-lookup"><span data-stu-id="915d3-130">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

<span data-ttu-id="915d3-131">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]</span><span class="sxs-lookup"><span data-stu-id="915d3-131">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]</span></span>

<span data-ttu-id="915d3-132">Una volta ottenuto un riferimento di *angular.js* file di script, si è pronti per iniziare a usare AngularJS nelle pagine web.</span><span class="sxs-lookup"><span data-stu-id="915d3-132">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="915d3-133">Componenti chiave</span><span class="sxs-lookup"><span data-stu-id="915d3-133">Key components</span></span>

<span data-ttu-id="915d3-134">AngularJS include un numero di componenti principali, ad esempio *direttive*, *modelli*, *controlli Repeater*, *moduli*,  *controller*, *componenti*, *router componente* e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="915d3-134">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="915d3-135">Esaminiamo questi componenti interagiscono per aggiungere un comportamento alle pagine web.</span><span class="sxs-lookup"><span data-stu-id="915d3-135">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="915d3-136">Direttive</span><span class="sxs-lookup"><span data-stu-id="915d3-136">Directives</span></span>

<span data-ttu-id="915d3-137">Usa AngularJS [direttive](https://docs.angularjs.org/guide/directive) estendere HTML con elementi e attributi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="915d3-137">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="915d3-138">Direttive AngularJS sono definite tramite `data-ng-*` o `ng-*` prefissi (`ng` è breve angolare).</span><span class="sxs-lookup"><span data-stu-id="915d3-138">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="915d3-139">Esistono due tipi di direttive AngularJS:</span><span class="sxs-lookup"><span data-stu-id="915d3-139">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="915d3-140">**Direttive primitive**: questi sono già definiti dal team di angolare e fanno parte del framework AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-140">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="915d3-141">**Direttive personalizzate**: si tratta di direttive personalizzate che è possibile definire.</span><span class="sxs-lookup"><span data-stu-id="915d3-141">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="915d3-142">Una delle direttive di primitivi utilizzate in tutte le applicazioni di AngularJS è il `ng-app` direttiva, che avvia l'applicazione di AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-142">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="915d3-143">Questa direttiva può essere applicata al `<body>` tag o a un elemento figlio del corpo.</span><span class="sxs-lookup"><span data-stu-id="915d3-143">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="915d3-144">Di seguito viene illustrato un esempio in azione.</span><span class="sxs-lookup"><span data-stu-id="915d3-144">Let's see an example in action.</span></span> <span data-ttu-id="915d3-145">Supponendo che si trova in un progetto ASP.NET, è possibile aggiungere un file HTML per il `wwwroot` cartella, oppure aggiungere una nuova azione di controller e una visualizzazione associata.</span><span class="sxs-lookup"><span data-stu-id="915d3-145">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="915d3-146">In questo caso, ho aggiunto un nuovo `Directives` metodo di azione da `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="915d3-146">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="915d3-147">La visualizzazione associata è illustrata di seguito:</span><span class="sxs-lookup"><span data-stu-id="915d3-147">The associated view is shown here:</span></span>

<span data-ttu-id="915d3-148">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]</span><span class="sxs-lookup"><span data-stu-id="915d3-148">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]</span></span>

<span data-ttu-id="915d3-149">Per mantenere questi esempi indipendenti una da altra, non si utilizza il file di layout condivise.</span><span class="sxs-lookup"><span data-stu-id="915d3-149">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="915d3-150">È possibile vedere che è decorata con tag body il `ng-app` direttiva per indicare questa pagina è un'applicazione di AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-150">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="915d3-151">Il `{{2+2}}` è un'espressione di associazione di dati angolare ulteriori informazioni in proposito.</span><span class="sxs-lookup"><span data-stu-id="915d3-151">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="915d3-152">Ecco il risultato, se si esegue l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="915d3-152">Here is the result if you run this application:</span></span>

![Direttiva Angular semplice](angular/_static/simple-directive.png)

<span data-ttu-id="915d3-154">Altre direttive primitivi in AngularJS includono:</span><span class="sxs-lookup"><span data-stu-id="915d3-154">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="915d3-155">`ng-controller`Determina quale controller JavaScript è associato alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-155">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="915d3-156">`ng-model`Determina il modello a cui sono associati i valori delle proprietà dell'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="915d3-156">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="915d3-157">`ng-init`Utilizzato per inizializzare i dati dell'applicazione sotto forma di un'espressione per l'ambito corrente.</span><span class="sxs-lookup"><span data-stu-id="915d3-157">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="915d3-158">`ng-if`Rimuove o ricrea l'elemento HTML specificato nel DOM in base a truthiness dell'espressione fornita.</span><span class="sxs-lookup"><span data-stu-id="915d3-158">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="915d3-159">`ng-repeat`Ripete un blocco di HTML specifico su un set di dati.</span><span class="sxs-lookup"><span data-stu-id="915d3-159">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="915d3-160">`ng-show`Mostra o nasconde l'elemento HTML specificato in base all'espressione fornita.</span><span class="sxs-lookup"><span data-stu-id="915d3-160">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="915d3-161">Per un elenco completo di tutte le direttive primitivi supportati in AngularJS, consultare il [una sezione di documentazione direttiva nel sito Web di documentazione AngularJS](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="915d3-161">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="915d3-162">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="915d3-162">Data binding</span></span>

<span data-ttu-id="915d3-163">Fornisce AngularJS [associazione dati](https://docs.angularjs.org/guide/databinding) supporto out-of-the-box utilizzando il `ng-bind` direttiva o un data binding, ad esempio la sintassi dell'espressione `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="915d3-163">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="915d3-164">AngularJS supporta l'associazione bidirezionale dei dati in cui i dati da un modello viene mantenuti in sincronizzazione con un modello di visualizzazione in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="915d3-164">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="915d3-165">Tutte le modifiche alla visualizzazione vengono automaticamente applicate al modello.</span><span class="sxs-lookup"><span data-stu-id="915d3-165">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="915d3-166">Analogamente, tutte le modifiche nel modello vengono riflesse nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-166">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="915d3-167">Creare un file HTML o un'azione del controller con una vista associata denominata `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="915d3-167">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="915d3-168">Nella visualizzazione, includono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="915d3-168">Include the following in the view:</span></span>

<span data-ttu-id="915d3-169">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]</span><span class="sxs-lookup"><span data-stu-id="915d3-169">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]</span></span>

<span data-ttu-id="915d3-170">Si noti che è possibile visualizzare i valori del modello utilizzando l'associazione di direttive o dati (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="915d3-170">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="915d3-171">Nella pagina risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="915d3-171">The resulting page should look like this:</span></span>

![Associazione dati semplice](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="915d3-173">Modelli</span><span class="sxs-lookup"><span data-stu-id="915d3-173">Templates</span></span>

<span data-ttu-id="915d3-174">[Modelli](https://docs.angularjs.org/guide/templates) in AngularJS pagine HTML semplice decorate con direttive AngularJS e gli elementi.</span><span class="sxs-lookup"><span data-stu-id="915d3-174">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="915d3-175">Un modello AngularJS è una combinazione di direttive, espressioni, filtri e i controlli che consentono di combinare in HTML per la visualizzazione form.</span><span class="sxs-lookup"><span data-stu-id="915d3-175">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="915d3-176">Aggiungere un'altra visualizzazione per mostrare i modelli e aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="915d3-176">Add another view to demonstrate templates, and add the following to it:</span></span>

<span data-ttu-id="915d3-177">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]</span><span class="sxs-lookup"><span data-stu-id="915d3-177">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]</span></span>

<span data-ttu-id="915d3-178">Il modello dispone di direttive AngularJS come `ng-app`, `ng-init`, `ng-model` e sintassi di espressione di associazione dati per associare il `personName` proprietà.</span><span class="sxs-lookup"><span data-stu-id="915d3-178">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="915d3-179">In esecuzione nel browser, la visualizzazione è simile nella schermata seguente:</span><span class="sxs-lookup"><span data-stu-id="915d3-179">Running in the browser, the view looks like the screenshot below:</span></span>

![Esempio di modelli semplice 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="915d3-181">Se si modifica il nome digitando nel campo di input, verrà visualizzato il testo accanto al campo di input in modo dinamico aggiornamento, che mostra l'associazione dati bidirezionale angolare in azione.</span><span class="sxs-lookup"><span data-stu-id="915d3-181">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Esempio di modelli semplice 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="915d3-183">Espressioni</span><span class="sxs-lookup"><span data-stu-id="915d3-183">Expressions</span></span>

<span data-ttu-id="915d3-184">[Espressioni](https://docs.angularjs.org/guide/expression) in AngularJS sono frammenti di codice JavaScript che vengono scritte all'interno di `{{ expression }}` sintassi.</span><span class="sxs-lookup"><span data-stu-id="915d3-184">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="915d3-185">L'associazione dei dati da queste espressioni HTML nello stesso modo `ng-bind` direttive.</span><span class="sxs-lookup"><span data-stu-id="915d3-185">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="915d3-186">La differenza principale tra AngularJS espressioni ed espressioni regolari di JavaScript è tale AngularJS espressioni vengono valutate in base il `$scope` oggetto AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-186">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="915d3-187">Le espressioni di AngularJS l'esempio riportato di seguito binding `personName` e un semplice JavaScript calcolati espressione:</span><span class="sxs-lookup"><span data-stu-id="915d3-187">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

<span data-ttu-id="915d3-188">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]</span><span class="sxs-lookup"><span data-stu-id="915d3-188">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]</span></span>

<span data-ttu-id="915d3-189">L'esempio in esecuzione nel browser viene visualizzata la `personName` dati e i risultati del calcolo:</span><span class="sxs-lookup"><span data-stu-id="915d3-189">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Espressioni semplici](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="915d3-191">Controlli Repeater</span><span class="sxs-lookup"><span data-stu-id="915d3-191">Repeaters</span></span>

<span data-ttu-id="915d3-192">Ripetizione di AngularJS viene eseguita tramite una direttiva primitiva chiamata `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="915d3-192">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="915d3-193">Il `ng-repeat` direttiva ripete un elemento HTML specificato in una vista per la durata di una matrice di dati ripetuto.</span><span class="sxs-lookup"><span data-stu-id="915d3-193">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="915d3-194">Controlli Repeater in AngularJS possibile ripetere su una matrice di stringhe o oggetti.</span><span class="sxs-lookup"><span data-stu-id="915d3-194">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="915d3-195">Di seguito è riportato un esempio di utilizzo di ripetizione di una matrice di stringhe:</span><span class="sxs-lookup"><span data-stu-id="915d3-195">Here is a sample usage of repeating over an array of strings:</span></span>

<span data-ttu-id="915d3-196">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]</span><span class="sxs-lookup"><span data-stu-id="915d3-196">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]</span></span>

<span data-ttu-id="915d3-197">Il [repeat (direttiva)](https://docs.angularjs.org/api/ng/directive/ngRepeat) genera una serie di elementi dell'elenco in un elenco non ordinato, come si può notare in strumenti di sviluppo illustrati in questo screenshot:</span><span class="sxs-lookup"><span data-stu-id="915d3-197">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Esempio di Repeater](angular/_static/repeater.png)

<span data-ttu-id="915d3-199">Di seguito è riportato un esempio che si ripete su una matrice di oggetti.</span><span class="sxs-lookup"><span data-stu-id="915d3-199">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="915d3-200">Il `ng-init` direttiva stabilisce un `names` matrice, in cui ogni elemento è un oggetto contenente prima e il cognome.</span><span class="sxs-lookup"><span data-stu-id="915d3-200">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="915d3-201">Il `ng-repeat` assegnazione, `name in names`, restituisce un elemento di elenco per ogni elemento della matrice.</span><span class="sxs-lookup"><span data-stu-id="915d3-201">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

<span data-ttu-id="915d3-202">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]</span><span class="sxs-lookup"><span data-stu-id="915d3-202">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]</span></span>

<span data-ttu-id="915d3-203">In questo caso l'output è uguale a quello dell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="915d3-203">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="915d3-204">Angolare fornisce alcune direttive aggiuntive che consentono di fornire un comportamento in base a dove il ciclo è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="915d3-204">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="915d3-205">Utilizzare `$index` nel `ng-repeat` ciclo per determinare quale indice posizionare il ciclo attualmente è in.</span><span class="sxs-lookup"><span data-stu-id="915d3-205">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="915d3-206">`$even` e `$odd`</span><span class="sxs-lookup"><span data-stu-id="915d3-206">`$even` and `$odd`</span></span>

<span data-ttu-id="915d3-207">Utilizzare `$even` nel `ng-repeat` ciclo per determinare se l'indice nel ciclo corrente è una riga anche indicizzata.</span><span class="sxs-lookup"><span data-stu-id="915d3-207">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="915d3-208">Analogamente, utilizzare `$odd` per determinare se l'indice corrente è una riga indicizzata dispari.</span><span class="sxs-lookup"><span data-stu-id="915d3-208">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="915d3-209">`$first` e `$last`</span><span class="sxs-lookup"><span data-stu-id="915d3-209">`$first` and `$last`</span></span>

<span data-ttu-id="915d3-210">Utilizzare `$first` nel `ng-repeat` ciclo per determinare se l'indice nel ciclo corrente è la prima riga.</span><span class="sxs-lookup"><span data-stu-id="915d3-210">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="915d3-211">Analogamente, utilizzare `$last` per determinare se l'indice corrente è l'ultima riga.</span><span class="sxs-lookup"><span data-stu-id="915d3-211">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="915d3-212">Di seguito è riportato un esempio che illustra `$index`, `$even`, `$odd`, `$first`, e `$last` in azione:</span><span class="sxs-lookup"><span data-stu-id="915d3-212">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

<span data-ttu-id="915d3-213">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]</span><span class="sxs-lookup"><span data-stu-id="915d3-213">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]</span></span>

<span data-ttu-id="915d3-214">Di seguito è riportato l'output risultante:</span><span class="sxs-lookup"><span data-stu-id="915d3-214">Here is the resulting output:</span></span>

![Esempio Ripetitore 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="915d3-216">$scope</span><span class="sxs-lookup"><span data-stu-id="915d3-216">$scope</span></span>

<span data-ttu-id="915d3-217">`$scope`è un oggetto JavaScript che fungono da collante tra la visualizzazione (modello) e il controller (come illustrato di seguito).</span><span class="sxs-lookup"><span data-stu-id="915d3-217">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="915d3-218">Un modello di visualizzazione in AngularJS viene a conoscenza solo i valori collegati al `$scope` oggetto nel controller.</span><span class="sxs-lookup"><span data-stu-id="915d3-218">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="915d3-219">Nel mondo, MVVM il `$scope` oggetto AngularJS è spesso definito come il ViewModel.</span><span class="sxs-lookup"><span data-stu-id="915d3-219">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="915d3-220">Il team di AngularJS si intende il `$scope` oggetto come modello di dati.</span><span class="sxs-lookup"><span data-stu-id="915d3-220">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="915d3-221">[Altre informazioni sugli ambiti di AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="915d3-221">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="915d3-222">Di seguito è riportato un esempio semplice che illustra come impostare le proprietà delle `$scope` all'interno di un file JavaScript separato, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="915d3-222">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

<span data-ttu-id="915d3-223">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="915d3-223">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]</span></span>

<span data-ttu-id="915d3-224">Osservare il `$scope` parametro passato per il controller nella riga 2.</span><span class="sxs-lookup"><span data-stu-id="915d3-224">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="915d3-225">Questo oggetto è ciò che la vista viene a conoscenza.</span><span class="sxs-lookup"><span data-stu-id="915d3-225">This object is what the view knows about.</span></span> <span data-ttu-id="915d3-226">Nella riga 3, si sta impostando una proprietà denominata "name" a "Maria Jane".</span><span class="sxs-lookup"><span data-stu-id="915d3-226">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="915d3-227">Cosa accade quando una determinata proprietà non viene trovata la vista?</span><span class="sxs-lookup"><span data-stu-id="915d3-227">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="915d3-228">La vista definita di seguito fa riferimento alla proprietà "name" e "age":</span><span class="sxs-lookup"><span data-stu-id="915d3-228">The view defined below refers to "name" and "age" properties:</span></span>

<span data-ttu-id="915d3-229">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]</span><span class="sxs-lookup"><span data-stu-id="915d3-229">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]</span></span>

<span data-ttu-id="915d3-230">Notare che alla riga 9 che viene richiesto di angolare per visualizzare la proprietà "name" utilizzando la sintassi dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="915d3-230">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="915d3-231">Riga 10 fa quindi riferimento a "age", una proprietà che non esiste.</span><span class="sxs-lookup"><span data-stu-id="915d3-231">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="915d3-232">L'esempio mostra il nome impostato su "Mary Jane" e non per età.</span><span class="sxs-lookup"><span data-stu-id="915d3-232">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="915d3-233">Proprietà mancanti vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="915d3-233">Missing properties are ignored.</span></span>

![Esempio di ambito](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="915d3-235">Moduli</span><span class="sxs-lookup"><span data-stu-id="915d3-235">Modules</span></span>

<span data-ttu-id="915d3-236">Oggetto [modulo](https://docs.angularjs.org/guide/module) in AngularJS è una raccolta di controller di servizi, le direttive, e così via. Il `angular.module()` chiamata di funzione viene utilizzata per creare, registrare e recuperare i moduli AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-236">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="915d3-237">Tutti i moduli, inclusi quelli forniti dal team di AngularJS e librerie di terze parti, devono essere registrati tramite il `angular.module()` (funzione).</span><span class="sxs-lookup"><span data-stu-id="915d3-237">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="915d3-238">Di seguito è riportato un frammento di codice che illustra come creare un nuovo modulo in AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-238">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="915d3-239">Il primo parametro è il nome del modulo.</span><span class="sxs-lookup"><span data-stu-id="915d3-239">The first parameter is the name of the module.</span></span> <span data-ttu-id="915d3-240">Il secondo parametro definisce le dipendenze in altri moduli.</span><span class="sxs-lookup"><span data-stu-id="915d3-240">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="915d3-241">Più avanti in questo articolo è mostrerà come passare queste dipendenze per un `angular.module()` chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="915d3-241">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="915d3-242">Utilizzare il `ng-app` direttiva per rappresentare un modulo AngularJS nella pagina.</span><span class="sxs-lookup"><span data-stu-id="915d3-242">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="915d3-243">Per usare un modulo, assegnare il nome del modulo, `personApp` in questo esempio, per il `ng-app` direttiva di modelli.</span><span class="sxs-lookup"><span data-stu-id="915d3-243">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="915d3-244">Controller</span><span class="sxs-lookup"><span data-stu-id="915d3-244">Controllers</span></span>

<span data-ttu-id="915d3-245">[Controller](https://docs.angularjs.org/guide/controller) in AngularJS sono il primo punto di ingresso per il codice.</span><span class="sxs-lookup"><span data-stu-id="915d3-245">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="915d3-246">Il `<module name>.controller()` chiamata di funzione viene utilizzata per creare e registrare i controller in AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-246">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="915d3-247">Il `ng-controller` direttiva viene utilizzata per rappresentare un controller AngularJS nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="915d3-247">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="915d3-248">Il ruolo del controller nel angolare consiste nell'impostare lo stato e il comportamento del modello di dati (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="915d3-248">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="915d3-249">Controller non deve essere utilizzati per modificare direttamente il DOM.</span><span class="sxs-lookup"><span data-stu-id="915d3-249">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="915d3-250">Di seguito è riportato un frammento di codice che registra un nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="915d3-250">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="915d3-251">Il `personApp` variabile nel frammento di fa riferimento a un modulo angolare, definito nella riga 2.</span><span class="sxs-lookup"><span data-stu-id="915d3-251">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

<span data-ttu-id="915d3-252">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]</span><span class="sxs-lookup"><span data-stu-id="915d3-252">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]</span></span>

<span data-ttu-id="915d3-253">La vista che utilizza il `ng-controller` direttiva assegna il nome del controller:</span><span class="sxs-lookup"><span data-stu-id="915d3-253">The view using the `ng-controller` directive assigns the controller name:</span></span>

<span data-ttu-id="915d3-254">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]</span><span class="sxs-lookup"><span data-stu-id="915d3-254">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]</span></span>

<span data-ttu-id="915d3-255">Nella pagina viene visualizzato "Maria" e "Jane" che corrispondono al `firstName` e `lastName` proprietà collegata la `$scope` oggetto:</span><span class="sxs-lookup"><span data-stu-id="915d3-255">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Esempio di controller](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="915d3-257">Componenti</span><span class="sxs-lookup"><span data-stu-id="915d3-257">Components</span></span>

<span data-ttu-id="915d3-258">[Componenti](https://docs.angularjs.org/guide/component) in angolare 1.5 consentire per l'incapsulamento e la capacità di creazione di singoli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="915d3-258">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="915d3-259">In 1.4.x angolare è possibile ottenere la stessa funzionalità utilizzando il metodo .directive().</span><span class="sxs-lookup"><span data-stu-id="915d3-259">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="915d3-260">Tramite il metodo .component(), lo sviluppo è stato semplificato ottenere la funzionalità della direttiva e il controller.</span><span class="sxs-lookup"><span data-stu-id="915d3-260">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="915d3-261">Altri vantaggi includono; isolamento dell'ambito, le procedure consigliate sono intrinseche e la migrazione a 2 angolare diventa un'attività semplice.</span><span class="sxs-lookup"><span data-stu-id="915d3-261">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="915d3-262">Il `<module name>.component()` chiamata di funzione viene utilizzata per creare e registrare i componenti in AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-262">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="915d3-263">Di seguito è riportato un frammento di codice che registra un nuovo componente.</span><span class="sxs-lookup"><span data-stu-id="915d3-263">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="915d3-264">Il `personApp` variabile nel frammento di fa riferimento a un modulo angolare, definito nella riga 2.</span><span class="sxs-lookup"><span data-stu-id="915d3-264">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

<span data-ttu-id="915d3-265">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]</span><span class="sxs-lookup"><span data-stu-id="915d3-265">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]</span></span>

<span data-ttu-id="915d3-266">La vista in cui viene visualizzato l'elemento HTML personalizzato.</span><span class="sxs-lookup"><span data-stu-id="915d3-266">The view where we are displaying the custom HTML element.</span></span>

<span data-ttu-id="915d3-267">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="915d3-267">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]</span></span>

<span data-ttu-id="915d3-268">Il modello associato utilizzato dal componente:</span><span class="sxs-lookup"><span data-stu-id="915d3-268">The associated template used by component:</span></span>

<span data-ttu-id="915d3-269">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="915d3-269">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]</span></span>

<span data-ttu-id="915d3-270">Nella pagina viene visualizzato "Aftab" e "Ansari" che corrispondono al `firstName` e `lastName` proprietà collegata la `vm` oggetto:</span><span class="sxs-lookup"><span data-stu-id="915d3-270">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Esempio di componenti](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="915d3-272">Servizi</span><span class="sxs-lookup"><span data-stu-id="915d3-272">Services</span></span>

<span data-ttu-id="915d3-273">[Servizi](https://docs.angularjs.org/guide/services) in AngularJS sono in genere utilizzati per il codice condiviso che viene estratto in un file che può essere utilizzato per tutta la durata di un'applicazione angolare.</span><span class="sxs-lookup"><span data-stu-id="915d3-273">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="915d3-274">Servizi in modo differito creazione di istanze, vale a dire che non sarà presente un'istanza di un servizio a meno che non viene utilizzato un componente che dipende dal servizio.</span><span class="sxs-lookup"><span data-stu-id="915d3-274">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="915d3-275">Factory sono un esempio di servizio utilizzato nelle applicazioni di AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-275">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="915d3-276">Factory vengono create utilizzando il `myApp.factory()` chiamata di funzione, in cui `myApp` è il modulo.</span><span class="sxs-lookup"><span data-stu-id="915d3-276">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="915d3-277">Di seguito è riportato un esempio che illustra come usare una factory di AngularJS:</span><span class="sxs-lookup"><span data-stu-id="915d3-277">Below is an example that shows how to use factories in AngularJS:</span></span>

<span data-ttu-id="915d3-278">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="915d3-278">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]</span></span>

<span data-ttu-id="915d3-279">Per chiamare questa factory dal controller, passare `personFactory` come parametro per il `controller` funzione:</span><span class="sxs-lookup"><span data-stu-id="915d3-279">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="915d3-280">Utilizzo dei servizi per comunicare con un endpoint REST</span><span class="sxs-lookup"><span data-stu-id="915d3-280">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="915d3-281">Di seguito è riportato un esempio end-to-end utilizzando servizi in AngularJS per interagire con un endpoint API Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="915d3-281">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="915d3-282">L'esempio ottiene i dati dall'API Web e visualizza i dati in un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-282">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="915d3-283">Iniziamo con la vista prima:</span><span class="sxs-lookup"><span data-stu-id="915d3-283">Let's start with the view first:</span></span>

<span data-ttu-id="915d3-284">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]</span><span class="sxs-lookup"><span data-stu-id="915d3-284">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]</span></span>

<span data-ttu-id="915d3-285">In questa vista, si dispone di un modulo angolare chiamato `PersonsApp` e un controller denominato `personController`.</span><span class="sxs-lookup"><span data-stu-id="915d3-285">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="915d3-286">Si sta usando `ng-repeat` per scorrere l'elenco di persone.</span><span class="sxs-lookup"><span data-stu-id="915d3-286">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="915d3-287">Ci stiamo che fanno riferimento a tre file JavaScript personalizzati nelle righe 17-19.</span><span class="sxs-lookup"><span data-stu-id="915d3-287">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="915d3-288">Il *personApp.js* file viene utilizzato per registrare il `PersonsApp` modulo; e la sintassi è simile agli esempi precedenti.</span><span class="sxs-lookup"><span data-stu-id="915d3-288">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="915d3-289">Si sta usando il `angular.module` funzione per creare una nuova istanza del modulo che verrà utilizzato con.</span><span class="sxs-lookup"><span data-stu-id="915d3-289">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

<span data-ttu-id="915d3-290">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="915d3-290">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]</span></span>

<span data-ttu-id="915d3-291">Esaminiamo un *personFactory.js*riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="915d3-291">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="915d3-292">Qui viene chiamato il modulo `factory` metodo per creare una factory.</span><span class="sxs-lookup"><span data-stu-id="915d3-292">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="915d3-293">La riga 12 mostra l'angolare incorporato `$http` servizio recupero delle informazioni di utenti da un servizio web.</span><span class="sxs-lookup"><span data-stu-id="915d3-293">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

<span data-ttu-id="915d3-294">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]</span><span class="sxs-lookup"><span data-stu-id="915d3-294">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]</span></span>

<span data-ttu-id="915d3-295">In *personController.js*, venga chiamato il modulo `controller` metodo per creare il controller.</span><span class="sxs-lookup"><span data-stu-id="915d3-295">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="915d3-296">Il `$scope` dell'oggetto `people` proprietà viene assegnato ai dati restituiti da personFactory (riga 13).</span><span class="sxs-lookup"><span data-stu-id="915d3-296">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

<span data-ttu-id="915d3-297">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]</span><span class="sxs-lookup"><span data-stu-id="915d3-297">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]</span></span>

<span data-ttu-id="915d3-298">Diamo un'occhiata veloce l'API Web e il modello sottostante.</span><span class="sxs-lookup"><span data-stu-id="915d3-298">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="915d3-299">Il `Person` modello è un POCO (Plain oggetto CLR precedente) con `Id`, `FirstName`, e `LastName` proprietà:</span><span class="sxs-lookup"><span data-stu-id="915d3-299">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

<span data-ttu-id="915d3-300">[!code-csharp[Principale](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]</span><span class="sxs-lookup"><span data-stu-id="915d3-300">[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]</span></span>

<span data-ttu-id="915d3-301">Il `Person` controller restituisce un elenco in formato JSON di `Person` oggetti:</span><span class="sxs-lookup"><span data-stu-id="915d3-301">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

<span data-ttu-id="915d3-302">[!code-csharp[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]</span><span class="sxs-lookup"><span data-stu-id="915d3-302">[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]</span></span>

<span data-ttu-id="915d3-303">Di seguito viene illustrato il funzionamento dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="915d3-303">Let's see the application in action:</span></span>

![Controller visualizzare altri risultati](angular/_static/rest-bound.png)

<span data-ttu-id="915d3-305">È possibile [consente di visualizzare la struttura dell'applicazione su GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="915d3-305">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="915d3-306">Per ulteriori informazioni su come strutturare applicazioni AngularJS, vedere [Guida della John Papa di stile Angular](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="915d3-306">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="915d3-307">Per creare un modulo AngularJS, controller, factory, file direttiva e visualizzare facilmente, assicurarsi di estrarre Sayed Hashimi [SideWaffle pack dei modelli di Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="915d3-307">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="915d3-308">Hashimi sayed è un Senior Program Manager del Team di Visual Studio Web in Microsoft e i modelli di SideWaffle sono considerati lo standard.</span><span class="sxs-lookup"><span data-stu-id="915d3-308">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="915d3-309">Al momento della redazione del presente documento, è disponibile per Visual Studio 2012, 2013 e 2015 SideWaffle.</span><span class="sxs-lookup"><span data-stu-id="915d3-309">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="915d3-310">Routing e visualizzazioni multiple</span><span class="sxs-lookup"><span data-stu-id="915d3-310">Routing and multiple views</span></span>

<span data-ttu-id="915d3-311">AngularJS dispone di un provider di route predefinite per la navigazione di base di SPA (applicazione a pagina singola).</span><span class="sxs-lookup"><span data-stu-id="915d3-311">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="915d3-312">Per lavorare con il routing di AngularJS, è necessario aggiungere il `angular-route` libreria tramite Bower.</span><span class="sxs-lookup"><span data-stu-id="915d3-312">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="915d3-313">È possibile visualizzare nel [bower. JSON](#angular-bower-json) file a cui fa riferimento all'inizio di questo articolo che si sta già farvi riferimento nel progetto.</span><span class="sxs-lookup"><span data-stu-id="915d3-313">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="915d3-314">Dopo aver installato il pacchetto, aggiungere il riferimento allo script (*angolare route.js*) alla propria visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-314">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="915d3-315">Ora prendiamo l'App di persona che si sta compilando e aggiungere la navigazione a esso.</span><span class="sxs-lookup"><span data-stu-id="915d3-315">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="915d3-316">In primo luogo, si creerà una copia dell'app mediante la creazione di un nuovo `PeopleController` azione denominata `Spa` e un oggetto corrispondente `Spa.cshtml` visualizzazione copiando la vista cshtml il `People` cartella.</span><span class="sxs-lookup"><span data-stu-id="915d3-316">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="915d3-317">Aggiungere un riferimento allo script `angular-route` (vedere la riga 11).</span><span class="sxs-lookup"><span data-stu-id="915d3-317">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="915d3-318">Aggiungere anche un `div` contrassegnato con il `ng-view` (direttiva) (vedere la riga 6) come segnaposto per inserire le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="915d3-318">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="915d3-319">Occorre disporre di diversi altri *. js* file a cui fanno riferimento alle righe 13-16.</span><span class="sxs-lookup"><span data-stu-id="915d3-319">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

<span data-ttu-id="915d3-320">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]</span><span class="sxs-lookup"><span data-stu-id="915d3-320">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]</span></span>

<span data-ttu-id="915d3-321">Esaminiamo un *personModule.js* file per vedere come si sta creando un'istanza il modulo con il routing.</span><span class="sxs-lookup"><span data-stu-id="915d3-321">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="915d3-322">Viene passato `ngRoute` come libreria nel modulo.</span><span class="sxs-lookup"><span data-stu-id="915d3-322">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="915d3-323">Questo modulo gestisce routing nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-323">This module handles routing in our application.</span></span>

<span data-ttu-id="915d3-324">[!code-javascript[Principale](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]</span><span class="sxs-lookup"><span data-stu-id="915d3-324">[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]</span></span>

<span data-ttu-id="915d3-325">Il *personRoutes.js* file, di seguito, definisce le route in base al provider di route.</span><span class="sxs-lookup"><span data-stu-id="915d3-325">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="915d3-326">Definiscono la navigazione pronunciando in modo efficace, quando un URL con righe 4-7 `/persons` è richiesto, utilizzare un modello denominato `partials/personlist` affrontando `personListController`.</span><span class="sxs-lookup"><span data-stu-id="915d3-326">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="915d3-327">Le righe da 8 a 11 indicano una pagina di dettaglio con un parametro di route di `personId`.</span><span class="sxs-lookup"><span data-stu-id="915d3-327">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="915d3-328">Se l'URL non corrisponde a uno dei modelli, angolare per impostazione predefinita il `/persons` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="915d3-328">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

<span data-ttu-id="915d3-329">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]</span><span class="sxs-lookup"><span data-stu-id="915d3-329">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]</span></span>

<span data-ttu-id="915d3-330">Il `personlist.html` file è una visualizzazione parziale contenente solo il codice HTML necessario per la visualizzazione elenco.</span><span class="sxs-lookup"><span data-stu-id="915d3-330">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

<span data-ttu-id="915d3-331">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="915d3-331">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]</span></span>

<span data-ttu-id="915d3-332">Il controller viene definito mediante il modulo `controller` funzionare in *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="915d3-332">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

<span data-ttu-id="915d3-333">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="915d3-333">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]</span></span>

<span data-ttu-id="915d3-334">Se si esegue l'applicazione e passare il `people/spa#/persons` URL, verrà spiegato:</span><span class="sxs-lookup"><span data-stu-id="915d3-334">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Visualizzazione elenco di persone](angular/_static/spa-persons.png)

<span data-ttu-id="915d3-336">Se si passa a una pagina di dettaglio, ad esempio `people/spa#/persons/2`, vedremo della visualizzazione parziale di dettaglio:</span><span class="sxs-lookup"><span data-stu-id="915d3-336">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Visualizzazione di dettagli utente](angular/_static/spa-persons-2.png)

<span data-ttu-id="915d3-338">È possibile visualizzare il codice sorgente completo e tutti i file non visualizzati in questo articolo [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="915d3-338">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="915d3-339">Gestori eventi</span><span class="sxs-lookup"><span data-stu-id="915d3-339">Event Handlers</span></span>

<span data-ttu-id="915d3-340">Esistono un numero di direttive di AngularJS che aggiungono funzionalità di gestione degli eventi per gli elementi di input in DOM. l'HTML</span><span class="sxs-lookup"><span data-stu-id="915d3-340">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="915d3-341">Di seguito è un elenco di eventi che vengono compilati in AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-341">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="915d3-342">È possibile aggiungere i propri gestori di eventi utilizzando il [direttive personalizzate funzionalità in AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="915d3-342">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="915d3-343">Vengono descritte le modalità di `ng-click` è collegato l'evento.</span><span class="sxs-lookup"><span data-stu-id="915d3-343">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="915d3-344">Creare un nuovo file JavaScript denominato *eventHandlerController.js*e aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="915d3-344">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

<span data-ttu-id="915d3-345">[!code-javascript[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]</span><span class="sxs-lookup"><span data-stu-id="915d3-345">[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]</span></span>

<span data-ttu-id="915d3-346">Si noti il nuovo `sayName` funzionare in `eventHandlerController` nella riga 5 riportato sopra.</span><span class="sxs-lookup"><span data-stu-id="915d3-346">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="915d3-347">Esegue tutti il metodo per l'ora viene visualizzato un avviso di JavaScript per l'utente con un messaggio di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="915d3-347">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="915d3-348">La vista seguente associa una funzione di controller per un evento di AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-348">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="915d3-349">Riga 9 dispone di un pulsante su cui il `ng-click` direttiva Angular è stata applicata.</span><span class="sxs-lookup"><span data-stu-id="915d3-349">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="915d3-350">Chiama il nostro `sayName` funzione, a cui è associato il `$scope` oggetto passato a questa vista.</span><span class="sxs-lookup"><span data-stu-id="915d3-350">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

<span data-ttu-id="915d3-351">[!code-html[Principale](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="915d3-351">[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]</span></span>

<span data-ttu-id="915d3-352">L'esempio dimostra che il controller `sayName` funzione viene chiamata automaticamente quando si fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="915d3-352">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click (evento)](angular/_static/events.png)

<span data-ttu-id="915d3-354">Per ulteriori informazioni sulle direttive di gestore eventi predefinito AngularJS, assicurarsi di intestazione per il [sito Web di documentazione](https://docs.angularjs.org/api/ng/directive/ngClick) di AngularJS.</span><span class="sxs-lookup"><span data-stu-id="915d3-354">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="915d3-355">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="915d3-355">Additional resources</span></span>

* [<span data-ttu-id="915d3-356">Documenti Angular</span><span class="sxs-lookup"><span data-stu-id="915d3-356">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="915d3-357">Info 2 Angular</span><span class="sxs-lookup"><span data-stu-id="915d3-357">Angular 2 Info</span></span>](http://angular.io)
