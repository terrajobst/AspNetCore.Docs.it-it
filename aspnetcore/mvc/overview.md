---
title: Panoramica di ASP.NET MVC
author: ardalis
description: "Informazioni su come Core di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API."
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: ad8a1dfae89a7ecd5573c16ba70d7d12216b4c57
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="9fe97-103">Panoramica di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9fe97-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="9fe97-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9fe97-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9fe97-105">Componenti di base di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API.</span><span class="sxs-lookup"><span data-stu-id="9fe97-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="9fe97-106">Che cos'è il modello MVC?</span><span class="sxs-lookup"><span data-stu-id="9fe97-106">What is the MVC pattern?</span></span>

<span data-ttu-id="9fe97-107">Il modello architetturale Model-View-Controller (MVC) suddivide un'applicazione in tre gruppi principali di componenti: modelli, visualizzazioni e controller.</span><span class="sxs-lookup"><span data-stu-id="9fe97-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="9fe97-108">Questo modello consente di ottenere [separazione delle problematiche](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="9fe97-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="9fe97-109">Usando questo modello, le richieste degli utenti vengono indirizzate a un Controller che è responsabile per l'utilizzo del modello per eseguire le azioni dell'utente e/o recuperare i risultati delle query.</span><span class="sxs-lookup"><span data-stu-id="9fe97-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="9fe97-110">Il Controller sceglie visualizzazione da mostrare all'utente e fornisce i dati del modello che richiede.</span><span class="sxs-lookup"><span data-stu-id="9fe97-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="9fe97-111">Il diagramma seguente illustra i tre componenti principali e quelle che fanno riferimento gli altri:</span><span class="sxs-lookup"><span data-stu-id="9fe97-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modello MVC](overview/_static/mvc.png)

<span data-ttu-id="9fe97-113">Questo limite delle responsabilità consente di scalare l'applicazione in termini di complessità, perché è più facile da codice, eseguire il debug e testare un elemento (modello, visualizzazione o controller) con un singolo processo (e segue il [singolo principio di responsabilità ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="9fe97-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="9fe97-114">È più difficile da aggiornare, test e il debug del codice con dipendenze distribuiti in due o più di queste tre aree.</span><span class="sxs-lookup"><span data-stu-id="9fe97-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="9fe97-115">Ad esempio, logica dell'interfaccia utente tende a cambiano più frequentemente logica di business.</span><span class="sxs-lookup"><span data-stu-id="9fe97-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="9fe97-116">Se la logica di presentazione di codice e di business viene combinata in un singolo oggetto, è necessario modificare un oggetto che contiene la logica di business, ogni volta che si modifica l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="9fe97-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="9fe97-117">Questo problema può causare errori e richiede la riesecuzione del test di tutte le regole di business dopo ogni interfaccia utente minima modificato.</span><span class="sxs-lookup"><span data-stu-id="9fe97-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe97-118">Sia la visualizzazione e il controller dipendono dal modello.</span><span class="sxs-lookup"><span data-stu-id="9fe97-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="9fe97-119">Tuttavia, il modello dipende la vista né il controller.</span><span class="sxs-lookup"><span data-stu-id="9fe97-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="9fe97-120">Questo è uno dei principali vantaggi di separazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="9fe97-121">Questa separazione consente la presentazione visiva indipendente da compilare e testare il modello.</span><span class="sxs-lookup"><span data-stu-id="9fe97-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="9fe97-122">Responsabilità di modello</span><span class="sxs-lookup"><span data-stu-id="9fe97-122">Model Responsibilities</span></span>

<span data-ttu-id="9fe97-123">Il modello in un'applicazione MVC rappresenta lo stato dell'applicazione e qualsiasi logica di business o di operazioni che devono essere eseguite da esso.</span><span class="sxs-lookup"><span data-stu-id="9fe97-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="9fe97-124">Logica di business deve essere incapsulata nel modello, insieme a qualsiasi logica di implementazione per la persistenza dello stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="9fe97-125">Visualizzazioni fortemente tipizzate utilizzano in genere i tipi di ViewModel progettati per contenere i dati da visualizzare per tale vista.</span><span class="sxs-lookup"><span data-stu-id="9fe97-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="9fe97-126">Il controller crea e popola le istanze di ViewModel dal modello.</span><span class="sxs-lookup"><span data-stu-id="9fe97-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe97-127">Esistono diversi modi per organizzare il modello in un'applicazione che utilizza il modello architetturale MVC.</span><span class="sxs-lookup"><span data-stu-id="9fe97-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="9fe97-128">Altre informazioni su alcune [diversi tipi di modello](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="9fe97-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="9fe97-129">Visualizza responsabilità</span><span class="sxs-lookup"><span data-stu-id="9fe97-129">View Responsibilities</span></span>

<span data-ttu-id="9fe97-130">Le visualizzazioni sono responsabili per la presentazione di contenuto tramite l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="9fe97-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="9fe97-131">Usano il [motore di visualizzazione Razor](#razor-view-engine) per incorporare il codice .NET nel markup HTML.</span><span class="sxs-lookup"><span data-stu-id="9fe97-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="9fe97-132">Dovrebbe esserci minimo logica all'interno di viste e qualsiasi logica in essi contenuti devono essere correlati alla presentazione di contenuto.</span><span class="sxs-lookup"><span data-stu-id="9fe97-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="9fe97-133">Se si rileva la necessità di eseguire una notevole della logica di visualizzazione di file per visualizzare i dati da un modello complesso, è consigliabile utilizzare un [del componente di visualizzazione](views/view-components.md), ViewModel, o del modello di visualizzazione per semplificare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="9fe97-134">Responsabilità controller</span><span class="sxs-lookup"><span data-stu-id="9fe97-134">Controller Responsibilities</span></span>

<span data-ttu-id="9fe97-135">I controller sono i componenti che gestiscono l'interazione dell'utente, utilizzano il modello e selezionano infine una visualizzazione per il rendering.</span><span class="sxs-lookup"><span data-stu-id="9fe97-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="9fe97-136">In un'applicazione MVC, Visualizza solo informazioni; il controller gestisce e risponde all'input dell'utente e l'interazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="9fe97-137">Nel modello MVC, è il punto di ingresso iniziale, il controller ed è responsabile per selezionare il modello di tipi da utilizzare e la visualizzazione per il rendering (pertanto il relativo nome - controlla la modalità dell'app risponde a una determinata richiesta).</span><span class="sxs-lookup"><span data-stu-id="9fe97-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="9fe97-138">Controller dovrebbero non essere eccessivamente complessa da un numero eccessivo di responsabilità.</span><span class="sxs-lookup"><span data-stu-id="9fe97-138">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="9fe97-139">Per mantenere la logica di controller di diventare eccessivamente complessa, utilizzare il [singolo responsabilità principio](http://deviq.com/single-responsibility-principle/) alla logica di business di push fuori il controller e nel modello di dominio.</span><span class="sxs-lookup"><span data-stu-id="9fe97-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="9fe97-140">Se si ritiene che le azioni del controller eseguono frequentemente gli stessi tipi di azioni, è possibile seguire il [non ripetere manualmente principio](http://deviq.com/don-t-repeat-yourself/) spostando queste azioni comuni in [filtri](#filters).</span><span class="sxs-lookup"><span data-stu-id="9fe97-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="9fe97-141">Che cos'è ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="9fe97-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="9fe97-142">Il framework ASP.NET MVC di base è un semplice open source, il framework di presentazione testabili altamente ottimizzato per l'utilizzo con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9fe97-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="9fe97-143">Componenti di base di ASP.NET MVC fornisce basate su modelli consentono di creare siti Web dinamici che consente una netta separazione delle problematiche.</span><span class="sxs-lookup"><span data-stu-id="9fe97-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="9fe97-144">Fornisce un controllo completo sul markup, supporta basto e utilizza gli standard web più recenti.</span><span class="sxs-lookup"><span data-stu-id="9fe97-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="9fe97-145">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="9fe97-145">Features</span></span>

<span data-ttu-id="9fe97-146">Componenti di base di ASP.NET MVC include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="9fe97-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="9fe97-147">Routing</span><span class="sxs-lookup"><span data-stu-id="9fe97-147">Routing</span></span>](#routing)
* [<span data-ttu-id="9fe97-148">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9fe97-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="9fe97-149">Convalida modello</span><span class="sxs-lookup"><span data-stu-id="9fe97-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="9fe97-150">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="9fe97-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="9fe97-151">Filtri</span><span class="sxs-lookup"><span data-stu-id="9fe97-151">Filters</span></span>](#filters)
* [<span data-ttu-id="9fe97-152">Aree</span><span class="sxs-lookup"><span data-stu-id="9fe97-152">Areas</span></span>](#areas)
* [<span data-ttu-id="9fe97-153">API Web</span><span class="sxs-lookup"><span data-stu-id="9fe97-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="9fe97-154">Testabilità</span><span class="sxs-lookup"><span data-stu-id="9fe97-154">Testability</span></span>](#testability)
* [<span data-ttu-id="9fe97-155">Motore di visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="9fe97-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="9fe97-156">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="9fe97-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="9fe97-157">Helper tag</span><span class="sxs-lookup"><span data-stu-id="9fe97-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="9fe97-158">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="9fe97-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="9fe97-159">Routing</span><span class="sxs-lookup"><span data-stu-id="9fe97-159">Routing</span></span>

<span data-ttu-id="9fe97-160">ASP.NET MVC di base viene compilato in cima [routing ASP.NET Core](../fundamentals/routing.md), un componente di mapping di URL potente che consente di compilare applicazioni con URL comprensibili e disponibile per la ricerca.</span><span class="sxs-lookup"><span data-stu-id="9fe97-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="9fe97-161">In questo modo è possibile definire URL criteri di denominazione dell'applicazione che funzionano correttamente per l'ottimizzazione del motore di ricerca (SEO) e per la generazione di collegamento, senza tener conto di come vengono organizzati i file sul server web.</span><span class="sxs-lookup"><span data-stu-id="9fe97-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="9fe97-162">È possibile definire i percorsi utilizzando una sintassi di modello di route pratico che supporta i vincoli di valore di route, i valori predefiniti e i valori facoltativi.</span><span class="sxs-lookup"><span data-stu-id="9fe97-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="9fe97-163">*Basato sulle convenzioni di routing* i formati di globale definiscono l'URL che consente l'applicazione accetta e ciascuna di queste modalità di formattazione viene eseguito il mapping a un metodo di azione specifica in specificato controller.</span><span class="sxs-lookup"><span data-stu-id="9fe97-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="9fe97-164">Quando viene ricevuta una richiesta in ingresso, il motore di routing analizza l'URL corrisponde a uno dei formati di URL definiti e quindi chiama il metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="9fe97-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="9fe97-165">*Attributo routing* consente di specificare le informazioni di routing dichiarando i controller e azioni con attributi che definiscono la route dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="9fe97-166">Ciò significa che le definizioni route vengono posizionate accanto al controller e l'azione con cui si associato.</span><span class="sxs-lookup"><span data-stu-id="9fe97-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="9fe97-167">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9fe97-167">Model binding</span></span>

<span data-ttu-id="9fe97-168">Componenti di base di ASP.NET MVC [associazione del modello](models/model-binding.md) converte i dati di richiesta del client (i valori del form, dati della route, i parametri di stringa di query, le intestazioni HTTP) in oggetti in grado di gestire il controller.</span><span class="sxs-lookup"><span data-stu-id="9fe97-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="9fe97-169">Di conseguenza, la logica del controller non è necessaria l'operazione di capire i dati della richiesta in ingresso; è semplicemente i dati come parametri per i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="9fe97-170">Convalida modello</span><span class="sxs-lookup"><span data-stu-id="9fe97-170">Model validation</span></span>

<span data-ttu-id="9fe97-171">Componenti di base ASP.NET MVC supporta [convalida](models/validation.md) dichiarando l'oggetto modello con gli attributi di convalida di dati dell'annotazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="9fe97-172">Gli attributi di convalida vengono controllate sul lato client prima che i valori vengono inviati al server, nonché nel server prima di azione del controller viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="9fe97-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="9fe97-173">Un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="9fe97-173">A controller action:</span></span>

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

<span data-ttu-id="9fe97-174">Il framework gestisce convalida dei dati richiesta sul client e nel server.</span><span class="sxs-lookup"><span data-stu-id="9fe97-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="9fe97-175">La logica di convalida specificata sui tipi di modello viene aggiunto alle viste sottoposto a rendering come annotazioni non intrusivi e viene applicata nei browser con [convalida jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="9fe97-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="9fe97-176">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="9fe97-176">Dependency injection</span></span>

<span data-ttu-id="9fe97-177">ASP.NET di base include il supporto incorporato per [inserimento di dipendenze (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="9fe97-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="9fe97-178">In ASP.NET MVC di base, [controller](controllers/dependency-injection.md) possibile richiesta necessari servizi tramite i relativi costruttori, consentendo loro di seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="9fe97-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="9fe97-179">Inoltre è possibile utilizzare l'app [inserimento di dipendenze di file nella visualizzazione](views/dependency-injection.md), usando il `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="9fe97-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="9fe97-180">Filtri</span><span class="sxs-lookup"><span data-stu-id="9fe97-180">Filters</span></span>

<span data-ttu-id="9fe97-181">[I filtri](controllers/filters.md) consentono agli sviluppatori di incapsulare i problemi di montaggio incrociato, ad esempio la gestione delle eccezioni o autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="9fe97-182">Filtri di abilitare la logica personalizzata in esecuzione di pre e post-elaborazione per i metodi di azione e possono essere configurati per l'esecuzione in determinati punti all'interno della pipeline di esecuzione per una determinata richiesta.</span><span class="sxs-lookup"><span data-stu-id="9fe97-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="9fe97-183">Filtri possono essere applicati ai controller o azioni come attributi (o possono essere eseguiti a livello globale).</span><span class="sxs-lookup"><span data-stu-id="9fe97-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="9fe97-184">Alcuni filtri (ad esempio `Authorize`) sono inclusi in framework.</span><span class="sxs-lookup"><span data-stu-id="9fe97-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="9fe97-185">Aree</span><span class="sxs-lookup"><span data-stu-id="9fe97-185">Areas</span></span>

<span data-ttu-id="9fe97-186">[Aree](controllers/areas.md) consentono di suddividere un'app Web ASP.NET MVC di base di grandi dimensioni in raggruppamenti funzionali più piccoli.</span><span class="sxs-lookup"><span data-stu-id="9fe97-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="9fe97-187">Un'area è una struttura MVC all'interno di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="9fe97-188">In un progetto MVC, vengono mantenuti i componenti logici come modello, Controller e visualizza in cartelle diverse e MVC utilizza le convenzioni di denominazione per creare la relazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="9fe97-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="9fe97-189">Per un'app di grandi dimensioni, può risultare utile per partizionare l'app in varie aree di livello elevate di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="9fe97-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="9fe97-190">Ad esempio, un'applicazione di e-commerce con più unità aziendali, ad esempio l'estrazione, fatturazione e la ricerca e così via. Ognuna di queste unità sono le proprie viste componenti logici, controller e i modelli.</span><span class="sxs-lookup"><span data-stu-id="9fe97-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="9fe97-191">API Web</span><span class="sxs-lookup"><span data-stu-id="9fe97-191">Web APIs</span></span>

<span data-ttu-id="9fe97-192">Oltre a essere una piattaforma ideale per la compilazione di siti web, ASP.NET MVC di base dispone di supporto ottimale per la compilazione di API Web.</span><span class="sxs-lookup"><span data-stu-id="9fe97-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="9fe97-193">È possibile compilare servizi raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="9fe97-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="9fe97-194">Il framework include il supporto per la negoziazione del contenuto HTTP con supporto incorporato per [la formattazione dei dati](models/formatting.md) come JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="9fe97-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="9fe97-195">Scrivere [formattatori personalizzati](advanced/custom-formatters.md) per aggiungere supporto per formati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="9fe97-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="9fe97-196">Utilizzare la generazione di collegamenti per abilitare il supporto per hypermedia.</span><span class="sxs-lookup"><span data-stu-id="9fe97-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="9fe97-197">Attivare con facilità il supporto per [risorse multiorigine (CORS) di condivisione](http://www.w3.org/TR/cors/) in modo che le API Web può essere condiviso tra più applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="9fe97-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="9fe97-198">Testabilità</span><span class="sxs-lookup"><span data-stu-id="9fe97-198">Testability</span></span>

<span data-ttu-id="9fe97-199">Utilizzo del framework di interfacce e inserimento di dipendenze renderlo particolarmente adatto agli unit test e il framework include funzionalità (ad esempio, un provider TestHost e InMemory per Entity Framework) che rendono [test di integrazione](../testing/integration-testing.md) rapido semplice e oltre.</span><span class="sxs-lookup"><span data-stu-id="9fe97-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="9fe97-200">Altre informazioni, vedere [logica del controller di test](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="9fe97-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="9fe97-201">Motore di visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="9fe97-201">Razor view engine</span></span>

<span data-ttu-id="9fe97-202">[ASP.NET MVC Core views](views/overview.md) utilizzare il [motore di visualizzazione Razor](views/razor.md) per il rendering di visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="9fe97-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="9fe97-203">Razor è un linguaggio di markup modello compact, espressivo e fluida per la definizione di viste usando codice c# incorporato.</span><span class="sxs-lookup"><span data-stu-id="9fe97-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="9fe97-204">Razor viene utilizzato per generare dinamicamente il contenuto web nel server.</span><span class="sxs-lookup"><span data-stu-id="9fe97-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="9fe97-205">Normalmente, è possibile combinare codice lato server con il codice e contenuto sul lato client.</span><span class="sxs-lookup"><span data-stu-id="9fe97-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="9fe97-206">Tramite il motore di visualizzazione Razor è possibile definire [layout](views/layout.md), [visualizzazioni parziali](views/partial.md) e sezioni sostituibili.</span><span class="sxs-lookup"><span data-stu-id="9fe97-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="9fe97-207">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="9fe97-207">Strongly typed views</span></span>

<span data-ttu-id="9fe97-208">Visualizzazioni Razor in MVC possono essere fortemente tipizzate in base al modello.</span><span class="sxs-lookup"><span data-stu-id="9fe97-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="9fe97-209">Controller è possono passare un modello fortemente tipizzato per abilitare le viste di controllo dei tipi e IntelliSense supporta le viste.</span><span class="sxs-lookup"><span data-stu-id="9fe97-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="9fe97-210">Ad esempio, la vista seguente esegue il rendering di un modello di tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="9fe97-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="9fe97-211">Helper di tag</span><span class="sxs-lookup"><span data-stu-id="9fe97-211">Tag Helpers</span></span>

<span data-ttu-id="9fe97-212">[Gli helper di tag](views/tag-helpers/intro.md) consentire al codice lato server partecipare alla creazione e il rendering di elementi HTML in file Razor.</span><span class="sxs-lookup"><span data-stu-id="9fe97-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="9fe97-213">È possibile utilizzare gli helper di tag per definire i tag personalizzati (ad esempio, `<environment>`) o per modificare il comportamento dei tag esistenti (ad esempio, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="9fe97-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="9fe97-214">Eseguire il binding gli helper di tag a elementi specifici in base al nome di elemento e i relativi attributi.</span><span class="sxs-lookup"><span data-stu-id="9fe97-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="9fe97-215">Offrono i vantaggi del rendering lato server mantenendo al tempo stesso un'esperienza di modifica HTML.</span><span class="sxs-lookup"><span data-stu-id="9fe97-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="9fe97-216">Vi sono molti gli helper di Tag predefiniti per le attività comuni, ad esempio la creazione di moduli, collegamenti, asset durante il caricamento e più - e ancora più disponibili nel repository GitHub pubblici e come NuGet pacchetti.</span><span class="sxs-lookup"><span data-stu-id="9fe97-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="9fe97-217">Vengono creati gli helper di tag in c# e di destinazione gli elementi HTML in base al nome di elemento, il nome dell'attributo o tag padre.</span><span class="sxs-lookup"><span data-stu-id="9fe97-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="9fe97-218">Ad esempio, l'oggetto incorporato LinkTagHelper può essere utilizzato per creare un collegamento al `Login` azione del `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="9fe97-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="9fe97-219">Il `EnvironmentTagHelper` può essere utilizzato per includere script differenti nelle visualizzazioni (ad esempio, non elaborato o minimizzata) in base all'ambiente di runtime, ad esempio sviluppo, gestione temporanea o produzione:</span><span class="sxs-lookup"><span data-stu-id="9fe97-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="9fe97-220">Gli helper di tag forniscono un'esperienza di sviluppo HTML descrittivo e un ambiente completo di IntelliSense per la creazione di markup HTML e Razor.</span><span class="sxs-lookup"><span data-stu-id="9fe97-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="9fe97-221">La maggior parte degli helper Tag predefiniti come destinazione gli elementi HTML esistenti e fornire attributi sul lato server per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="9fe97-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="9fe97-222">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="9fe97-222">View Components</span></span>

<span data-ttu-id="9fe97-223">[Visualizzare i componenti](views/view-components.md) consentono di creare un pacchetto la logica di rendering e riutilizzarla in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9fe97-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="9fe97-224">Ma sono simili a [visualizzazioni parziali](views/partial.md), ma con logica associata.</span><span class="sxs-lookup"><span data-stu-id="9fe97-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
