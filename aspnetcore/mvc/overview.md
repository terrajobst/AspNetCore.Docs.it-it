---
title: Panoramica di ASP.NET MVC
author: ardalis
description: "Informazioni su come Core di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 33c293e15c0a7f18bbace9dc564fe11d93a7d509
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="94092-104">Panoramica di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="94092-104">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="94092-105">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="94092-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="94092-106">Componenti di base di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API.</span><span class="sxs-lookup"><span data-stu-id="94092-106">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="94092-107">Che cos'è il modello MVC?</span><span class="sxs-lookup"><span data-stu-id="94092-107">What is the MVC pattern?</span></span>

<span data-ttu-id="94092-108">Il modello architetturale Model-View-Controller (MVC) suddivide un'applicazione in tre gruppi principali di componenti: modelli, visualizzazioni e controller.</span><span class="sxs-lookup"><span data-stu-id="94092-108">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="94092-109">Questo modello consente di ottenere [separazione delle problematiche](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="94092-109">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="94092-110">Usando questo modello, le richieste degli utenti vengono indirizzate a un Controller che è responsabile per l'utilizzo del modello per eseguire le azioni dell'utente e/o recuperare i risultati delle query.</span><span class="sxs-lookup"><span data-stu-id="94092-110">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="94092-111">Il Controller sceglie visualizzazione da mostrare all'utente e fornisce i dati del modello che richiede.</span><span class="sxs-lookup"><span data-stu-id="94092-111">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="94092-112">Il diagramma seguente illustra i tre componenti principali e quelle che fanno riferimento gli altri:</span><span class="sxs-lookup"><span data-stu-id="94092-112">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modello MVC](overview/_static/mvc.png)

<span data-ttu-id="94092-114">Questo limite delle responsabilità consente di scalare l'applicazione in termini di complessità, perché è più facile da codice, eseguire il debug e testare un elemento (modello, visualizzazione o controller) con un singolo processo (e segue il [singolo principio di responsabilità ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="94092-114">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="94092-115">È più difficile da aggiornare, test e il debug del codice con dipendenze distribuiti in due o più di queste tre aree.</span><span class="sxs-lookup"><span data-stu-id="94092-115">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="94092-116">Ad esempio, logica dell'interfaccia utente tende a cambiano più frequentemente logica di business.</span><span class="sxs-lookup"><span data-stu-id="94092-116">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="94092-117">Se la logica di presentazione di codice e di business viene combinata in un singolo oggetto, è necessario modificare un oggetto che contiene la logica di business, ogni volta che si modifica l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="94092-117">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="94092-118">Questo problema può causare errori e richiede la riesecuzione del test di tutte le regole di business dopo ogni interfaccia utente minima modificato.</span><span class="sxs-lookup"><span data-stu-id="94092-118">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="94092-119">Sia la visualizzazione e il controller dipendono dal modello.</span><span class="sxs-lookup"><span data-stu-id="94092-119">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="94092-120">Tuttavia, il modello dipende la vista né il controller.</span><span class="sxs-lookup"><span data-stu-id="94092-120">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="94092-121">Questo è uno dei principali vantaggi di separazione.</span><span class="sxs-lookup"><span data-stu-id="94092-121">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="94092-122">Questa separazione consente la presentazione visiva indipendente da compilare e testare il modello.</span><span class="sxs-lookup"><span data-stu-id="94092-122">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="94092-123">Responsabilità di modello</span><span class="sxs-lookup"><span data-stu-id="94092-123">Model Responsibilities</span></span>

<span data-ttu-id="94092-124">Il modello in un'applicazione MVC rappresenta lo stato dell'applicazione e qualsiasi logica di business o di operazioni che devono essere eseguite da esso.</span><span class="sxs-lookup"><span data-stu-id="94092-124">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="94092-125">Logica di business deve essere incapsulata nel modello, insieme a qualsiasi logica di implementazione per la persistenza dello stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="94092-125">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="94092-126">Visualizzazioni fortemente tipizzate utilizzano in genere i tipi di ViewModel progettati per contenere i dati da visualizzare per tale vista.</span><span class="sxs-lookup"><span data-stu-id="94092-126">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="94092-127">Il controller crea e popola le istanze di ViewModel dal modello.</span><span class="sxs-lookup"><span data-stu-id="94092-127">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="94092-128">Esistono diversi modi per organizzare il modello in un'applicazione che utilizza il modello architetturale MVC.</span><span class="sxs-lookup"><span data-stu-id="94092-128">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="94092-129">Altre informazioni su alcune [diversi tipi di modello](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="94092-129">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="94092-130">Visualizza responsabilità</span><span class="sxs-lookup"><span data-stu-id="94092-130">View Responsibilities</span></span>

<span data-ttu-id="94092-131">Le visualizzazioni sono responsabili per la presentazione di contenuto tramite l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="94092-131">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="94092-132">Usano il [motore di visualizzazione Razor](#razor-view-engine) per incorporare il codice .NET nel markup HTML.</span><span class="sxs-lookup"><span data-stu-id="94092-132">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="94092-133">Dovrebbe esserci minimo logica all'interno di viste e qualsiasi logica in essi contenuti devono essere correlati alla presentazione di contenuto.</span><span class="sxs-lookup"><span data-stu-id="94092-133">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="94092-134">Se si rileva la necessità di eseguire una notevole della logica di visualizzazione di file per visualizzare i dati da un modello complesso, è consigliabile utilizzare un [del componente di visualizzazione](views/view-components.md), ViewModel, o del modello di visualizzazione per semplificare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="94092-134">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="94092-135">Responsabilità controller</span><span class="sxs-lookup"><span data-stu-id="94092-135">Controller Responsibilities</span></span>

<span data-ttu-id="94092-136">I controller sono i componenti che gestiscono l'interazione dell'utente, utilizzano il modello e selezionano infine una visualizzazione per il rendering.</span><span class="sxs-lookup"><span data-stu-id="94092-136">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="94092-137">In un'applicazione MVC, Visualizza solo informazioni; il controller gestisce e risponde all'input dell'utente e l'interazione.</span><span class="sxs-lookup"><span data-stu-id="94092-137">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="94092-138">Nel modello MVC, è il punto di ingresso iniziale, il controller ed è responsabile per selezionare il modello di tipi da utilizzare e la visualizzazione per il rendering (pertanto il relativo nome - controlla la modalità dell'app risponde a una determinata richiesta).</span><span class="sxs-lookup"><span data-stu-id="94092-138">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="94092-139">Controller dovrebbero non essere eccessivamente complessa da un numero eccessivo di responsabilità.</span><span class="sxs-lookup"><span data-stu-id="94092-139">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="94092-140">Per mantenere la logica di controller di diventare eccessivamente complessa, utilizzare il [singolo responsabilità principio](http://deviq.com/single-responsibility-principle/) alla logica di business di push fuori il controller e nel modello di dominio.</span><span class="sxs-lookup"><span data-stu-id="94092-140">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="94092-141">Se si ritiene che le azioni del controller eseguono frequentemente gli stessi tipi di azioni, è possibile seguire il [non ripetere manualmente principio](http://deviq.com/don-t-repeat-yourself/) spostando queste azioni comuni in [filtri](#filters).</span><span class="sxs-lookup"><span data-stu-id="94092-141">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="94092-142">Che cos'è ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="94092-142">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="94092-143">Il framework ASP.NET MVC di base è un semplice open source, il framework di presentazione testabili altamente ottimizzato per l'utilizzo con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="94092-143">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="94092-144">Componenti di base di ASP.NET MVC fornisce basate su modelli consentono di creare siti Web dinamici che consente una netta separazione delle problematiche.</span><span class="sxs-lookup"><span data-stu-id="94092-144">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="94092-145">Fornisce un controllo completo sul markup, supporta basto e utilizza gli standard web più recenti.</span><span class="sxs-lookup"><span data-stu-id="94092-145">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="94092-146">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="94092-146">Features</span></span>

<span data-ttu-id="94092-147">Componenti di base di ASP.NET MVC include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="94092-147">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="94092-148">Routing</span><span class="sxs-lookup"><span data-stu-id="94092-148">Routing</span></span>](#routing)
* [<span data-ttu-id="94092-149">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="94092-149">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="94092-150">Convalida modello</span><span class="sxs-lookup"><span data-stu-id="94092-150">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="94092-151">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="94092-151">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="94092-152">Filtri</span><span class="sxs-lookup"><span data-stu-id="94092-152">Filters</span></span>](#filters)
* [<span data-ttu-id="94092-153">Aree</span><span class="sxs-lookup"><span data-stu-id="94092-153">Areas</span></span>](#areas)
* [<span data-ttu-id="94092-154">API Web</span><span class="sxs-lookup"><span data-stu-id="94092-154">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="94092-155">Testabilità</span><span class="sxs-lookup"><span data-stu-id="94092-155">Testability</span></span>](#testability)
* [<span data-ttu-id="94092-156">Motore di visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="94092-156">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="94092-157">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="94092-157">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="94092-158">Helper tag</span><span class="sxs-lookup"><span data-stu-id="94092-158">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="94092-159">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="94092-159">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="94092-160">Routing</span><span class="sxs-lookup"><span data-stu-id="94092-160">Routing</span></span>

<span data-ttu-id="94092-161">ASP.NET MVC di base viene compilato in cima [routing ASP.NET Core](../fundamentals/routing.md), un componente di mapping di URL potente che consente di compilare applicazioni con URL comprensibili e disponibile per la ricerca.</span><span class="sxs-lookup"><span data-stu-id="94092-161">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="94092-162">In questo modo è possibile definire URL criteri di denominazione dell'applicazione che funzionano correttamente per l'ottimizzazione del motore di ricerca (SEO) e per la generazione di collegamento, senza tener conto di come vengono organizzati i file sul server web.</span><span class="sxs-lookup"><span data-stu-id="94092-162">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="94092-163">È possibile definire i percorsi utilizzando una sintassi di modello di route pratico che supporta i vincoli di valore di route, i valori predefiniti e i valori facoltativi.</span><span class="sxs-lookup"><span data-stu-id="94092-163">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="94092-164">*Basato sulle convenzioni di routing* i formati di globale definiscono l'URL che consente l'applicazione accetta e ciascuna di queste modalità di formattazione viene eseguito il mapping a un metodo di azione specifica in specificato controller.</span><span class="sxs-lookup"><span data-stu-id="94092-164">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="94092-165">Quando viene ricevuta una richiesta in ingresso, il motore di routing analizza l'URL corrisponde a uno dei formati di URL definiti e quindi chiama il metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="94092-165">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="94092-166">*Attributo routing* consente di specificare le informazioni di routing dichiarando i controller e azioni con attributi che definiscono la route dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="94092-166">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="94092-167">Ciò significa che le definizioni route vengono posizionate accanto al controller e l'azione con cui si associato.</span><span class="sxs-lookup"><span data-stu-id="94092-167">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="94092-168">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="94092-168">Model binding</span></span>

<span data-ttu-id="94092-169">Componenti di base di ASP.NET MVC [associazione del modello](models/model-binding.md) converte i dati di richiesta del client (i valori del form, dati della route, i parametri di stringa di query, le intestazioni HTTP) in oggetti in grado di gestire il controller.</span><span class="sxs-lookup"><span data-stu-id="94092-169">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="94092-170">Di conseguenza, la logica del controller non è necessaria l'operazione di capire i dati della richiesta in ingresso; è semplicemente i dati come parametri per i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="94092-170">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="94092-171">Convalida modello</span><span class="sxs-lookup"><span data-stu-id="94092-171">Model validation</span></span>

<span data-ttu-id="94092-172">Componenti di base ASP.NET MVC supporta [convalida](models/validation.md) dichiarando l'oggetto modello con gli attributi di convalida di dati dell'annotazione.</span><span class="sxs-lookup"><span data-stu-id="94092-172">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="94092-173">Gli attributi di convalida vengono controllate sul lato client prima che i valori vengono inviati al server, nonché nel server prima di azione del controller viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="94092-173">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="94092-174">Un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="94092-174">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="94092-175">Il framework gestisce convalida dei dati richiesta sul client e nel server.</span><span class="sxs-lookup"><span data-stu-id="94092-175">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="94092-176">La logica di convalida specificata sui tipi di modello viene aggiunto alle viste sottoposto a rendering come annotazioni non intrusivi e viene applicata nei browser con [convalida jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="94092-176">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="94092-177">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="94092-177">Dependency injection</span></span>

<span data-ttu-id="94092-178">ASP.NET di base include il supporto incorporato per [inserimento di dipendenze (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="94092-178">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="94092-179">In ASP.NET MVC di base, [controller](controllers/dependency-injection.md) possibile richiesta necessari servizi tramite i relativi costruttori, consentendo loro di seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="94092-179">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="94092-180">Inoltre è possibile utilizzare l'app [inserimento di dipendenze di file nella visualizzazione](views/dependency-injection.md), usando il `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="94092-180">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="94092-181">Filtri</span><span class="sxs-lookup"><span data-stu-id="94092-181">Filters</span></span>

<span data-ttu-id="94092-182">[I filtri](controllers/filters.md) consentono agli sviluppatori di incapsulare i problemi di montaggio incrociato, ad esempio la gestione delle eccezioni o autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="94092-182">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="94092-183">Filtri di abilitare la logica personalizzata in esecuzione di pre e post-elaborazione per i metodi di azione e possono essere configurati per l'esecuzione in determinati punti all'interno della pipeline di esecuzione per una determinata richiesta.</span><span class="sxs-lookup"><span data-stu-id="94092-183">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="94092-184">Filtri possono essere applicati ai controller o azioni come attributi (o possono essere eseguiti a livello globale).</span><span class="sxs-lookup"><span data-stu-id="94092-184">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="94092-185">Alcuni filtri (ad esempio `Authorize`) sono inclusi in framework.</span><span class="sxs-lookup"><span data-stu-id="94092-185">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="94092-186">Aree</span><span class="sxs-lookup"><span data-stu-id="94092-186">Areas</span></span>

<span data-ttu-id="94092-187">[Aree](controllers/areas.md) consentono di suddividere un'app Web ASP.NET MVC di base di grandi dimensioni in raggruppamenti funzionali più piccoli.</span><span class="sxs-lookup"><span data-stu-id="94092-187">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="94092-188">Un'area è una struttura MVC all'interno di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="94092-188">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="94092-189">In un progetto MVC, vengono mantenuti i componenti logici come modello, Controller e visualizza in cartelle diverse e MVC utilizza le convenzioni di denominazione per creare la relazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="94092-189">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="94092-190">Per un'app di grandi dimensioni, può risultare utile per partizionare l'app in varie aree di livello elevate di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="94092-190">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="94092-191">Ad esempio, un'applicazione di e-commerce con più unità aziendali, ad esempio l'estrazione, fatturazione e la ricerca e così via. Ognuna di queste unità sono le proprie viste componenti logici, controller e i modelli.</span><span class="sxs-lookup"><span data-stu-id="94092-191">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="94092-192">API Web</span><span class="sxs-lookup"><span data-stu-id="94092-192">Web APIs</span></span>

<span data-ttu-id="94092-193">Oltre a essere una piattaforma ideale per la compilazione di siti web, ASP.NET MVC di base dispone di supporto ottimale per la compilazione di API Web.</span><span class="sxs-lookup"><span data-stu-id="94092-193">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="94092-194">È possibile compilare servizi raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="94092-194">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="94092-195">Il framework include il supporto per la negoziazione del contenuto HTTP con supporto incorporato per [la formattazione dei dati](models/formatting.md) come JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="94092-195">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="94092-196">Scrivere [formattatori personalizzati](advanced/custom-formatters.md) per aggiungere supporto per formati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="94092-196">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="94092-197">Utilizzare la generazione di collegamenti per abilitare il supporto per hypermedia.</span><span class="sxs-lookup"><span data-stu-id="94092-197">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="94092-198">Attivare con facilità il supporto per [risorse multiorigine (CORS) di condivisione](http://www.w3.org/TR/cors/) in modo che le API Web può essere condiviso tra più applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="94092-198">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="94092-199">Testabilità</span><span class="sxs-lookup"><span data-stu-id="94092-199">Testability</span></span>

<span data-ttu-id="94092-200">Utilizzo del framework di interfacce e inserimento di dipendenze renderlo particolarmente adatto agli unit test e il framework include funzionalità (ad esempio, un provider TestHost e InMemory per Entity Framework) che rendono [test di integrazione](../testing/integration-testing.md) rapido semplice e oltre.</span><span class="sxs-lookup"><span data-stu-id="94092-200">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="94092-201">Altre informazioni, vedere [logica del controller di test](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="94092-201">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="94092-202">Motore di visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="94092-202">Razor view engine</span></span>

<span data-ttu-id="94092-203">[ASP.NET MVC Core views](views/overview.md) utilizzare il [motore di visualizzazione Razor](views/razor.md) per il rendering di visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="94092-203">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="94092-204">Razor è un linguaggio di markup modello compact, espressivo e fluida per la definizione di viste usando codice c# incorporato.</span><span class="sxs-lookup"><span data-stu-id="94092-204">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="94092-205">Razor viene utilizzato per generare dinamicamente il contenuto web nel server.</span><span class="sxs-lookup"><span data-stu-id="94092-205">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="94092-206">Normalmente, è possibile combinare codice lato server con il codice e contenuto sul lato client.</span><span class="sxs-lookup"><span data-stu-id="94092-206">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="94092-207">Tramite il motore di visualizzazione Razor è possibile definire [layout](views/layout.md), [visualizzazioni parziali](views/partial.md) e sezioni sostituibili.</span><span class="sxs-lookup"><span data-stu-id="94092-207">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="94092-208">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="94092-208">Strongly typed views</span></span>

<span data-ttu-id="94092-209">Visualizzazioni Razor in MVC possono essere fortemente tipizzate in base al modello.</span><span class="sxs-lookup"><span data-stu-id="94092-209">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="94092-210">Controller è possono passare un modello fortemente tipizzato per abilitare le viste di controllo dei tipi e IntelliSense supporta le viste.</span><span class="sxs-lookup"><span data-stu-id="94092-210">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="94092-211">Ad esempio, la vista seguente esegue il rendering di un modello di tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="94092-211">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="94092-212">Helper di tag</span><span class="sxs-lookup"><span data-stu-id="94092-212">Tag Helpers</span></span>

<span data-ttu-id="94092-213">[Gli helper di tag](views/tag-helpers/intro.md) consentire al codice lato server partecipare alla creazione e il rendering di elementi HTML in file Razor.</span><span class="sxs-lookup"><span data-stu-id="94092-213">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="94092-214">È possibile utilizzare gli helper di tag per definire i tag personalizzati (ad esempio, `<environment>`) o per modificare il comportamento dei tag esistenti (ad esempio, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="94092-214">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="94092-215">Eseguire il binding gli helper di tag a elementi specifici in base al nome di elemento e i relativi attributi.</span><span class="sxs-lookup"><span data-stu-id="94092-215">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="94092-216">Offrono i vantaggi del rendering lato server mantenendo al tempo stesso un'esperienza di modifica HTML.</span><span class="sxs-lookup"><span data-stu-id="94092-216">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="94092-217">Vi sono molti gli helper di Tag predefiniti per le attività comuni, ad esempio la creazione di moduli, collegamenti, asset durante il caricamento e più - e ancora più disponibili nel repository GitHub pubblici e come NuGet pacchetti.</span><span class="sxs-lookup"><span data-stu-id="94092-217">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="94092-218">Vengono creati gli helper di tag in c# e di destinazione gli elementi HTML in base al nome di elemento, il nome dell'attributo o tag padre.</span><span class="sxs-lookup"><span data-stu-id="94092-218">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="94092-219">Ad esempio, l'oggetto incorporato LinkTagHelper può essere utilizzato per creare un collegamento al `Login` azione del `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="94092-219">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="94092-220">Il `EnvironmentTagHelper` può essere utilizzato per includere script differenti nelle visualizzazioni (ad esempio, non elaborato o minimizzata) in base all'ambiente di runtime, ad esempio sviluppo, gestione temporanea o produzione:</span><span class="sxs-lookup"><span data-stu-id="94092-220">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="94092-221">Gli helper di tag forniscono un'esperienza di sviluppo HTML descrittivo e un ambiente completo di IntelliSense per la creazione di markup HTML e Razor.</span><span class="sxs-lookup"><span data-stu-id="94092-221">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="94092-222">La maggior parte degli helper Tag predefiniti come destinazione gli elementi HTML esistenti e fornire attributi sul lato server per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="94092-222">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="94092-223">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="94092-223">View Components</span></span>

<span data-ttu-id="94092-224">[Visualizzare i componenti](views/view-components.md) consentono di creare un pacchetto la logica di rendering e riutilizzarla in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="94092-224">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="94092-225">Ma sono simili a [visualizzazioni parziali](views/partial.md), ma con logica associata.</span><span class="sxs-lookup"><span data-stu-id="94092-225">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
