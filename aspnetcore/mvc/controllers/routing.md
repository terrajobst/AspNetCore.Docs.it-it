---
title: Routing di azioni del Controller
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: cc3277400aee956f47c53e5a4f3d4e84d3a3d1a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="73c5c-103">Routing di azioni del Controller</span><span class="sxs-lookup"><span data-stu-id="73c5c-103">Routing to Controller Actions</span></span>

<span data-ttu-id="73c5c-104">Da [Ryan Nowak](https://github.com/rynowak) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73c5c-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="73c5c-105">Componenti di base di ASP.NET MVC utilizza il Routing [middleware](../../fundamentals/middleware.md) per associare gli URL delle richieste in ingresso e di eseguirne il mapping alle azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-105">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="73c5c-106">Le route sono definite nel codice di avvio o attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="73c5c-107">Le route viene descritto come i percorsi URL dovrebbero corrispondere alle azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="73c5c-108">Le route consentono inoltre di generare gli URL (per i collegamenti) inviati nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="73c5c-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="73c5c-109">Azioni di vengono tradizionalmente indirizzate o attributo indirizzato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="73c5c-110">Inserimento di una route del controller o l'azione rende attributo indirizzato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="73c5c-111">Vedere [mista routing](#routing-mixed-ref-label) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="73c5c-112">Questo documento illustrerà le interazioni tra MVC e routing e come tipico assicurarsi di applicazioni MVC Usa le funzionalità di routine.</span><span class="sxs-lookup"><span data-stu-id="73c5c-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="73c5c-113">Vedere [Routing](xref:fundamentals/routing) per ulteriori informazioni sul routing avanzata.</span><span class="sxs-lookup"><span data-stu-id="73c5c-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="73c5c-114">Configurazione di Routing Middleware</span><span class="sxs-lookup"><span data-stu-id="73c5c-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="73c5c-115">Nel *configura* metodo potrebbe apparire simile al codice:</span><span class="sxs-lookup"><span data-stu-id="73c5c-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="73c5c-116">Nella chiamata a `UseMvc`, `MapRoute` viene utilizzato per creare una singola route, si farà riferimento a come il `default` route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="73c5c-117">La maggior parte delle applicazioni MVC utilizzerà una route con un modello simile al `default` route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="73c5c-118">Il modello di route `"{controller=Home}/{action=Index}/{id?}"` può corrispondere a un percorso URL come `/Products/Details/5` che consentirà di estrarre i valori della route `{ controller = Products, action = Details, id = 5 }` per il percorso di suddivisione in token.</span><span class="sxs-lookup"><span data-stu-id="73c5c-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="73c5c-119">MVC tenterà di individuare un controller denominato `ProductsController` ed eseguire l'azione `Details`:</span><span class="sxs-lookup"><span data-stu-id="73c5c-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="73c5c-120">Si noti che in questo esempio, l'associazione di modelli utilizzerà il valore di `id = 5` per impostare il `id` parametro `5` quando si richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="73c5c-121">Vedere il [associazione di modelli](../models/model-binding.md) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="73c5c-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="73c5c-122">Utilizzo di `default` route:</span><span class="sxs-lookup"><span data-stu-id="73c5c-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="73c5c-123">Il modello di route:</span><span class="sxs-lookup"><span data-stu-id="73c5c-123">The route template:</span></span>

* <span data-ttu-id="73c5c-124">`{controller=Home}`definisce `Home` come impostazione predefinita`controller`</span><span class="sxs-lookup"><span data-stu-id="73c5c-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="73c5c-125">`{action=Index}`definisce `Index` come impostazione predefinita`action`</span><span class="sxs-lookup"><span data-stu-id="73c5c-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="73c5c-126">`{id?}`definisce `id` come facoltativi</span><span class="sxs-lookup"><span data-stu-id="73c5c-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="73c5c-127">Impostazione predefinita e i parametri di route facoltativo non è necessario trovarsi nel percorso URL per trovare una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="73c5c-127">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="73c5c-128">Vedere [riferimento del modello di Route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi di modello di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="73c5c-129">`"{controller=Home}/{action=Index}/{id?}"`può corrispondere al percorso di URL `/` e restituirà i valori della route `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="73c5c-130">I valori per `controller` e `action` rendere utilizzare i valori predefiniti, `id` non produce un valore perché è presente alcun segmento corrispondente nel percorso URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-130">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="73c5c-131">MVC utilizzare questi valori di route per selezionare il `HomeController` e `Index` azione:</span><span class="sxs-lookup"><span data-stu-id="73c5c-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="73c5c-132">Usare questa definizione di controller e il modello di route, il `HomeController.Index` azione verrà eseguita per uno o più percorsi URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="73c5c-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="73c5c-133">Il metodo praticità `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="73c5c-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="73c5c-134">Può essere utilizzato per sostituire:</span><span class="sxs-lookup"><span data-stu-id="73c5c-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="73c5c-135">`UseMvc`e `UseMvcWithDefaultRoute` aggiungere un'istanza di `RouterMiddleware` la pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="73c5c-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="73c5c-136">MVC non interagisce direttamente con middleware e utilizza il routing per gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="73c5c-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="73c5c-137">MVC è connesso a tutte le route tramite un'istanza di `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="73c5c-138">Il codice all'interno di `UseMvc` è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="73c5c-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="73c5c-139">`UseMvc`non definire direttamente le route, viene aggiunto un segnaposto per la raccolta di route per il `attribute` route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-139">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="73c5c-140">L'overload `UseMvc(Action<IRouteBuilder>)` consente di aggiungere route personalizzati e supporta anche l'attributo di routing.</span><span class="sxs-lookup"><span data-stu-id="73c5c-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="73c5c-141">`UseMvc`e le sue varianti viene aggiunto un segnaposto per la route di attributo: routing degli attributi è sempre disponibile indipendentemente dalla modalità di configurazione `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="73c5c-142">`UseMvcWithDefaultRoute`definisce una route predefinita e supporta il routing di attributo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="73c5c-143">Il [attributo Routing](#attribute-routing-ref-label) sono incluse ulteriori informazioni sul routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="73c5c-144">Il routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="73c5c-144">Conventional routing</span></span>

<span data-ttu-id="73c5c-145">Il `default` route:</span><span class="sxs-lookup"><span data-stu-id="73c5c-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="73c5c-146">è un esempio di un *routing convenzionale*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="73c5c-147">Viene chiamato questo stile *routing convenzionale* poiché stabilisce un *convenzione* per i percorsi URL:</span><span class="sxs-lookup"><span data-stu-id="73c5c-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="73c5c-148">il primo segmento del percorso viene eseguito il mapping al nome del controller</span><span class="sxs-lookup"><span data-stu-id="73c5c-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="73c5c-149">il secondo esegue il mapping per il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-149">the second maps to the action name.</span></span>

* <span data-ttu-id="73c5c-150">il terzo segmento viene utilizzato per un parametro facoltativo `id` utilizzato per eseguire il mapping a un'entità del modello</span><span class="sxs-lookup"><span data-stu-id="73c5c-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="73c5c-151">Usando questa `default` route, il percorso URL `/Products/List` esegue il mapping al `ProductsController.List` action e `/Blog/Article/17` esegue il mapping a `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="73c5c-152">Questo mapping si basa sui nomi di azione e del controller **solo** e non si basa su spazi dei nomi, i percorsi dei file di origine o i parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-152">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="73c5c-153">Utilizzo convenzionale di routing con la route predefinita consente di compilare rapidamente l'applicazione senza che sia necessario ideare un nuovo modello di URL per ogni azione che si definisce.</span><span class="sxs-lookup"><span data-stu-id="73c5c-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="73c5c-154">Per un'applicazione con azioni di stile CRUD, con verifica coerenza per gli URL tra i controller consente di semplificare il codice e rendere l'interfaccia utente più prevedibile.</span><span class="sxs-lookup"><span data-stu-id="73c5c-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="73c5c-155">Il `id` definiti come facoltativi per il modello di route, vale a dire che le azioni possono essere eseguiti senza l'ID fornito come parte dell'URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="73c5c-156">In genere cosa succede se `id` viene omesso dall'URL che verrà impostato su `0` dall'associazione del modello e di conseguenza verrà rilevata alcuna entità nei database di ricerca `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="73c5c-157">Routing degli attributi consente di visualizzare un controllo accurato per rendere l'ID obbligatorio per alcune azioni e non per altri utenti.</span><span class="sxs-lookup"><span data-stu-id="73c5c-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="73c5c-158">Per convenzione includerà la documentazione di parametri facoltativi come `id` quando vengono maggiore probabilità di comparire in uso corretto.</span><span class="sxs-lookup"><span data-stu-id="73c5c-158">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="73c5c-159">Più route</span><span class="sxs-lookup"><span data-stu-id="73c5c-159">Multiple routes</span></span>

<span data-ttu-id="73c5c-160">È possibile aggiungere più route all'interno di `UseMvc` aggiungendo ulteriori chiamate a `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="73c5c-161">In questo modo consente di definire più convenzioni, o aggiungere le route convenzionale dedicati a un'azione specifica, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73c5c-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="73c5c-162">Il `blog` route qui è un *dedicato route convenzionale*, vale a dire che utilizza il sistema di routing tradizionale, ma è dedicato a un'azione specifica.</span><span class="sxs-lookup"><span data-stu-id="73c5c-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="73c5c-163">Poiché `controller` e `action` non vengono visualizzati nel modello di route come parametri, possono avere solo i valori predefiniti e pertanto questa route verrà sempre eseguito il mapping all'azione `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="73c5c-164">Le route nella raccolta di route vengono ordinate e verranno elaborate nell'ordine in che cui vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="73c5c-164">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="73c5c-165">In tal caso, in questo esempio, il `blog` route verrà tentate prima il `default` route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-166">*Dedicato route convenzionale* utilizzano spesso parametri di route catch-all come `{*article}` per acquisire la parte rimanente del percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="73c5c-167">In questo modo una route 'troppo greedy' vale a dire che corrisponda agli URL che si deve essere accompagnato da altre route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="73c5c-168">Inserire le route "greedy" più avanti in questa tabella di route per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="73c5c-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="73c5c-169">Fallback</span><span class="sxs-lookup"><span data-stu-id="73c5c-169">Fallback</span></span>

<span data-ttu-id="73c5c-170">Come parte dell'elaborazione della richiesta, MVC verificherà i valori della route possono essere utilizzati per trovare un controller e l'azione nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="73c5c-171">Se i valori di route non corrispondano a un'azione quindi la route non viene considerata una corrispondenza e la successiva route verrà tentata.</span><span class="sxs-lookup"><span data-stu-id="73c5c-171">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="73c5c-172">Si tratta di *fallback*, e è progettato per semplificare i casi in cui si sovrappongono route convenzionale.</span><span class="sxs-lookup"><span data-stu-id="73c5c-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="73c5c-173">Per evitare ambiguità tra azioni</span><span class="sxs-lookup"><span data-stu-id="73c5c-173">Disambiguating actions</span></span>

<span data-ttu-id="73c5c-174">Quando due azioni corrispondono dal routing, è necessario evitare ambiguità MVC per scegliere il candidato 'migliore' o in caso contrario, genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="73c5c-175">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73c5c-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="73c5c-176">Il controller definisce due azioni corrispondenti il percorso URL `/Products/Edit/17` e l'invio di dati `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="73c5c-177">Si tratta di un modello tipico per i controller MVC in `Edit(int)` viene illustrato un form per modificare un prodotto, e `Edit(int, Product)` elabora il modulo registrato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="73c5c-178">A tale scopo sarà necessario scegliere MVC `Edit(int, Product)` se la richiesta è HTTP `POST` e `Edit(int)` quando il verbo HTTP è diversa.</span><span class="sxs-lookup"><span data-stu-id="73c5c-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="73c5c-179">Il `HttpPostAttribute` ( `[HttpPost]` ) è un'implementazione di `IActionConstraint` che consentirà solo l'azione che deve essere selezionato quando il verbo HTTP è `POST`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="73c5c-180">La presenza di un `IActionConstraint` rende il `Edit(int, Product)` 'meglio' corrisponde a `Edit(int)`, pertanto `Edit(int, Product)` verranno provate prima.</span><span class="sxs-lookup"><span data-stu-id="73c5c-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="73c5c-181">È necessario scrivere personalizzato `IActionConstraint` implementazioni specializzati scenari, ma di importanti per comprendere il ruolo di attributi quali `HttpPostAttribute` -definiti attributi simili per altri verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="73c5c-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="73c5c-182">Nel routing convenzionale è comune per le azioni da utilizzare lo stesso nome di azione quando sono parte di un `show form -> submit form` flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="73c5c-182">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="73c5c-183">Il vantaggio di questo modello verrà diventano più evidente dopo aver esaminato il [IActionConstraint comprensione](#understanding-iactionconstraint) sezione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="73c5c-184">Se più route corrispondano e MVC non è possibile trovare una route 'le', verrà generata una `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="73c5c-185">Nomi di route</span><span class="sxs-lookup"><span data-stu-id="73c5c-185">Route names</span></span>

<span data-ttu-id="73c5c-186">Le stringhe `"blog"` e `"default"` negli esempi seguenti sono i nomi di route:</span><span class="sxs-lookup"><span data-stu-id="73c5c-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="73c5c-187">I nomi delle route dare la route un nome logico, in modo che la route denominata può essere utilizzata per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="73c5c-188">Questo semplifica notevolmente la creazione di URL durante l'ordinamento delle route può rendere complessa la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="73c5c-189">I nomi di route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-189">Routes names must be unique application-wide.</span></span>

<span data-ttu-id="73c5c-190">I nomi di route non avere un impatto sull'URL corrispondente o la gestione di richieste. vengono utilizzati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-190">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="73c5c-191">[Routing](xref:fundamentals/routing) maggiori informazioni sulla generazione di URL incluso generazione URL helper MVC specifico.</span><span class="sxs-lookup"><span data-stu-id="73c5c-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="73c5c-192">Routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="73c5c-192">Attribute routing</span></span>

<span data-ttu-id="73c5c-193">Attributo routing utilizza un set di attributi per eseguire il mapping di azioni direttamente ai modelli di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="73c5c-194">Nell'esempio seguente, `app.UseMvc();` viene utilizzata la `Configure` (metodo) e nessuna route viene passata.</span><span class="sxs-lookup"><span data-stu-id="73c5c-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="73c5c-195">Il `HomeController` corrisponderà a un set di URL simile al quale la route predefinita `{controller=Home}/{action=Index}/{id?}` corrisponderebbe:</span><span class="sxs-lookup"><span data-stu-id="73c5c-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="73c5c-196">Il `HomeController.Index()` azione verrà eseguita per tutti i percorsi URL `/`, `/Home`, o `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-197">In questo esempio evidenzia una chiave programmazione differenza routing degli attributi e il routing convenzionale.</span><span class="sxs-lookup"><span data-stu-id="73c5c-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="73c5c-198">Routing degli attributi richiede più input per specificare una route; la route predefinita convenzionale gestisce le route più conciso.</span><span class="sxs-lookup"><span data-stu-id="73c5c-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="73c5c-199">Tuttavia, routing degli attributi consente e richiede un controllo preciso dei quali modelli route si applicano a ogni azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="73c5c-200">Con il nome del controller e i nomi di azione di routing attributo riprodurre **non** ruolo in cui è selezionata l'azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="73c5c-201">In questo esempio corrisponderà gli stessi URL dell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-201">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="73c5c-202">I modelli di route precedenti non definiscono i parametri di route per `action`, `area`, e `controller`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="73c5c-203">In realtà, questi parametri di route non sono consentiti in route di attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="73c5c-204">Poiché il modello di route è già associato a un'azione, il risulterebbero utili per analizzare il nome dell'azione dall'URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="73c5c-205">Attributo di routing con attributi Http [verbo]</span><span class="sxs-lookup"><span data-stu-id="73c5c-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="73c5c-206">Routing degli attributi può inoltre effettuare usano il `Http[Verb]` attributi quali `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="73c5c-207">Tutti questi attributi possono accettare un modello di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="73c5c-208">Questo esempio mostra due azioni che corrispondono al modello di route stesso:</span><span class="sxs-lookup"><span data-stu-id="73c5c-208">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="73c5c-209">Per un percorso URL come `/products` il `ProductsApi.ListProducts` azione verrà eseguita quando il verbo HTTP è `GET` e `ProductsApi.CreateProduct` verrà eseguito quando il verbo HTTP è `POST`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="73c5c-210">Attributo di routing corrisponde all'URL rispetto al set di modelli di route definite dagli attributi di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="73c5c-211">Una volta corrisponde a un modello di route, `IActionConstraint` i vincoli vengono applicati per determinare le azioni che possono essere eseguite.</span><span class="sxs-lookup"><span data-stu-id="73c5c-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="73c5c-212">Quando si compila un'API REST, è raro che è consigliabile usare `[Route(...)]` su un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="73c5c-213">Si consiglia di utilizzare più specifica `Http*Verb*Attributes` precisa su quelle supportate dall'API.</span><span class="sxs-lookup"><span data-stu-id="73c5c-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="73c5c-214">I client di API REST devono sapere quali verbi HTTP e percorsi di eseguire il mapping a specifiche operazioni logiche.</span><span class="sxs-lookup"><span data-stu-id="73c5c-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="73c5c-215">Poiché una route di attributo si applica a un'azione specifica, è ideale per ottenere i parametri richiesti come parte della definizione del modello di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="73c5c-216">In questo esempio, `id` è necessario come parte del percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="73c5c-217">Il `ProductsApi.GetProduct(int)` azione verrà eseguita per un percorso URL come `/products/3` ma non per un percorso URL come `/products`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="73c5c-218">Vedere [Routing](../../fundamentals/routing.md) per una descrizione completa dei modelli di route e le opzioni correlate.</span><span class="sxs-lookup"><span data-stu-id="73c5c-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="73c5c-219">Nome di route</span><span class="sxs-lookup"><span data-stu-id="73c5c-219">Route Name</span></span>

<span data-ttu-id="73c5c-220">Il codice seguente definisce un *nome di route* di `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="73c5c-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="73c5c-221">I nomi di route è utilizzabile per generare un URL basato su un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="73c5c-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="73c5c-222">I nomi di route non avere un impatto sull'URL di comportamento di routing di corrispondenza e vengono usati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="73c5c-223">I nomi di route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-224">Ciò si differenzia convenzionali *route predefinita*, che definisce il `id` parametro come facoltativi (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="73c5c-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="73c5c-225">La possibilità di specificare in modo preciso le API presenta vantaggi, ad esempio per consentire `/products` e `/products/5` deve essere inviato a diverse azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="73c5c-226">Route di combinazione</span><span class="sxs-lookup"><span data-stu-id="73c5c-226">Combining routes</span></span>

<span data-ttu-id="73c5c-227">Affinché l'attributo routing meno ricorrenti, attributi delle route sul controller vengono combinati con attributi di route per le singole azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="73c5c-228">I modelli di route definiti nel controller vengono anteposti ai modelli di route alle azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="73c5c-229">Inserendo un attributo della route nel controller di **tutti** azioni nel controller di utilizzano il routing di attributo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="73c5c-230">In questo esempio il percorso URL `/products` può corrispondere a `ProductsApi.ListProducts`e il percorso URL `/products/5` può corrispondere a `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="73c5c-231">Entrambe le azioni corrisponde solo HTTP `GET` perché sono contrassegnati con il `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-231">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="73c5c-232">Distribuire modelli applicati a un'azione che iniziano con un `/` non combinare con i modelli di route applicati al controller.</span><span class="sxs-lookup"><span data-stu-id="73c5c-232">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="73c5c-233">In questo esempio corrisponde a un set di percorsi URL simili a quelli di *route predefinita*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="73c5c-234">Route di attributi di ordinamento</span><span class="sxs-lookup"><span data-stu-id="73c5c-234">Ordering attribute routes</span></span>

<span data-ttu-id="73c5c-235">A differenza delle route convenzionale che vengono eseguiti in un ordine definito, routing degli attributi viene compilato un albero e corrisponde a tutte le route contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="73c5c-236">Si comporta come-se le voci della route inserite in un ordinamento ideale; le route più specifiche hanno la possibilità di eseguire prima le route più generale.</span><span class="sxs-lookup"><span data-stu-id="73c5c-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="73c5c-237">Ad esempio, una route come `blog/search/{topic}` è più specifico di una route come `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="73c5c-238">In termini logici di `blog/search/{topic}` route 'esegue' in primo luogo, per impostazione predefinita, poiché questo è l'ordinamento solo ragionevoli.</span><span class="sxs-lookup"><span data-stu-id="73c5c-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="73c5c-239">Utilizza il routing convenzionale, lo sviluppatore è responsabile dell'immissione delle route nell'ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="73c5c-240">Route di attributi è possono configurare un ordine, utilizzando il `Order` proprietà di tutti gli attributi di route framework fornito.</span><span class="sxs-lookup"><span data-stu-id="73c5c-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="73c5c-241">Le route vengono elaborate in base a un ordinamento crescente del `Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="73c5c-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="73c5c-242">L'ordine predefinito è `0`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-242">The default order is `0`.</span></span> <span data-ttu-id="73c5c-243">Impostazione di una route utilizzando `Order = -1` verrà eseguito prima che le route che non impostano un ordine.</span><span class="sxs-lookup"><span data-stu-id="73c5c-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="73c5c-244">Impostazione di una route utilizzando `Order = 1` verrà eseguito dopo l'ordinamento predefinito route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="73c5c-245">Evitare in base alle `Order`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="73c5c-246">Se lo spazio di URL richiede valori di ordine espliciti per indirizzare correttamente, è probabilmente confusione anche ai client.</span><span class="sxs-lookup"><span data-stu-id="73c5c-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="73c5c-247">In generale attributo routing selezionerà corretta route con URL corrispondente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="73c5c-248">Se l'ordine predefinito utilizzato per la generazione di URL non funziona, utilizzando il nome della route come una sostituzione è in genere più semplice rispetto all'applicazione il `Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="73c5c-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="73c5c-249">Token di sostituzione nei modelli di route ([controller], [azione], [area])</span><span class="sxs-lookup"><span data-stu-id="73c5c-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="73c5c-250">Per praticità, route di attributi supportano *sostituzione del token* da un token di inclusione tra parentesi graffe di quadrati (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="73c5c-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="73c5c-251">I token `[action]`, `[area]`, e `[controller]` verrà sostituito con i valori del nome dell'azione, nome dell'area e nome del controller dall'azione in cui è definita la route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="73c5c-252">In questo esempio le azioni possono corrispondere i percorsi URL come descritto nei commenti:</span><span class="sxs-lookup"><span data-stu-id="73c5c-252">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="73c5c-253">Sostituzione dei token si verifica come ultimo passaggio della creazione di route di attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-253">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="73c5c-254">L'esempio precedente si comporterà come il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="73c5c-254">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="73c5c-255">Route di attributi possono anche essere combinate con ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="73c5c-255">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="73c5c-256">Questo è particolarmente efficace combinato con la sostituzione dei token.</span><span class="sxs-lookup"><span data-stu-id="73c5c-256">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="73c5c-257">Sostituzione dei token si applica anche ai nomi di route definiti per una route di attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-257">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="73c5c-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]`verrà generato un nome di route univoca per ogni azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="73c5c-259">In modo che corrisponda al delimitatore valore letterale per la sostituzione dei token `[` o `]`, ripetendo il carattere di escape (`[[` o `]]`).</span><span class="sxs-lookup"><span data-stu-id="73c5c-259">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="73c5c-260">Più route</span><span class="sxs-lookup"><span data-stu-id="73c5c-260">Multiple Routes</span></span>

<span data-ttu-id="73c5c-261">Attributo di routing supporta la definizione di più route che raggiungono la stessa azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-261">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="73c5c-262">L'utilizzo più comune di questo oggetto è per simulare il comportamento del *route convenzionale predefinita* come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="73c5c-262">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="73c5c-263">Inserimento di più attributi delle route nel controller significa che ognuno di essi verrà combinati con gli attributi delle route in metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-263">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="73c5c-264">Quando più attributi delle route (che implementano `IActionConstraint`) si trovano in un'azione, quindi ogni vincolo azione combina con il modello di route dall'attributo che lo definisce.</span><span class="sxs-lookup"><span data-stu-id="73c5c-264">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="73c5c-265">Utilizzo di più route in azioni può sembrare potente, è preferibile mantenere spazio URL dell'applicazione semplice e ben definito.</span><span class="sxs-lookup"><span data-stu-id="73c5c-265">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="73c5c-266">Utilizzare più route azioni solo in caso di necessità, ad esempio per supportare i client esistenti.</span><span class="sxs-lookup"><span data-stu-id="73c5c-266">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="73c5c-267">Specifica di parametri facoltativi route di attributo, i valori predefiniti e vincoli</span><span class="sxs-lookup"><span data-stu-id="73c5c-267">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="73c5c-268">Route di attributi supportano la stessa sintassi inline come route convenzionale per specificare i parametri facoltativi, i valori predefiniti e vincoli.</span><span class="sxs-lookup"><span data-stu-id="73c5c-268">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="73c5c-269">Vedere [riferimento del modello di Route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi di modello di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-269">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="73c5c-270">Attributi di route personalizzati utilizzando`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="73c5c-270">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="73c5c-271">Tutti gli attributi di route disponibili in framework ( `[Route(...)]`, `[HttpGet(...)]` e così via) implementare il `IRouteTemplateProvider` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="73c5c-271">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="73c5c-272">MVC esegue la ricerca per gli attributi nelle classi controller e metodi di azione quando l'app viene avviata e utilizza quelle che implementano `IRouteTemplateProvider` per compilare il set iniziale di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-272">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="73c5c-273">È possibile implementare `IRouteTemplateProvider` per definire gli attributi della route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-273">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="73c5c-274">Ogni `IRouteTemplateProvider` consente di definire una singola route con un modello di route personalizzati, ordinamento e assegnare un nome:</span><span class="sxs-lookup"><span data-stu-id="73c5c-274">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="73c5c-275">Imposta automaticamente l'attributo dell'esempio precedente il `Template` a `"api/[controller]"` quando `[MyApiController]` viene applicato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-275">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="73c5c-276">Utilizzando il modello di applicazione per personalizzare le route di attributo</span><span class="sxs-lookup"><span data-stu-id="73c5c-276">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="73c5c-277">Il *modello applicativo* è un modello a oggetti creato all'avvio con tutti i metadati utilizzati da MVC per distribuire ed eseguire le azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-277">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="73c5c-278">Il *modello applicativo* include tutti i dati raccolti da attributi delle route (tramite `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="73c5c-278">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="73c5c-279">È possibile scrivere *convenzioni* per modificare il modello di applicazione in fase di avvio per personalizzare il comportamento di routing.</span><span class="sxs-lookup"><span data-stu-id="73c5c-279">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="73c5c-280">In questa sezione illustra un semplice esempio di personalizzazione di routing tramite il modello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-280">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="73c5c-281">Mista routing: attributo vs routing il routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="73c5c-281">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="73c5c-282">Le applicazioni MVC è possono combinare l'utilizzo di routing convenzionale e routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-282">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="73c5c-283">È in genere utilizzare convenzionale route per i controller di servire le pagine HTML per i browser e routing per controller serve le API REST dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-283">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="73c5c-284">Azioni di vengono tradizionalmente indirizzate o attributo indirizzato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-284">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="73c5c-285">Inserimento di una route del controller o l'azione rende attributo indirizzato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-285">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="73c5c-286">Le azioni che definiscono le route di attributo non è raggiungibile tramite la route convenzionale e viceversa.</span><span class="sxs-lookup"><span data-stu-id="73c5c-286">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="73c5c-287">**Qualsiasi** degli attributi delle route nel controller rende tutte le azioni nell'attributo controller indirizzato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-287">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-288">Ciò che distingue i due tipi di sistemi di routing è il processo applicato dopo un URL corrisponde a un modello di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-288">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="73c5c-289">Nel routing convenzionale, vengono utilizzati i valori della route da quella per scegliere l'azione e il controller da una tabella di ricerca di tutte le azioni indirizzate convenzionale.</span><span class="sxs-lookup"><span data-stu-id="73c5c-289">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="73c5c-290">Routing degli attributi, ogni modello è già associata a un'azione, in non è necessaria alcuna ulteriore ricerca.</span><span class="sxs-lookup"><span data-stu-id="73c5c-290">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="73c5c-291">Generazione di URL</span><span class="sxs-lookup"><span data-stu-id="73c5c-291">URL Generation</span></span>

<span data-ttu-id="73c5c-292">Le applicazioni MVC è possono utilizzare funzionalità di generazione di URL del routing per generare URL i collegamenti alle azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-292">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="73c5c-293">Generazione degli URL Elimina URL hardcoded, rendere il codice più affidabile e gestibile.</span><span class="sxs-lookup"><span data-stu-id="73c5c-293">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="73c5c-294">In questa sezione vengono illustrate le funzionalità di generazione di URL fornite da MVC e riguarderà solo nozioni di base del funzionamento della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-294">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="73c5c-295">Vedere [Routing](../../fundamentals/routing.md) per una descrizione dettagliata della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-295">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="73c5c-296">Il `IUrlHelper` interfaccia è l'elemento sottostante dell'infrastruttura tra MVC e routing per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-296">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="73c5c-297">È possibile trovare un'istanza di `IUrlHelper` disponibili tramite il `Url` proprietà nel controller, visualizzazioni e i componenti di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-297">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="73c5c-298">In questo esempio, il `IUrlHelper` viene utilizzata l'interfaccia tramite la `Controller.Url` proprietà per generare un URL a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-298">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="73c5c-299">Se l'applicazione usa il valore predefinito di convenzionale route, il valore di `url` variabile corrisponderà alla stringa di percorso URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-299">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="73c5c-300">Il percorso dell'URL viene creato dal routing combinando i valori della route della richiesta corrente (valori di ambiente), con i valori passati a `Url.Action` e sostituendo i valori nel modello di route:</span><span class="sxs-lookup"><span data-stu-id="73c5c-300">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="73c5c-301">Ogni parametro di route nel modello di route ha valore sostituito da nomi corrispondenti con i valori e ambiente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-301">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="73c5c-302">Un parametro di route che non dispone di un valore è possibile utilizzare un valore predefinito se dispone di uno o essere ignorato se è facoltativo (come nel caso di `id` in questo esempio).</span><span class="sxs-lookup"><span data-stu-id="73c5c-302">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="73c5c-303">Generazione URL avrà esito negativo se qualsiasi parametro di route richiesto non dispone di un valore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-303">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="73c5c-304">Se la generazione di URL non riesce per una route, viene tentata la successiva route fino a quando non sono stati provati tutti i cicli o viene trovata una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="73c5c-304">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="73c5c-305">Nell'esempio di `Url.Action` sopra presuppone il routing convenzionale, ma URL generazione funziona allo stesso modo con il routing di attributo, sebbene i concetti sono diversi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-305">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="73c5c-306">Con il routing convenzionale, i valori della route vengono utilizzati per estendere un modello e i valori di route per `controller` e `action` in genere vengono incluse nel modello - questo procedimento funziona perché l'URL corrispondente al routing rispettare un *convenzione*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-306">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="73c5c-307">Nel routing degli attributi, i valori della route per `controller` e `action` non potranno essere visualizzati nel modello, vengono invece utilizzati per cercare il modello da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="73c5c-307">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="73c5c-308">Questo esempio viene utilizzato l'attributo di routing:</span><span class="sxs-lookup"><span data-stu-id="73c5c-308">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="73c5c-309">MVC compila una tabella di ricerca di tutte le azioni attributo indirizzato e corrisponderà il `controller` e `action` i valori per selezionare il modello di route da utilizzare per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-309">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="73c5c-310">Nell'esempio precedente, `custom/url/to/destination` viene generato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-310">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="73c5c-311">Generazione degli URL in base al nome di azione</span><span class="sxs-lookup"><span data-stu-id="73c5c-311">Generating URLs by action name</span></span>

<span data-ttu-id="73c5c-312">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="73c5c-312">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="73c5c-313">`Action`) e tutti i relativi overload tutti sono basati su tale concetto che si desidera specificare ciò che sta collegando a specificando un nome del controller e il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-313">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-314">Quando si utilizza `Url.Action`, i valori della route corrente per `controller` e `action` specificati per il valore è - `controller` e `action` fanno parte di entrambi *ambiente valori* **e** *valori*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-314">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="73c5c-315">Il metodo `Url.Action`, Usa sempre i valori correnti delle `action` e `controller` e genererà un percorso di URL che esegue il routing per l'azione corrente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-315">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="73c5c-316">Tentativo di utilizzare i valori nei valori di ambiente per ottenere informazioni che non sono stati forniti durante la generazione di un URL di routing.</span><span class="sxs-lookup"><span data-stu-id="73c5c-316">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="73c5c-317">Utilizzo di una route come `{a}/{b}/{c}/{d}` e i valori di ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, routing disponga di informazioni sufficienti per generare un URL senza valori aggiuntivi: poiché indirizzare tutti i parametri sono un valore.</span><span class="sxs-lookup"><span data-stu-id="73c5c-317">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="73c5c-318">Se è stato aggiunto il valore `{ d = Donovan }`, il valore `{ d = David }` verrà ignorato e il percorso URL generato sarà `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-318">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="73c5c-319">I percorsi URL sono gerarchici.</span><span class="sxs-lookup"><span data-stu-id="73c5c-319">URL paths are hierarchical.</span></span> <span data-ttu-id="73c5c-320">Nell'esempio precedente, se è stato aggiunto il valore `{ c = Cheryl }`, entrambi i valori `{ c = Carol, d = David }` verrà ignorato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-320">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="73c5c-321">In questo caso non è più disponibile un valore per `d` e generazione di URL avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-321">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="73c5c-322">È necessario specificare il valore desiderato di `c` e `d`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-322">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="73c5c-323">Si potrebbe aspettare di passaggi di questo problema con la route predefinita (`{controller}/{action}/{id?}`)- ma si verifica raramente ciò in pratica come `Url.Action` specificherà sempre in modo esplicito un `controller` e `action` valore.</span><span class="sxs-lookup"><span data-stu-id="73c5c-323">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="73c5c-324">Più overload di `Url.Action` accettano anche un ulteriore *i valori di route* oggetto per fornire valori per parametri di route diverso da `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-324">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="73c5c-325">In genere visualizzato utilizzata con `id` come `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-325">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="73c5c-326">Per convenzione il *i valori di route* è in genere un oggetto di tipo anonimo, ma può anche essere un `IDictionary<>` o *normale vecchio oggetto .NET*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-326">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="73c5c-327">I valori di route aggiuntive che non corrispondono a parametri di route vengono inseriti nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="73c5c-327">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="73c5c-328">Per creare un URL assoluto, usare un overload che accetta un `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="73c5c-328">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="73c5c-329">Generazione degli URL route</span><span class="sxs-lookup"><span data-stu-id="73c5c-329">Generating URLs by route</span></span>

<span data-ttu-id="73c5c-330">Il codice precedente è stato illustrato generazione di un URL passando il nome di azione e del controller.</span><span class="sxs-lookup"><span data-stu-id="73c5c-330">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="73c5c-331">`IUrlHelper`fornisce inoltre il `Url.RouteUrl` famiglia di metodi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-331">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="73c5c-332">Questi metodi sono simili a `Url.Action`, ma non copiano i valori correnti delle `action` e `controller` per i valori della route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-332">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="73c5c-333">L'utilizzo più comune consiste nello specificare un nome di route da utilizzare un percorso specifico per generare l'URL, in genere *senza* specificando un nome di azione o controller.</span><span class="sxs-lookup"><span data-stu-id="73c5c-333">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="73c5c-334">Generazione degli URL in formato HTML</span><span class="sxs-lookup"><span data-stu-id="73c5c-334">Generating URLs in HTML</span></span>

<span data-ttu-id="73c5c-335">`IHtmlHelper`fornisce il `HtmlHelper` metodi `Html.BeginForm` e `Html.ActionLink` per generare `<form>` e `<a>` elementi rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-335">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="73c5c-336">Utilizzano questi metodi di `Url.Action` metodo per generare un URL e accettino argomenti simili.</span><span class="sxs-lookup"><span data-stu-id="73c5c-336">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="73c5c-337">Il `Url.RouteUrl` se utilizzati insieme per `HtmlHelper` sono `Html.BeginRouteForm` e `Html.RouteLink` che hanno una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="73c5c-337">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="73c5c-338">TagHelpers generare URL tramite il `form` helper tag e `<a>` helper tag.</span><span class="sxs-lookup"><span data-stu-id="73c5c-338">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="73c5c-339">Entrambi questi utilizzare `IUrlHelper` per la relativa implementazione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-339">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="73c5c-340">Vedere [utilizzo dei moduli](../views/working-with-forms.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-340">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="73c5c-341">All'interno delle viste, le `IUrlHelper` è disponibile tramite il `Url` proprietà per tutte le generazioni di URL ad hoc non coperto da sopra indicate.</span><span class="sxs-lookup"><span data-stu-id="73c5c-341">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="73c5c-342">Generazione degli URL nei risultati di azione</span><span class="sxs-lookup"><span data-stu-id="73c5c-342">Generating URLS in Action Results</span></span>

<span data-ttu-id="73c5c-343">Gli esempi precedenti sono state illustrate utilizzando `IUrlHelper` in un controller, mentre l'utilizzo più comune in un controller consiste nel generare un URL come parte del risultato di un'azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-343">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="73c5c-344">Il `ControllerBase` e `Controller` classi di base forniscono metodi pratici per i risultati dell'azione che fanno riferimento a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-344">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="73c5c-345">Un utilizzo tipico consiste nel reindirizzamento dopo aver accettato l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-345">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="73c5c-346">I metodi di azione risultati factory seguono un modello simile ai metodi `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-346">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="73c5c-347">Caso speciale per le route convenzionale dedicate</span><span class="sxs-lookup"><span data-stu-id="73c5c-347">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="73c5c-348">Il routing convenzionale è possibile utilizzare un tipo speciale di definizione di una route denominata un *route convenzionale dedicato*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-348">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="73c5c-349">Nell'esempio seguente, la route denominata `blog` sia una route convenzionale dedicata.</span><span class="sxs-lookup"><span data-stu-id="73c5c-349">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="73c5c-350">Utilizzo di queste definizioni di route, `Url.Action("Index", "Home")` genererà il percorso URL `/` con il `default` route, ma il motivo?</span><span class="sxs-lookup"><span data-stu-id="73c5c-350">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="73c5c-351">Si potrebbero immaginare i valori della route `{ controller = Home, action = Index }` sarebbero sufficienti per generare un URL utilizzando `blog`, e il risultato sarebbe `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-351">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="73c5c-352">Le route convenzionale dedicate si basano su un comportamento speciale dei valori predefiniti che non dispongono di un parametro di route corrispondenti che impedisce la route "troppo greedy" con la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-352">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="73c5c-353">In questo caso i valori predefiniti sono `{ controller = Blog, action = Article }`e né `controller` né `action` viene visualizzato come un parametro di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-353">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="73c5c-354">Quando il routing esegue la generazione di URL, i valori forniti devono corrispondere i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="73c5c-354">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="73c5c-355">Generazione di URL utilizzando `blog` avrà esito negativo perché i valori `{ controller = Home, action = Index }` non corrispondono `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-355">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="73c5c-356">Routing quindi esegue il fallback per provare `default`, che ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="73c5c-356">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="73c5c-357">Aree</span><span class="sxs-lookup"><span data-stu-id="73c5c-357">Areas</span></span>

<span data-ttu-id="73c5c-358">[Aree](areas.md) sono una funzionalità MVC consentono di organizzare la funzionalità correlata in un gruppo come una struttura di cartelle (per le viste) e il routing-spazio dei nomi separato (per le azioni del controller).</span><span class="sxs-lookup"><span data-stu-id="73c5c-358">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="73c5c-359">Utilizzo di aree consente a più controller con lo stesso nome, un'applicazione purché hanno diversi *aree*.</span><span class="sxs-lookup"><span data-stu-id="73c5c-359">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="73c5c-360">Utilizzo di aree consente di creare una gerarchia allo scopo di routing mediante l'aggiunta di un altro parametro di route, `area` a `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-360">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="73c5c-361">In questa sezione verrà illustrate le modalità routing interagisce con aree - vedere [aree](areas.md) per informazioni dettagliate sull'utilizzo delle aree con le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-361">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="73c5c-362">Nell'esempio seguente configura MVC per l'utilizzo convenzionale route predefinita e un *route area* per un'area denominata `Blog`:</span><span class="sxs-lookup"><span data-stu-id="73c5c-362">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="73c5c-363">In caso di corrispondenza di un percorso URL come `/Manage/Users/AddUser`, la prima route produrrà i valori della route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-363">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="73c5c-364">Il `area` valore route è generato da un valore predefinito per `area`, in realtà la route creata `MapAreaRoute` equivale alla seguente:</span><span class="sxs-lookup"><span data-stu-id="73c5c-364">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="73c5c-365">`MapAreaRoute`Crea una route che utilizza un valore predefinito e vincolo per `area` utilizzando il nome dell'area specificata, in questo caso `Blog`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-365">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="73c5c-366">Il valore predefinito assicura che la route produce sempre `{ area = Blog, ... }`, che richiede il valore `{ area = Blog, ... }` per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-366">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="73c5c-367">Il routing convenzionale è dipendente dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="73c5c-367">Conventional routing is order-dependent.</span></span> <span data-ttu-id="73c5c-368">In generale, le route delle aree devono trovarsi in precedenza nella tabella di routing come se fossero più specifiche rispetto alle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="73c5c-368">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="73c5c-369">Utilizzando l'esempio precedente, i valori della route corrisponderebbe l'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="73c5c-369">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="73c5c-370">Il `AreaAttribute` è ciò che denota un controller come parte di un'area, si supponga che il controller è nel `Blog` area.</span><span class="sxs-lookup"><span data-stu-id="73c5c-370">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="73c5c-371">Controller senza un `[Area]` attributo non sono membri di qualsiasi area e verrà **non** corrisponde a quando il `area` viene fornito il valore di route dal routing.</span><span class="sxs-lookup"><span data-stu-id="73c5c-371">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="73c5c-372">Nell'esempio seguente, solo il primo controller elencati può corrispondere i valori della route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-372">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="73c5c-373">Lo spazio dei nomi di ogni controller è illustrato di seguito per motivi di completezza, in caso contrario i controller siano presenti una denominazione in conflitto e generare un errore del compilatore.</span><span class="sxs-lookup"><span data-stu-id="73c5c-373">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="73c5c-374">Spazi dei nomi di classe non hanno effetto sul routing di MVC.</span><span class="sxs-lookup"><span data-stu-id="73c5c-374">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="73c5c-375">I primi due controller sono membri di aree e corrispondenza solo quando viene fornito il proprio nome di ciascuna area per il `area` indirizzare valore.</span><span class="sxs-lookup"><span data-stu-id="73c5c-375">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="73c5c-376">Il terzo controller non è un membro di qualsiasi area e può essere la corrispondenza solo quando nessun valore per `area` viene fornito dal routing.</span><span class="sxs-lookup"><span data-stu-id="73c5c-376">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-377">In termini di ricerca *alcun valore*, l'assenza del `area` valore è lo stesso come se il valore per `area` sono null o una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="73c5c-377">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="73c5c-378">Quando si esegue un'azione all'interno di un'area, il valore per la route `area` saranno disponibili come un *valore ambiente* per il routing per l'utilizzo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="73c5c-378">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="73c5c-379">Ciò significa che per impostazione predefinita le aree di agire *sticky* per la generazione di URL, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-379">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="73c5c-380">Informazioni sui IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="73c5c-380">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="73c5c-381">In questa sezione è un'analisi approfondita su elementi interni di framework e come MVC sceglie un'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="73c5c-381">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="73c5c-382">Una tipica applicazione non è necessario un oggetto personalizzato`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="73c5c-382">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="73c5c-383">Probabilmente è già utilizzato `IActionConstraint` anche se non si ha familiarità con l'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="73c5c-383">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="73c5c-384">Il `[HttpGet]` attributo e così via `[Http-VERB]` implementano `IActionConstraint` per limitare l'esecuzione di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="73c5c-384">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="73c5c-385">Presupponendo che la route predefinita convenzionale, il percorso URL `/Products/Edit` produrrebbe i valori `{ controller = Products, action = Edit }`, quale corrisponderebbe **entrambi** delle azioni illustrate di seguito.</span><span class="sxs-lookup"><span data-stu-id="73c5c-385">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="73c5c-386">In `IActionConstraint` terminologia si direbbe che entrambe le azioni sono considerati candidati - come corrispondano i dati della route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-386">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="73c5c-387">Quando il `HttpGetAttribute` esegue, indicherà che *Edit()* è una corrispondenza per *ottenere* e non è una corrispondenza per qualsiasi altro verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="73c5c-387">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="73c5c-388">Il `Edit(...)` azione non dispone di tutti i vincoli definiti e pertanto corrisponderà a qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="73c5c-388">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="73c5c-389">Pertanto, presupponendo che un `POST` - solo `Edit(...)` corrispondente.</span><span class="sxs-lookup"><span data-stu-id="73c5c-389">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="73c5c-390">Tuttavia, per un `GET` entrambe le azioni possono tuttavia corrispondere -, tuttavia, un'azione con un `IActionConstraint` viene sempre considerata *migliori* rispetto a un'azione senza.</span><span class="sxs-lookup"><span data-stu-id="73c5c-390">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="73c5c-391">Pertanto, perché `Edit()` è `[HttpGet]` è considerato più specifica e verrà selezionato se possono corrispondere a entrambe le azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-391">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="73c5c-392">Concettualmente, `IActionConstraint` è una forma di *overload*, ma anziché l'overload dei metodi con lo stesso nome, è l'overload tra le azioni che corrispondono all'URL stesso.</span><span class="sxs-lookup"><span data-stu-id="73c5c-392">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="73c5c-393">Attributo di routing utilizza inoltre `IActionConstraint` e possono causare azioni dai diversi controller sia presa in considerazione candidati.</span><span class="sxs-lookup"><span data-stu-id="73c5c-393">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="73c5c-394">Implementazione IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="73c5c-394">Implementing IActionConstraint</span></span>

<span data-ttu-id="73c5c-395">Il modo più semplice per implementare un `IActionConstraint` consiste nel creare una classe derivata da `System.Attribute` e posizionarlo sul controller e azioni.</span><span class="sxs-lookup"><span data-stu-id="73c5c-395">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="73c5c-396">MVC rileverà automaticamente qualsiasi `IActionConstraint` che vengono applicati come attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-396">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="73c5c-397">È possibile utilizzare il modello di applicazione per applicare vincoli e questo problema è probabilmente l'approccio più flessibile, in quanto consente a metaprogram come vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="73c5c-397">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="73c5c-398">Nell'esempio seguente un vincolo sceglie un'azione in base a un *codice paese* dai dati di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-398">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="73c5c-399">Il [esempio completo su GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="73c5c-399">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="73c5c-400">Si è responsabili dell'implementazione di `Accept` (metodo) e scegliendo un 'Order' per il vincolo da eseguire.</span><span class="sxs-lookup"><span data-stu-id="73c5c-400">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="73c5c-401">In questo caso, il `Accept` restituisce `true` per indicare l'azione è una corrispondenza quando il `country` valore corrispondenze di route.</span><span class="sxs-lookup"><span data-stu-id="73c5c-401">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="73c5c-402">Questo comportamento è diverso da un `RouteValueAttribute` nel senso che consente il fallback a un'azione senza attributi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-402">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="73c5c-403">L'esempio mostra che se si definisce un `en-US` azione quindi un codice paese, ad esempio `fr-FR` eseguirà il fallback a un controller più generico che non dispone di `[CountrySpecific(...)]` applicato.</span><span class="sxs-lookup"><span data-stu-id="73c5c-403">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="73c5c-404">Il `Order` proprietà decide quali *fase* il vincolo fa parte di.</span><span class="sxs-lookup"><span data-stu-id="73c5c-404">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="73c5c-405">Vincoli relativi alle azioni eseguite in gruppi in base il `Order`.</span><span class="sxs-lookup"><span data-stu-id="73c5c-405">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="73c5c-406">Ad esempio, tutti i framework forniti gli attributi del metodo HTTP utilizzano la stessa `Order` valore in modo che eseguano la stessa fase.</span><span class="sxs-lookup"><span data-stu-id="73c5c-406">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="73c5c-407">È possibile avere altre fasi necessarie implementare i criteri desiderati.</span><span class="sxs-lookup"><span data-stu-id="73c5c-407">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="73c5c-408">Per scegliere un valore per `Order` considerare o meno il vincolo deve essere applicato prima di metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="73c5c-408">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="73c5c-409">I numeri più bassi eseguiti per primi.</span><span class="sxs-lookup"><span data-stu-id="73c5c-409">Lower numbers run first.</span></span>
