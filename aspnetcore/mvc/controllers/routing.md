---
title: Routing ad azioni del controller in ASP.NET Core
author: rick-anderson
description: Informazioni su come ASP.NET Core MVC usa middleware di routing per verificare la corrispondenza degli URL delle richieste in ingresso ed eseguirne il mapping alle azioni.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: be7da9eeaf64c2f52c095b5179ccc22db43d57c3
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242575"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="fe0c7-103">Routing ad azioni del controller in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe0c7-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="fe0c7-104">Di [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5)e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fe0c7-105">I controller di ASP.NET Core usano il [middleware](xref:fundamentals/middleware/index) di routing per trovare le corrispondenze con gli URL delle richieste in ingresso ed eseguirne il mapping alle [azioni](#action).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="fe0c7-106">Modelli di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-106">Routes templates:</span></span>

* <span data-ttu-id="fe0c7-107">Sono definiti nel codice di avvio o negli attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="fe0c7-108">Descrizione della corrispondenza dei percorsi URL con le [azioni](#action).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="fe0c7-109">Vengono usati per generare URL per i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="fe0c7-110">I collegamenti generati vengono in genere restituiti nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="fe0c7-111">Le azioni sono indirizzate [convenzionalmente](#cr) o [indirizzate agli attributi](#ar).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="fe0c7-112">L'inserimento di una route sul controller o sull' [azione](#action) lo rende instradato con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="fe0c7-113">Per altre informazioni, vedere [Routing misto](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="fe0c7-114">Questo documento:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-114">This document:</span></span>

* <span data-ttu-id="fe0c7-115">Illustra le interazioni tra MVC e il routing:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="fe0c7-116">Il modo in cui le app MVC tipiche usano le funzionalità di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="fe0c7-117">Vengono illustrati entrambi:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-117">Covers both:</span></span>
    * <span data-ttu-id="fe0c7-118">[Routing convenzionale](#cr) usato in genere con i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-118">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="fe0c7-119">*Routing degli attributi* usato con le API REST.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="fe0c7-120">Se si è interessati principalmente al routing per le API REST, passare alla sezione [routing degli attributi per le API REST](#ar) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="fe0c7-121">Vedere [routing](xref:fundamentals/routing) per i dettagli del routing avanzato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="fe0c7-122">Si riferisce al sistema di routing predefinito aggiunto in ASP.NET Core 3,0, denominato endpoint routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="fe0c7-123">È possibile usare i controller con la versione precedente di routing per motivi di compatibilità.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="fe0c7-124">Per istruzioni, vedere la [Guida alla migrazione 2.2-3.0](xref:migration/22-to-30) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="fe0c7-125">Per materiale di riferimento sul sistema di routing legacy, vedere la [versione 2,2 di questo documento](xref:mvc/controllers/routing?view=aspnetcore-2.2) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="fe0c7-126">Configurare la route convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-126">Set up conventional route</span></span>

<span data-ttu-id="fe0c7-127">Quando si usa il [routing convenzionale](#crd), `Startup.Configure` in genere presenta codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="fe0c7-128">All'interno della chiamata a <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> viene usato per creare una singola route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="fe0c7-129">Il nome della route singola è `default` route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-129">The single route is named `default` route.</span></span> <span data-ttu-id="fe0c7-130">La maggior parte delle app con controller e visualizzazioni usa un modello di route simile a quello della route `default`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="fe0c7-131">Le API REST devono usare il [routing degli attributi](#ar).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="fe0c7-132">Il modello di route `"{controller=Home}/{action=Index}/{id?}"`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="fe0c7-133">Corrisponde a un percorso URL come `/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="fe0c7-134">Estrae i valori della route `{ controller = Products, action = Details, id = 5 }` suddivisione in token il percorso.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="fe0c7-135">L'estrazione dei valori di route comporta una corrispondenza se l'app dispone di un controller denominato `ProductsController` e di un'azione `Details`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 <span data-ttu-id="fe0c7-136">Il metodo [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) è incluso nel [download di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) e viene usato per visualizzare le informazioni di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-136">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

  * <span data-ttu-id="fe0c7-137">`/Products/Details/5` modello associa il valore di `id = 5` per impostare il parametro `id` su `5`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-137">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="fe0c7-138">Per ulteriori informazioni, vedere [associazione di modelli](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-138">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="fe0c7-139">`{controller=Home}` definisce `Home` come `controller`predefinito.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-139">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="fe0c7-140">`{action=Index}` definisce `Index` come `action`predefinito.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-140">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="fe0c7-141">Il carattere `?` in `{id?}` definisce `id` come facoltativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-141">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="fe0c7-142">I parametri di route predefiniti e facoltativi non devono necessariamente essere presenti nel percorso URL per trovare una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-142">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="fe0c7-143">Vedere [Riferimento per i modelli di route](xref:fundamentals/routing#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-143">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="fe0c7-144">Corrisponde al percorso dell'URL `/`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-144">Matches the URL path `/`.</span></span>
* <span data-ttu-id="fe0c7-145">Produce i valori della route `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-145">Produces the route values `{ controller = Home, action = Index }`.</span></span>
* <span data-ttu-id="fe0c7-146">Il metodo [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) è incluso nel [download di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) e viene usato per visualizzare le informazioni di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-146">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

<span data-ttu-id="fe0c7-147">I valori per `controller` e `action` utilizzano i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-147">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="fe0c7-148">`id` non produce un valore poiché non esiste un segmento corrispondente nel percorso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-148">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="fe0c7-149">`/` corrisponde solo se esiste una `HomeController` e `Index` azione:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-149">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="fe0c7-150">Utilizzando la definizione del controller e il modello di route precedenti, viene eseguita l'azione `HomeController.Index` per i seguenti percorsi URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-150">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="fe0c7-151">Il percorso URL `/` usa i controller `Home` predefiniti del modello di route e `Index` azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-151">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="fe0c7-152">Il percorso URL `/Home` usa il modello di route predefinito `Index` azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-152">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="fe0c7-153">Il metodo pratico <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-153">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="fe0c7-154">Sostituisce</span><span class="sxs-lookup"><span data-stu-id="fe0c7-154">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="fe0c7-155">Il routing viene configurato usando il middleware <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> e <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-155">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="fe0c7-156">Per usare i controller:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-156">To use controllers:</span></span>

* <span data-ttu-id="fe0c7-157">Chiamare <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> all'interno `UseEndpoints` per mappare i controller [indirizzati degli attributi](#ar) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-157">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
* <span data-ttu-id="fe0c7-158">Chiamare <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> o <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>per eseguire il mapping di controller [indirizzati convenzionalmente](#cr) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-158">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="fe0c7-159">Routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-159">Conventional routing</span></span>

<span data-ttu-id="fe0c7-160">Il routing convenzionale viene usato con i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-160">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="fe0c7-161">La route predefinita (`default`):</span><span class="sxs-lookup"><span data-stu-id="fe0c7-161">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="fe0c7-162">è un esempio di *routing convenzionale*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-162">is an example of a *conventional routing*.</span></span> <span data-ttu-id="fe0c7-163">Viene chiamato *routing convenzionale* perché stabilisce una *convenzione* per i percorsi URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-163">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="fe0c7-164">Il primo segmento di percorso, `{controller=Home}`, viene mappato al nome del controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-164">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="fe0c7-165">Il secondo segmento, `{action=Index}`, viene mappato al nome dell' [azione](#action) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-165">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="fe0c7-166">Il terzo segmento, `{id?}` viene usato per una `id`facoltativa.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-166">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="fe0c7-167">Il `?` in `{id?}` lo rende facoltativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-167">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="fe0c7-168">`id` viene utilizzata per eseguire il mapping a un'entità del modello.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-168">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="fe0c7-169">Utilizzando questo `default` Route, il percorso dell'URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-169">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="fe0c7-170">`/Products/List` esegue il mapping all'azione di `ProductsController.List`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-170">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="fe0c7-171">`/Blog/Article/17` esegue il mapping a `BlogController.Article` e in genere il modello associa il parametro `id` a 17.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-171">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="fe0c7-172">Questo mapping:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-172">This mapping:</span></span>

* <span data-ttu-id="fe0c7-173">Si basa **solo**sui nomi del controller e dell' [azione](#action) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-173">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="fe0c7-174">Non è basato su spazi dei nomi, percorsi di file di origine o parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-174">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="fe0c7-175">L'uso del routing convenzionale con la route predefinita consente di creare l'app senza dover ottenere un nuovo modello di URL per ogni azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-175">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="fe0c7-176">Per un'app con azioni di stile [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) , con coerenza per gli URL tra i controller:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-176">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="fe0c7-177">Consente di semplificare il codice.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-177">Helps simplify the code.</span></span>
* <span data-ttu-id="fe0c7-178">Rende l'interfaccia utente più prevedibile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-178">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="fe0c7-179">Il `id` nel codice precedente viene definito come facoltativo dal modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-179">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="fe0c7-180">Le azioni possono essere eseguite senza l'ID facoltativo fornito come parte dell'URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-180">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="fe0c7-181">In genere, quando`id` viene omesso dall'URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-181">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="fe0c7-182">`id` è impostato su `0` dall'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-182">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="fe0c7-183">Non è stata trovata alcuna entità nel database corrispondente `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-183">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="fe0c7-184">Il [routing degli attributi](#ar) offre un controllo con granularità fine per rendere l'ID necessario per alcune azioni e non per altri.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-184">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="fe0c7-185">Per convenzione, la documentazione include parametri facoltativi come `id` quando è probabile che vengano visualizzati nell'uso corretto.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-185">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="fe0c7-186">La maggior parte delle app dovrebbe scegliere uno schema di routing semplice e descrittivo in modo che gli URL siano leggibili e significativi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-186">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="fe0c7-187">La route convenzionale predefinita `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-187">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="fe0c7-188">Supporta uno schema di routing semplice e descrittivo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-188">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="fe0c7-189">È un punto iniziale utile per le app basate su interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-189">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="fe0c7-190">È l'unico modello di route necessario per molte app dell'interfaccia utente Web.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-190">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="fe0c7-191">Per le app di interfaccia utente Web di dimensioni maggiori, un'altra route che usa le [aree](#areas) se di frequente è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-191">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="fe0c7-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> e <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="fe0c7-193">Assegnare automaticamente un valore di **ordine** ai rispettivi endpoint in base all'ordine in cui vengono richiamati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-193">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="fe0c7-194">Routing degli endpoint in ASP.NET Core 3,0 e versioni successive:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-194">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="fe0c7-195">Non ha un concetto di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-195">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="fe0c7-196">Non fornisce garanzie di ordinamento per l'esecuzione dell'estensibilità, tutti gli endpoint vengono elaborati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-196">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="fe0c7-197">Abilitare la [registrazione](xref:fundamentals/logging/index) per verificare in che modo le implementazioni del routing predefinite, ad esempio <xref:Microsoft.AspNetCore.Routing.Route>, corrispondono alle richieste.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-197">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="fe0c7-198">Il [routing degli attributi](#ar) è illustrato più avanti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-198">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="fe0c7-199">Più route convenzionali</span><span class="sxs-lookup"><span data-stu-id="fe0c7-199">Multiple conventional routes</span></span>

<span data-ttu-id="fe0c7-200">È possibile aggiungere più [Route convenzionali](#cr) all'interno `UseEndpoints` aggiungendo più chiamate a <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> e <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-200">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="fe0c7-201">In questo modo è possibile definire più convenzioni o aggiungere route convenzionali dedicate a un' [azione](#action)specifica, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-201">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="fe0c7-202">Il `blog` route nel codice precedente è una **Route convenzionale dedicata**.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-202">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="fe0c7-203">Viene chiamato Route convenzionale dedicata perché:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-203">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="fe0c7-204">Usa il [routing convenzionale](#cr).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-204">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="fe0c7-205">È dedicata a un' [azione](#action)specifica.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-205">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="fe0c7-206">Poiché `controller` e `action` non vengono visualizzati nel modello di route `"blog/{*article}"` come parametri:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-206">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="fe0c7-207">Possono avere solo i valori predefiniti `{ controller = "Blog", action = "Article" }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-207">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="fe0c7-208">Questa route viene sempre mappata all'azione `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-208">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="fe0c7-209">`/Blog`, `/Blog/Article`e `/Blog/{any-string}` sono gli unici percorsi URL che corrispondono alla route del Blog.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-209">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="fe0c7-210">L'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-210">The preceding example:</span></span>

* <span data-ttu-id="fe0c7-211">`blog` Route ha una priorità maggiore per le corrispondenze rispetto alla route `default` perché viene aggiunta per prima.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-211">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="fe0c7-212">È un esempio di routing in stile [Slug](https://developer.mozilla.org/docs/Glossary/Slug) in cui è tipico avere un nome di articolo come parte dell'URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-212">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="fe0c7-213">In ASP.NET Core 3,0 e versioni successive, il routing non:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-213">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="fe0c7-214">Definire un concetto denominato *Route*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-214">Define a concept called a *route*.</span></span> <span data-ttu-id="fe0c7-215">`UseRouting` aggiunge la corrispondenza della route alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-215">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="fe0c7-216">Il middleware di `UseRouting` esamina il set di endpoint definiti nell'app e seleziona la corrispondenza dell'endpoint migliore in base alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-216">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="fe0c7-217">Fornire garanzie sull'ordine di esecuzione dell'estensibilità, ad esempio <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> o <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-217">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="fe0c7-218">Vedere [routing](xref:fundamentals/routing) per il materiale di riferimento sul routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-218">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="fe0c7-219">Ordine di routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-219">Conventional routing order</span></span>

<span data-ttu-id="fe0c7-220">Il routing convenzionale corrisponde solo a una combinazione di azione e controller definiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-220">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="fe0c7-221">Questa operazione è progettata per semplificare i casi in cui le route convenzionali si sovrappongono.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-221">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="fe0c7-222">L'aggiunta di route tramite <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>e <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> assegna automaticamente un valore di ordine ai rispettivi endpoint in base all'ordine in cui vengono richiamati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-222">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="fe0c7-223">Corrisponde a una route visualizzata in precedenza con una priorità più alta.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-223">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="fe0c7-224">Il routing convenzionale dipende dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-224">Conventional routing is order-dependent.</span></span> <span data-ttu-id="fe0c7-225">In generale, le route con aree devono essere posizionate in precedenza perché sono più specifiche delle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-225">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="fe0c7-226">Le route [convenzionali dedicate](#dcr) con intercetta tutti i parametri di route come `{*article}` possono rendere troppo [greedy](xref:fundamentals/routing#greedy)una route, vale a dire che corrisponde agli URL per i quali si intende trovare una corrispondenza con altre route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-226">[Dedicated conventional routes](#dcr) with catch all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="fe0c7-227">Inserire le route greedy in un secondo momento nella tabella di route per evitare corrispondenze greedy.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-227">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="fe0c7-228">Risoluzione di azioni ambigue</span><span class="sxs-lookup"><span data-stu-id="fe0c7-228">Resolving ambiguous actions</span></span>

<span data-ttu-id="fe0c7-229">Quando due endpoint corrispondono tramite il routing, il routing deve eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-229">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="fe0c7-230">Scegliere il candidato migliore.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-230">Choose the best candidate.</span></span>
* <span data-ttu-id="fe0c7-231">Generazione di un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-231">Throw an exception.</span></span>

<span data-ttu-id="fe0c7-232">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="fe0c7-232">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="fe0c7-233">Il controller precedente definisce due azioni che corrispondono a:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-233">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="fe0c7-234">Percorso URL `/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-234">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="fe0c7-235">`{ controller = Products33, action = Edit, id = 17 }`dati di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-235">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="fe0c7-236">Si tratta di un modello tipico per i controller MVC:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-236">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="fe0c7-237">`Edit(int)` Visualizza un modulo per la modifica di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-237">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="fe0c7-238">`Edit(int, Product)` elabora il form inviato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-238">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="fe0c7-239">Per risolvere la route corretta:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-239">To resolve the correct route:</span></span>

* <span data-ttu-id="fe0c7-240">`Edit(int, Product)` viene selezionato quando la richiesta è un `POST`HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-240">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="fe0c7-241">`Edit(int)` viene selezionato quando il [verbo http](#verb) è qualsiasi altro elemento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-241">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="fe0c7-242">`Edit(int)` viene in genere chiamato tramite `GET`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-242">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="fe0c7-243">Il <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, viene fornito al routing in modo che sia possibile scegliere in base al metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-243">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="fe0c7-244">Il `HttpPostAttribute` rende `Edit(int, Product)` una corrispondenza migliore rispetto a `Edit(int)`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-244">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="fe0c7-245">È importante comprendere il ruolo degli attributi come `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-245">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="fe0c7-246">Attributi simili vengono definiti per altri [verbi HTTP](#verb).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-246">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="fe0c7-247">Nel [routing convenzionale](#cr), è comune per le azioni usare lo stesso nome di azione quando fanno parte di un flusso di lavoro di invio del modulo di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-247">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="fe0c7-248">Vedere ad esempio [esaminare i due metodi di azione di modifica](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-248">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="fe0c7-249">Se il routing non è in grado di scegliere un candidato migliore, viene generata un'<xref:System.Reflection.AmbiguousMatchException>, che elenca i più endpoint corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-249">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="fe0c7-250">Nomi di route convenzionali</span><span class="sxs-lookup"><span data-stu-id="fe0c7-250">Conventional route names</span></span>

<span data-ttu-id="fe0c7-251">Le stringhe `"blog"` e `"default"` negli esempi seguenti sono nomi di route convenzionali:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-251">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-252">I nomi di route assegnano alla route un nome logico.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-252">The route names give the route a logical name.</span></span> <span data-ttu-id="fe0c7-253">La route denominata può essere usata per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-253">The named route can be used for URL generation.</span></span> <span data-ttu-id="fe0c7-254">L'uso di una route denominata semplifica la creazione di URL quando l'ordinamento delle route può rendere complessa la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-254">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="fe0c7-255">I nomi delle route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-255">Route names must be unique application wide.</span></span>

<span data-ttu-id="fe0c7-256">Nomi di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-256">Route names:</span></span>

* <span data-ttu-id="fe0c7-257">Non hanno alcun effetto sulla corrispondenza degli URL o sulla gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-257">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="fe0c7-258">Vengono usati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-258">Are used only for URL generation.</span></span>

<span data-ttu-id="fe0c7-259">Il concetto di nome della route è rappresentato nel routing come [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-259">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="fe0c7-260">Il **nome della route** e il nome dell' **endpoint**:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-260">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="fe0c7-261">Sono intercambiabili.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-261">Are interchangeable.</span></span>
* <span data-ttu-id="fe0c7-262">Quello usato nella documentazione e nel codice dipende dall'API descritta.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-262">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="fe0c7-263">Routing degli attributi per le API REST</span><span class="sxs-lookup"><span data-stu-id="fe0c7-263">Attribute routing for REST APIs</span></span>

<span data-ttu-id="fe0c7-264">Le API REST devono usare il routing degli attributi per modellare la funzionalità dell'app come un set di risorse in cui le operazioni sono rappresentate da [verbi HTTP](#verb).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-264">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="fe0c7-265">Il routing con attributi usa un set di attributi per eseguire il mapping delle azioni direttamente ai modelli di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-265">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="fe0c7-266">Il codice di `StartUp.Configure` seguente è tipico per un'API REST e viene usato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-266">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="fe0c7-267">Nel codice precedente, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> viene chiamato all'interno `UseEndpoints` per eseguire il mapping dei controller indirizzati degli attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-267">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="fe0c7-268">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-268">In the following example:</span></span>

* <span data-ttu-id="fe0c7-269">Viene utilizzato il metodo `Configure` precedente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-269">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="fe0c7-270">`HomeController` corrisponde a un set di URL simile a quello della route convenzionale predefinita `{controller=Home}/{action=Index}/{id?}` corrisponde.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-270">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="fe0c7-271">L'azione `HomeController.Index` viene eseguita per uno dei percorsi URL `/`, `/Home`, `/Home/Index`o `/Home/Index/3`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-271">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="fe0c7-272">In questo esempio viene evidenziata una differenza di programmazione principale tra routing degli attributi e [routing convenzionale](#cr).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-272">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="fe0c7-273">Il routing degli attributi richiede un maggior numero di input per specificare una route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-273">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="fe0c7-274">La route predefinita convenzionale gestisce le route in maniera più concisa.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-274">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="fe0c7-275">Tuttavia, il routing degli attributi consente e richiede un controllo preciso dei modelli di route applicabili a ciascuna [azione](#action).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-275">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="fe0c7-276">Con il routing degli attributi, il nome del controller e i nomi delle azioni **non svolgono alcun** ruolo in cui viene confrontata l'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-276">With attribute routing, the controller name and action names play **no** role in which action is matched.</span></span> <span data-ttu-id="fe0c7-277">L'esempio seguente corrisponde agli stessi URL dell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-277">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="fe0c7-278">Il codice seguente usa la sostituzione dei token per `action` e `controller`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-278">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="fe0c7-279">Il codice seguente si applica `[Route("[controller]/[action]")]` al controller:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-279">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="fe0c7-280">Nel codice precedente, i modelli di metodo di `Index` devono anteporre `/` o `~/` ai modelli di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-280">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="fe0c7-281">I modelli di route applicati a un'azione che iniziano con `/` o `~/` non vengono combinati con i modelli di route applicati al controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-281">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="fe0c7-282">Vedere [precedenza del modello di route](xref:fundamentals/routing#rtp) per informazioni sulla selezione del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-282">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="fe0c7-283">Nomi riservati di routing</span><span class="sxs-lookup"><span data-stu-id="fe0c7-283">Reserved routing names</span></span>

<span data-ttu-id="fe0c7-284">Le parole chiave seguenti sono nomi di parametro di route riservati quando si usano i controller o Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-284">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="fe0c7-285">L'uso di `page` come parametro di route con routing degli attributi è un errore comune.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-285">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="fe0c7-286">Questa operazione comporta un comportamento incoerente e confuso con la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-286">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="fe0c7-287">I nomi di parametro speciali vengono usati dalla generazione dell'URL per determinare se un'operazione di generazione URL fa riferimento a una pagina Razor o a un controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-287">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="fe0c7-288">Modelli di verbi HTTP</span><span class="sxs-lookup"><span data-stu-id="fe0c7-288">HTTP verb templates</span></span>

<span data-ttu-id="fe0c7-289">ASP.NET Core dispone dei seguenti modelli di verbi HTTP:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-289">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="fe0c7-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="fe0c7-291">[HttpPost](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-291">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="fe0c7-292">[HttpPut](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="fe0c7-293">[HttpDelete](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="fe0c7-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="fe0c7-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="fe0c7-296">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-296">Route templates</span></span>

<span data-ttu-id="fe0c7-297">ASP.NET Core include i modelli di route seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-297">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="fe0c7-298">Tutti i [modelli di verbi HTTP](#verb) sono modelli di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-298">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="fe0c7-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="fe0c7-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="fe0c7-300">Routing degli attributi con attributi di verbi http</span><span class="sxs-lookup"><span data-stu-id="fe0c7-300">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="fe0c7-301">Si consideri il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-301">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="fe0c7-302">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-302">In the preceding code:</span></span>

* <span data-ttu-id="fe0c7-303">Ogni azione contiene l'attributo `[HttpGet]`, che vincola la corrispondenza solo alle richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-303">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="fe0c7-304">L'azione `GetProduct` include il modello di `"{id}"`, pertanto `id` viene aggiunto al modello di `"api/[controller]"` nel controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-304">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="fe0c7-305">Il modello di metodi è `"api/[controller]/"{id}""`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-305">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="fe0c7-306">Pertanto questa azione corrisponde solo alle richieste GET di per il form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`e così via.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-306">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="fe0c7-307">L'azione `GetIntProduct` contiene il modello di `"int/{id:int}")`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-307">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="fe0c7-308">Il `:int` parte del modello vincola i valori di route `id` alle stringhe che possono essere convertite in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-308">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="fe0c7-309">Una richiesta GET a `/api/test2/int/abc`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-309">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="fe0c7-310">Non corrisponde a questa azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-310">Doesn't match this action.</span></span>
  * <span data-ttu-id="fe0c7-311">Restituisce un errore [404 non trovato](https://developer.mozilla.org/docs/Web/HTTP/Status/404) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-311">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="fe0c7-312">L'azione `GetInt2Product` contiene `{id}` nel modello, ma non vincola `id` a valori che possono essere convertiti in un valore integer.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-312">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="fe0c7-313">Una richiesta GET a `/api/test2/int2/abc`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-313">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="fe0c7-314">Corrisponde a questa route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-314">Matches this route.</span></span>
  * <span data-ttu-id="fe0c7-315">L'associazione di modelli non riesce a convertire `abc` in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-315">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="fe0c7-316">Il parametro `id` del metodo è di tipo Integer.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-316">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="fe0c7-317">Restituisce una [richiesta non valida 400](https://developer.mozilla.org/docs/Web/HTTP/Status/400) perché l'associazione di modelli non è riuscita a convertire`abc` in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-317">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="fe0c7-318">Il routing degli attributi può utilizzare attributi <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> come <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>e <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-318">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="fe0c7-319">Tutti gli attributi del [verbo http](#verb) accettano un modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-319">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="fe0c7-320">Nell'esempio seguente vengono illustrate due azioni che corrispondono allo stesso modello di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-320">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="fe0c7-321">Utilizzando il percorso URL `/products3`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-321">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="fe0c7-322">L'azione `MyProductsController.ListProducts` viene eseguita quando il [verbo http](#verb) è `GET`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-322">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="fe0c7-323">L'azione `MyProductsController.CreateProduct` viene eseguita quando il [verbo http](#verb) è `POST`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-323">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="fe0c7-324">Quando si compila un'API REST, è raro che sia necessario usare `[Route(...)]` su un metodo di azione perché l'azione accetta tutti i metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-324">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="fe0c7-325">È preferibile usare l' [attributo verbo http](#verb) più specifico per essere precisi sul supporto dell'API.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-325">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="fe0c7-326">I client delle API REST devono sapere quali percorsi e verbi HTTP devono essere mappati a operazioni logiche specifiche.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-326">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="fe0c7-327">Le API REST devono usare il routing degli attributi per modellare la funzionalità dell'app come un set di risorse in cui le operazioni sono rappresentate da verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-327">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="fe0c7-328">Ciò significa che molte operazioni, ad esempio GET e POST nella stessa risorsa logica, utilizzano lo stesso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-328">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="fe0c7-329">Il routing degli attributi offre un livello di controllo necessario per progettare con attenzione il layout dell'endpoint pubblico di un'API.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-329">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="fe0c7-330">Poiché una route con attributi si applica a un'azione specifica, è facile fare in modo che i parametri siano richiesti come parte della definizione del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-330">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="fe0c7-331">Nell'esempio seguente `id` è richiesto come parte del percorso URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-331">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="fe0c7-332">L'azione `Products2ApiController.GetProduct(int)`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-332">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="fe0c7-333">Viene eseguito con un percorso URL come `/products2/3`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-333">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="fe0c7-334">Non viene eseguito con il percorso URL `/products2`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-334">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="fe0c7-335">L'attributo [[consums]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) consente a un'azione di limitare i tipi di contenuto della richiesta supportati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-335">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="fe0c7-336">Per altre informazioni, vedere [definire i tipi di contenuto della richiesta supportati con l'attributo consums](xref:web-api/index#consumes).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-336">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="fe0c7-337">Vedere [Routing](xref:fundamentals/routing) per una descrizione completa dei modelli di route e delle opzioni correlate.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-337">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="fe0c7-338">Per ulteriori informazioni su `[ApiController]`, vedere [attributo ApiController](xref:web-api/index##apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-338">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="fe0c7-339">Nome route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-339">Route name</span></span>

<span data-ttu-id="fe0c7-340">Il codice seguente definisce un nome di route di `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-340">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="fe0c7-341">I nomi di route possono essere usati per generare un URL in base a un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-341">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="fe0c7-342">Nomi di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-342">Route names:</span></span>

* <span data-ttu-id="fe0c7-343">Non hanno alcun effetto sul comportamento di corrispondenza dell'URL del routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-343">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="fe0c7-344">Vengono usati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-344">Are only used for URL generation.</span></span>

<span data-ttu-id="fe0c7-345">I nomi di route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-345">Route names must be unique application-wide.</span></span>

<span data-ttu-id="fe0c7-346">Confrontare il codice precedente con la route predefinita convenzionale, che definisce il parametro `id` come facoltativo (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-346">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="fe0c7-347">La possibilità di specificare con precisione le API offre vantaggi, ad esempio consentendo l'invio di `/products` e `/products/5` a diverse azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-347">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="fe0c7-348">Combinazione di route per attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-348">Combining attribute routes</span></span>

<span data-ttu-id="fe0c7-349">Per rendere il routing con attributi meno ripetitivo, gli attributi di route del controller vengono combinati con gli attributi di route delle singole azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-349">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="fe0c7-350">I modelli di route definiti per il controller vengono anteposti ai modelli di route delle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-350">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="fe0c7-351">Inserendo un attributo di route nel controller, **tutte** le azioni presenti nel controller useranno il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-351">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="fe0c7-352">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-352">In the preceding example:</span></span>

* <span data-ttu-id="fe0c7-353">Il percorso URL `/products` può corrispondere `ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-353">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="fe0c7-354">Il percorso URL `/products/5` può corrispondere `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-354">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="fe0c7-355">Entrambe le azioni corrispondono solo a `GET` HTTP perché sono contrassegnate con l'attributo `[HttpGet]`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-355">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="fe0c7-356">I modelli di route applicati a un'azione che iniziano con `/` o `~/` non vengono combinati con i modelli di route applicati al controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-356">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="fe0c7-357">Nell'esempio seguente viene individuata la corrispondenza di un set di percorsi URL simile a quello della route predefinita.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-357">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="fe0c7-358">Nella tabella seguente vengono illustrati gli attributi `[Route]` nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-358">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="fe0c7-359">Attributo</span><span class="sxs-lookup"><span data-stu-id="fe0c7-359">Attribute</span></span>               | <span data-ttu-id="fe0c7-360">Combina con `[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-360">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="fe0c7-361">Definisce il modello di route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-361">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="fe0c7-362">Sì</span><span class="sxs-lookup"><span data-stu-id="fe0c7-362">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="fe0c7-363">Sì</span><span class="sxs-lookup"><span data-stu-id="fe0c7-363">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="fe0c7-364">**No**</span><span class="sxs-lookup"><span data-stu-id="fe0c7-364">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="fe0c7-365">Sì</span><span class="sxs-lookup"><span data-stu-id="fe0c7-365">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="fe0c7-366">Ordine Route attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-366">Attribute route order</span></span>

<span data-ttu-id="fe0c7-367">Il routing compila un albero e corrisponde a tutti gli endpoint contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-367">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="fe0c7-368">Le voci della route si comportano come se fossero posizionate in un ordinamento ideale.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-368">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="fe0c7-369">Le route più specifiche hanno la possibilità di essere eseguite prima delle route più generali.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-369">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="fe0c7-370">Ad esempio, una route di attributo come `blog/search/{topic}` è più specifica di una route di attributo come `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-370">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="fe0c7-371">Per impostazione predefinita, il `blog/search/{topic}` Route ha priorità più elevata perché è più specifico.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-371">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="fe0c7-372">Utilizzando il [routing convenzionale](#cr), lo sviluppatore è responsabile dell'inserimento delle route nell'ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-372">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="fe0c7-373">Le route degli attributi possono configurare un ordine utilizzando la proprietà <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-373">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="fe0c7-374">Tutti gli [attributi di route](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) forniti dal framework includono `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-374">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="fe0c7-375">Le route vengono elaborate in base a un ordinamento crescente della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-375">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="fe0c7-376">L'ordine predefinito è `0`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-376">The default order is `0`.</span></span> <span data-ttu-id="fe0c7-377">L'impostazione di una route con `Order = -1` viene eseguita prima delle route che non impostano un ordine.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-377">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="fe0c7-378">L'impostazione di una route con `Order = 1` viene eseguita dopo l'ordinamento predefinito della route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-378">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="fe0c7-379">**Evitare** a seconda del `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-379">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="fe0c7-380">Se lo spazio URL di un'app richiede che i valori di ordine espliciti vengano indirizzati correttamente, è probabile che anche i client siano confusi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-380">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="fe0c7-381">In generale, il routing degli attributi seleziona la route corretta con l'URL corrispondente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-381">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="fe0c7-382">Se l'ordine predefinito usato per la generazione di URL non funziona, l'uso di un nome di route come override è in genere più semplice dell'applicazione della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-382">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="fe0c7-383">Considerare i seguenti due controller che definiscono entrambe le `/home`di route corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-383">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="fe0c7-384">La richiesta di `/home` con il codice precedente genera un'eccezione simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-384">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="fe0c7-385">L'aggiunta di `Order` a uno degli attributi di route risolve l'ambiguità:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-385">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="fe0c7-386">Con il codice precedente, `/home` esegue l'endpoint di `HomeController.Index`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-386">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="fe0c7-387">Per ottenere la `MyDemoController.MyIndex`, richiedere `/home/MyIndex`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-387">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="fe0c7-388">**Nota**:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-388">**Note**:</span></span>

* <span data-ttu-id="fe0c7-389">Il codice precedente è un esempio o una progettazione di routing insufficiente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-389">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="fe0c7-390">È stato usato per illustrare la proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-390">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="fe0c7-391">La proprietà `Order` risolve solo l'ambiguità e non è possibile trovare una corrispondenza per il modello.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-391">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="fe0c7-392">È preferibile rimuovere il modello di `[Route("Home")]`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-392">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="fe0c7-393">Per informazioni sull'ordine di route con Razor Pages, vedere [Razor Pages Route e convenzioni delle app: ordine di route](xref:razor-pages/razor-pages-conventions#route-order) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-393">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="fe0c7-394">In alcuni casi, viene restituito un errore HTTP 500 con route ambigue.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-394">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="fe0c7-395">Usare la [registrazione](xref:fundamentals/logging/index) per verificare quali endpoint hanno causato la `AmbiguousMatchException`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-395">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="fe0c7-396">Sostituzione di token nei modelli di route [controller], [azione], [area]</span><span class="sxs-lookup"><span data-stu-id="fe0c7-396">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="fe0c7-397">Per praticità, le route degli attributi supportano la sostituzione dei token per i parametri di route riservati, racchiudendo un token in uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-397">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="fe0c7-398">Parentesi quadre: `[]`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-398">Square braces: `[]`</span></span>
* <span data-ttu-id="fe0c7-399">Parentesi graffe: `{}`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-399">Curly braces: `{}`</span></span>

<span data-ttu-id="fe0c7-400">I token `[action]`, `[area]`e `[controller]` vengono sostituiti con i valori del nome dell'azione, il nome dell'area e il nome del controller dall'azione in cui è definita la route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-400">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="fe0c7-401">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-401">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="fe0c7-402">Corrisponde `/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-402">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="fe0c7-403">Corrisponde `/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-403">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="fe0c7-404">La sostituzione dei token avviene come ultimo passaggio della creazione delle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-404">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="fe0c7-405">L'esempio precedente si comporta come il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-405">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="fe0c7-406">Le route con attributi possono anche essere combinate con l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-406">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="fe0c7-407">Questa funzionalità è potente in combinazione con la sostituzione dei token.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-407">This is powerful combined with token replacement.</span></span> <span data-ttu-id="fe0c7-408">La sostituzione dei token si applica anche ai nomi di route definiti dalle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-408">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="fe0c7-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`genera un nome di route univoco per ogni azione:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="fe0c7-410">La sostituzione dei token si applica anche ai nomi di route definiti dalle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-410">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="fe0c7-411">genera un nome di route univoco per ogni azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-411">generates a unique route name for each action.</span></span>

<span data-ttu-id="fe0c7-412">Per verificare la corrispondenza del delimitatore letterale della sostituzione di token `[` o `]`, eseguirne l'escape ripetendo il carattere (`[[` o `]]`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-412">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="fe0c7-413">Usare un trasformatore di parametri per personalizzare la sostituzione dei token</span><span class="sxs-lookup"><span data-stu-id="fe0c7-413">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="fe0c7-414">La sostituzione dei token può essere personalizzata usando un trasformatore di parametri.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-414">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="fe0c7-415">Un trasformatore di parametri implementa <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> e trasforma il valore dei parametri.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-415">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="fe0c7-416">Un trasformatore di parametro di `SlugifyParameterTransformer` personalizzato, ad esempio, modifica il valore della route di `SubscriptionManagement` in `subscription-management`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-416">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="fe0c7-417"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> è una convenzione del modello di applicazione che:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-417">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="fe0c7-418">Applica un trasformatore di parametri a tutte le route di attributi in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-418">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="fe0c7-419">Personalizza i valori dei token delle route di attributi quando vengono sostituiti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-419">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="fe0c7-420">Il metodo `ListAll` precedente corrisponde a `/subscription-management/list-all`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-420">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="fe0c7-421">La convenzione `RouteTokenTransformerConvention` è registrata come un'opzione in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-421">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="fe0c7-422">Per la definizione di Slug, vedere la [documentazione Web MDN in slug](https://developer.mozilla.org/docs/Glossary/Slug) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-422">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="fe0c7-423">Route di più attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-423">Multiple attribute routes</span></span>

<span data-ttu-id="fe0c7-424">Il routing con attributi supporta la definizione di più route che raggiungono la stessa azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-424">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="fe0c7-425">L'uso più comune è quello di simulare il comportamento della route convenzionale predefinita, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-425">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="fe0c7-426">L'inserimento di più attributi di route sul controller significa che ognuno di essi si combina con ognuno degli attributi di route nei metodi di azione:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-426">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="fe0c7-427">Tutti i vincoli di route di [verbi HTTP](#verb) implementano `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-427">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="fe0c7-428">Quando più attributi di route che implementano <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> vengono inseriti in un'azione:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-428">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="fe0c7-429">Ogni vincolo di azione viene combinato con il modello di route applicato al controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-429">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="fe0c7-430">L'uso di più route sulle azioni potrebbe sembrare utile e potente, ma è preferibile tenere lo spazio URL dell'app di base e ben definito.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-430">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="fe0c7-431">Usare più route sulle azioni **solo** se necessario, ad esempio per supportare i client esistenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-431">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="fe0c7-432">Definizione di parametri facoltativi, valori predefiniti e vincoli della route con attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-432">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="fe0c7-433">Le route con attributi supportano la stessa sintassi inline delle route convenzionali per specificare i parametri facoltativi, i valori predefiniti e i vincoli.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-433">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="fe0c7-434">Nel codice precedente `[HttpPost("product/{id:int}")]` applica un vincolo di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-434">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="fe0c7-435">L'azione `ProductsController.ShowProduct` corrisponde solo ai percorsi URL come `/product/3`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-435">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="fe0c7-436">La parte del modello di route `{id:int}` vincola il segmento solo ai numeri interi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-436">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="fe0c7-437">Vedere [Riferimento per i modelli di route](xref:fundamentals/routing#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-437">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="fe0c7-438">Attributi di Route personalizzati con IRouteTemplateProvider</span><span class="sxs-lookup"><span data-stu-id="fe0c7-438">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="fe0c7-439">Tutti gli [attributi di route](#rt) implementano <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-439">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="fe0c7-440">Il runtime di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-440">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="fe0c7-441">Cerca gli attributi nelle classi controller e nei metodi di azione all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-441">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="fe0c7-442">USA gli attributi che implementano `IRouteTemplateProvider` per compilare il set iniziale di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-442">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="fe0c7-443">Implementare `IRouteTemplateProvider` per definire gli attributi di Route personalizzati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-443">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="fe0c7-444">Ogni `IRouteTemplateProvider` consente di definire una singola route con un modello di route, un ordinamento e un nome personalizzati:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-444">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="fe0c7-445">Il metodo `Get` precedente restituisce `Order = 2, Template = api/MyTestApi`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-445">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="fe0c7-446">Usare il modello di applicazione per personalizzare le route degli attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-446">Use application model to customize attribute routes</span></span>

<span data-ttu-id="fe0c7-447">Modello di applicazione:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-447">The application model:</span></span>

* <span data-ttu-id="fe0c7-448">È un modello a oggetti creato all'avvio.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-448">Is an object model created at startup.</span></span>
* <span data-ttu-id="fe0c7-449">Contiene tutti i metadati usati da ASP.NET Core per indirizzare ed eseguire le azioni in un'app.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-449">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="fe0c7-450">Il modello di applicazione include tutti i dati raccolti dagli attributi di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-450">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="fe0c7-451">I dati degli attributi della route vengono forniti dall'implementazione del `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-451">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="fe0c7-452">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="fe0c7-452">Conventions:</span></span>

* <span data-ttu-id="fe0c7-453">È possibile scrivere per modificare il modello di applicazione per personalizzare il comportamento del routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-453">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="fe0c7-454">Vengono letti all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-454">Are read at app startup.</span></span>

<span data-ttu-id="fe0c7-455">Questa sezione illustra un esempio di base di personalizzazione del routing con il modello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-455">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="fe0c7-456">Il codice seguente rende le route approssimativamente allineate con la struttura di cartelle del progetto.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-456">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="fe0c7-457">Il codice seguente impedisce che la convenzione di `namespace` venga applicata ai controller indirizzati con l'attributo:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-457">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="fe0c7-458">Il controller seguente, ad esempio, non usa `NamespaceRoutingConvention`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-458">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="fe0c7-459">Il metodo `NamespaceRoutingConvention.Apply`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-459">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="fe0c7-460">Non esegue alcuna operazione se il controller è instradato all'attributo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-460">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="fe0c7-461">Imposta il modello del controller in base all'`namespace`, con la `namespace` di base rimossa.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-461">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="fe0c7-462">Il `NamespaceRoutingConvention` può essere applicato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-462">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="fe0c7-463">Si consideri, ad esempio, il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-463">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="fe0c7-464">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-464">In the preceding code:</span></span>

* <span data-ttu-id="fe0c7-465">Il `namespace` di base è `My.Application`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-465">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="fe0c7-466">Il nome completo del controller precedente è `My.Application.Admin.Controllers.UsersController`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-466">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="fe0c7-467">Il `NamespaceRoutingConvention` imposta il modello di controller su `Admin/Controllers/Users/[action]/{id?`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-467">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="fe0c7-468">Il `NamespaceRoutingConvention` può essere applicato anche come attributo in un controller:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-468">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="fe0c7-469">Routing misto: routing con attributi e routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-469">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="fe0c7-470">ASP.NET Core app possono combinare l'uso del routing convenzionale e del routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-470">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="fe0c7-471">In genere le route convenzionali vengono usate per i controller che gestiscono le pagine HTML per i browser e le route con attributi per i controller che gestiscono le API REST.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-471">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="fe0c7-472">Le azioni vengono indirizzate in modo convenzionale o con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-472">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="fe0c7-473">Se una route viene inserita nel controller o nell'azione, viene indirizzata con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-473">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="fe0c7-474">Le azioni che definiscono le route con attributi non possono essere raggiunte usando le route convenzionali e viceversa.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-474">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="fe0c7-475">**Qualsiasi** attributo di route nel controller esegue il routing di **tutte** le azioni nell'attributo controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-475">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="fe0c7-476">Il routing degli attributi e il routing convenzionale utilizzano lo stesso motore di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-476">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="fe0c7-477">Generazione di URL e valori di ambiente</span><span class="sxs-lookup"><span data-stu-id="fe0c7-477">URL Generation and ambient values</span></span>

<span data-ttu-id="fe0c7-478">Le app possono usare le funzionalità di generazione URL di routing per generare collegamenti URL alle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-478">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="fe0c7-479">La generazione di URL Elimina gli URL hardcoded, rendendo il codice più affidabile e gestibile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-479">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="fe0c7-480">Questa sezione è incentrata sulle funzionalità di generazione degli URL fornite da MVC e copre solo le nozioni di base sul funzionamento della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-480">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="fe0c7-481">Vedere [Routing](xref:fundamentals/routing) per una descrizione dettagliata della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-481">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="fe0c7-482">L'interfaccia <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> è l'elemento sottostante dell'infrastruttura tra MVC e il routing per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-482">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="fe0c7-483">Un'istanza di `IUrlHelper` è disponibile tramite la proprietà `Url` in controller, visualizzazioni e componenti di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-483">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="fe0c7-484">Nell'esempio seguente, l'interfaccia `IUrlHelper` viene utilizzata tramite la proprietà `Controller.Url` per generare un URL a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-484">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-485">Se l'app usa la route convenzionale predefinita, il valore della variabile `url` è la stringa del percorso URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-485">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="fe0c7-486">Questo percorso URL viene creato tramite il routing combinando:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-486">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="fe0c7-487">Valori della route dalla richiesta corrente, denominati **valori di ambiente**.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-487">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="fe0c7-488">I valori passati a `Url.Action` e sostituiscono tali valori nel modello di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-488">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="fe0c7-489">Il valore di ogni parametro di route del modello di route viene sostituito attraverso la corrispondenza dei nomi con i valori e i valori di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-489">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="fe0c7-490">Un parametro di route che non ha un valore può:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-490">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="fe0c7-491">Utilizzare un valore predefinito se ne è presente uno.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-491">Use a default value if it has one.</span></span>
* <span data-ttu-id="fe0c7-492">Viene ignorato se è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-492">Be skipped if it's optional.</span></span> <span data-ttu-id="fe0c7-493">Ad esempio, il `id` dal modello di route `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-493">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="fe0c7-494">La generazione di URL non riesce se un parametro di route obbligatorio non ha un valore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-494">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="fe0c7-495">Se la generazione di URL non riesce per una route, viene tentata la route successiva finché non vengono tentate tutte le route o viene trovata una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-495">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="fe0c7-496">Nell'esempio precedente di `Url.Action` viene presupposto il [routing convenzionale](#cr).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-496">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="fe0c7-497">La generazione di URL funziona in modo analogo al [routing degli attributi](#ar), sebbene i concetti siano diversi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-497">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="fe0c7-498">Con routing convenzionale:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-498">With conventional routing:</span></span>

* <span data-ttu-id="fe0c7-499">I valori di route vengono usati per espandere un modello.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-499">The route values are used to expand a template.</span></span>
* <span data-ttu-id="fe0c7-500">I valori di route per `controller` e `action` vengono in genere visualizzati in quel modello.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-500">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="fe0c7-501">Questa operazione funziona perché gli URL corrispondenti al routing rispettano una convenzione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-501">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="fe0c7-502">Nell'esempio seguente viene usato il routing degli attributi:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-502">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-503">L'azione `Source` nel codice precedente genera `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-503">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="fe0c7-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> è stato aggiunto in ASP.NET Core 3,0 come alternativa al `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="fe0c7-505">`LinkGenerator` offre funzionalità simili ma più flessibili.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-505">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="fe0c7-506">Anche i metodi su `IUrlHelper` hanno una famiglia di metodi corrispondente `LinkGenerator`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-506">Each other the methods on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="fe0c7-507">Generazione di URL in base al nome dell'azione</span><span class="sxs-lookup"><span data-stu-id="fe0c7-507">Generating URLs by action name</span></span>

<span data-ttu-id="fe0c7-508">[URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)e tutti gli overload correlati sono progettati per generare l'endpoint di destinazione specificando il nome del controller e il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-508">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="fe0c7-509">Quando si usa `Url.Action`, i valori di route correnti per `controller` e `action` vengono forniti dal runtime:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-509">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="fe0c7-510">Il valore di `controller` e `action` sono parte dei [valori di ambiente](#ambient) e.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-510">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="fe0c7-511">Il metodo `Url.Action` usa sempre i valori correnti di `action` e `controller` e genera un percorso URL che indirizza all'azione corrente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-511">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="fe0c7-512">Il routing tenta di usare i valori nei valori di ambiente per inserire le informazioni non fornite durante la generazione di un URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-512">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="fe0c7-513">Si consideri una route come `{a}/{b}/{c}/{d}` con valori di ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-513">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="fe0c7-514">Il routing ha informazioni sufficienti per generare un URL senza valori aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-514">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="fe0c7-515">Il routing ha informazioni sufficienti perché tutti i parametri di route hanno un valore.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-515">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="fe0c7-516">Se viene aggiunto il valore `{ d = Donovan }`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-516">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="fe0c7-517">Il valore `{ d = David }` viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-517">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="fe0c7-518">Il percorso URL generato è `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-518">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="fe0c7-519">**Avviso**: i percorsi URL sono gerarchici.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-519">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="fe0c7-520">Nell'esempio precedente, se viene aggiunto il valore `{ c = Cheryl }`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-520">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="fe0c7-521">Entrambi i valori `{ c = Carol, d = David }` vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-521">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="fe0c7-522">Non esiste più un valore per `d` e la generazione dell'URL ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-522">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="fe0c7-523">Per generare un URL, è necessario specificare i valori desiderati di `c` e `d`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-523">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="fe0c7-524">È possibile che si verifichi questo problema con la route predefinita `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-524">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="fe0c7-525">Questo problema è raro in pratica perché `Url.Action` specifica sempre in modo esplicito un valore `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-525">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="fe0c7-526">Diversi overload di [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) accettano un oggetto dei valori di route per fornire valori per i parametri di route diversi da `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-526">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="fe0c7-527">L'oggetto valori di route viene utilizzato spesso con `id`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-527">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="fe0c7-528">Ad esempio: `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-528">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="fe0c7-529">Oggetto valori di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-529">The route values object:</span></span>

* <span data-ttu-id="fe0c7-530">Per convenzione è in genere un oggetto di tipo anonimo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-530">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="fe0c7-531">Può essere un `IDictionary<>` o un [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-531">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="fe0c7-532">I valori di route aggiuntivi che non corrispondono a parametri di route vengono inseriti nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-532">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="fe0c7-533">Il codice precedente genera `/Products/Buy/17?color=red`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-533">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="fe0c7-534">Il codice seguente genera un URL assoluto:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-534">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="fe0c7-535">Per creare un URL assoluto, usare uno dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-535">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="fe0c7-536">Overload che accetta un `protocol`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-536">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="fe0c7-537">Ad esempio, il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-537">For example, the preceding code.</span></span>
* <span data-ttu-id="fe0c7-538">[LinkGenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), che genera URI assoluti per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-538">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="fe0c7-539">Genera URL per Route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-539">Generate URLs by route</span></span>

<span data-ttu-id="fe0c7-540">Nel codice precedente è stata illustrata la generazione di un URL passando il nome del controller e dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-540">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="fe0c7-541">`IUrlHelper` fornisce anche la famiglia di metodi [URL. RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-541">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="fe0c7-542">Questi metodi sono simili a [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), ma non copiano i valori correnti di `action` e `controller` ai valori della route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-542">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="fe0c7-543">L'utilizzo più comune di `Url.RouteUrl`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-543">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="fe0c7-544">Specifica un nome di route per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-544">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="fe0c7-545">In genere non specifica un nome di controller o di azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-545">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-546">Il file Razor seguente genera un collegamento HTML al `Destination_Route`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-546">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="fe0c7-547">Genera URL in HTML e Razor</span><span class="sxs-lookup"><span data-stu-id="fe0c7-547">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="fe0c7-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> fornisce i metodi <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) e [HTML. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) per generare rispettivamente gli elementi `<form>` e `<a>`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="fe0c7-549">Questi metodi usano il metodo [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) per generare un URL e accettano argomenti simili.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-549">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="fe0c7-550">Gli oggetti `Url.RouteUrl` complementi di `HtmlHelper` sono `Html.BeginRouteForm` e `Html.RouteLink` e hanno una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-550">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="fe0c7-551">Gli helper tag generano gli URL attraverso l'helper tag `form` e l'helper tag `<a>`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-551">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="fe0c7-552">Entrambi usano `IUrlHelper` per la propria implementazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-552">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="fe0c7-553">Per ulteriori informazioni, vedere [Helper tag nei moduli](xref:mvc/views/working-with-forms) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-553">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="fe0c7-554">All'interno delle visualizzazioni `IUrlHelper` è reso disponibile dalla proprietà `Url` per tutte le generazioni di URL ad hoc che non rientrano nelle situazioni descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-554">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="fe0c7-555">Generazione dell'URL nei risultati dell'azione</span><span class="sxs-lookup"><span data-stu-id="fe0c7-555">URL generation in Action Results</span></span>

<span data-ttu-id="fe0c7-556">Negli esempi precedenti è stato illustrato l'uso di `IUrlHelper` in un controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-556">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="fe0c7-557">L'utilizzo più comune in un controller consiste nel generare un URL come parte del risultato di un'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-557">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="fe0c7-558">Le classi di base <xref:Microsoft.AspNetCore.Mvc.ControllerBase> e <xref:Microsoft.AspNetCore.Mvc.Controller> offrono metodi pratici per i risultati delle azioni che fanno riferimento a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-558">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="fe0c7-559">Un utilizzo tipico prevede il reindirizzamento dopo l'accettazione dell'input utente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-559">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="fe0c7-560">I metodi factory dei risultati delle azioni come <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> seguono un modello simile ai metodi in `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-560">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="fe0c7-561">Caso speciale per le route convenzionali dedicate</span><span class="sxs-lookup"><span data-stu-id="fe0c7-561">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="fe0c7-562">Il [routing convenzionale](#cr) può usare un tipo speciale di definizione di route denominata [Route convenzionale dedicata](#dcr).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-562">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="fe0c7-563">Nell'esempio seguente, la route denominata `blog` è una route convenzionale dedicata:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-563">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-564">Usando le definizioni di route precedenti, `Url.Action("Index", "Home")` genera il percorso URL `/` usando la route `default`, ma perché?</span><span class="sxs-lookup"><span data-stu-id="fe0c7-564">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="fe0c7-565">Si potrebbe pensare che i valori di route `{ controller = Home, action = Index }` siano sufficienti per generare un URL usando `blog` e che il risultato sia `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-565">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="fe0c7-566">Le [Route convenzionali dedicate](#dcr) si basano su un comportamento speciale dei valori predefiniti che non hanno un parametro di route corrispondente che impedisce che la route sia troppo [greedy](xref:fundamentals/routing#greedy) con la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-566">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="fe0c7-567">In questo caso i valori predefiniti sono `{ controller = Blog, action = Article }` e né `controller` né `action` vengono visualizzati come parametri di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-567">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="fe0c7-568">Quando il routing esegue la generazione di URL, i valori specificati devono corrispondere ai valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-568">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="fe0c7-569">La generazione di URL con `blog` ha esito negativo perché i valori `{ controller = Home, action = Index }` non corrispondono `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-569">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="fe0c7-570">Il routing quindi esegue il fallback per provare `default`, che ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-570">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="fe0c7-571">Aree</span><span class="sxs-lookup"><span data-stu-id="fe0c7-571">Areas</span></span>

<span data-ttu-id="fe0c7-572">Le [aree](xref:mvc/controllers/areas) sono una funzionalità di MVC utilizzata per organizzare le funzionalità correlate in un gruppo come separato:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-572">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="fe0c7-573">Spazio dei nomi di routing per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-573">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="fe0c7-574">Struttura di cartelle per le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-574">Folder structure for views.</span></span>

<span data-ttu-id="fe0c7-575">L'uso delle aree consente a un'app di avere più controller con lo stesso nome, purché abbiano aree diverse.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-575">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="fe0c7-576">Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area` a `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-576">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="fe0c7-577">Questa sezione illustra il modo in cui il routing interagisce con le aree.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-577">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="fe0c7-578">Per informazioni dettagliate sull'uso delle aree con le visualizzazioni, vedere [aree](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-578">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="fe0c7-579">Nell'esempio seguente viene configurato MVC per l'utilizzo della route convenzionale predefinita e di una route `area` per una `area` denominata `Blog`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-579">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="fe0c7-580">Nel codice precedente, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> viene chiamato per creare il `"blog_route"`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-580">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="fe0c7-581">Il secondo parametro, `"Blog"`, è il nome dell'area.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-581">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="fe0c7-582">Quando si corrisponde a un percorso URL come `/Manage/Users/AddUser`, il `"blog_route"` Route genera i valori di route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-582">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="fe0c7-583">Il valore di route `area` viene generato da un valore predefinito per `area`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-583">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="fe0c7-584">La route creata da `MapAreaControllerRoute` è equivalente alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-584">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="fe0c7-585">`MapAreaControllerRoute` crea una route che usa sia un valore predefinito che un vincolo per `area` usando il nome di area specificato, in questo caso `Blog`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-585">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="fe0c7-586">Il valore predefinito assicura che la route generi sempre `{ area = Blog, ... }`, il vincolo richiede il valore `{ area = Blog, ... }` per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-586">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="fe0c7-587">Il routing convenzionale dipende dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-587">Conventional routing is order-dependent.</span></span> <span data-ttu-id="fe0c7-588">In generale, le route con aree devono essere posizionate in precedenza perché sono più specifiche delle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-588">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="fe0c7-589">Utilizzando l'esempio precedente, i valori della route `{ area = Blog, controller = Users, action = AddUser }` corrispondono all'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-589">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="fe0c7-590">L'attributo [[area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) è quello che denota un controller come parte di un'area.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-590">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="fe0c7-591">Questo controller si trova nell'area `Blog`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-591">This controller is in the `Blog` area.</span></span> <span data-ttu-id="fe0c7-592">I controller senza un attributo `[Area]` non sono membri di nessuna area e non **corrispondono quando** il valore di route `area` viene fornito dal routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-592">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="fe0c7-593">Nell'esempio seguente solo il primo controller indicato può corrispondere ai valori di route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-593">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="fe0c7-594">Lo spazio dei nomi di ogni controller viene visualizzato qui per completezza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-594">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="fe0c7-595">Se il controller precedente usa lo stesso spazio dei nomi, verrà generato un errore del compilatore.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-595">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="fe0c7-596">Gli spazi dei nomi delle classi non hanno effetto sul routing di MVC.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-596">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="fe0c7-597">I primi due controller sono membri di aree e corrispondono solo quando viene specificato il relativo nome di area dal valore di route `area`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-597">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="fe0c7-598">Il terzo controller non è un membro di un'area e può corrispondere solo quando non vengono specificati valori per `area` dal routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-598">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="fe0c7-599">In termini di corrispondenza con *nessun valore*, l'assenza del valore `area` è come se il valore per `area` fosse Null o la stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-599">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="fe0c7-600">Quando si esegue un'azione all'interno di un'area, il valore di route per `area` è disponibile come [valore di ambiente](#ambient) per il routing da usare per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-600">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="fe0c7-601">Ciò significa che per impostazione predefinita le aree funzionano *temporaneamente* per la generazione di URL, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-601">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="fe0c7-602">Il codice seguente genera un URL per `/Zebra/Users/AddUser`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-602">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="fe0c7-603">Definizione di azione</span><span class="sxs-lookup"><span data-stu-id="fe0c7-603">Action definition</span></span>

<span data-ttu-id="fe0c7-604">I metodi pubblici in un controller, ad eccezione di quelli con l'attributo [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) , sono azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-604">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="fe0c7-605">Codice di esempio</span><span class="sxs-lookup"><span data-stu-id="fe0c7-605">Sample code</span></span>

 * <span data-ttu-id="fe0c7-606">Il metodo [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) è incluso nel [download di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) e viene usato per visualizzare le informazioni di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-606">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="fe0c7-607">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe0c7-607">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fe0c7-608">ASP.NET Core MVC usa [middleware](xref:fundamentals/middleware/index) di routing per verificare la corrispondenza degli URL delle richieste in ingresso ed eseguirne il mapping alle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-608">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="fe0c7-609">Le route sono definite nel codice di avvio o negli attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-609">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="fe0c7-610">Le route descrivono il modo in cui i percorsi URL devono corrispondere alle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-610">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="fe0c7-611">Vengono usate anche per generare gli URL (per i collegamenti) inviati nelle risposte.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-611">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="fe0c7-612">Le azioni vengono indirizzate in modo convenzionale o con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-612">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="fe0c7-613">Se una route viene inserita nel controller o nell'azione, viene indirizzata con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-613">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="fe0c7-614">Per altre informazioni, vedere [Routing misto](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-614">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="fe0c7-615">Questo documento illustra le interazioni tra MVC e il routing e il modo in cui le tipiche app MVC usano le funzionalità di routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-615">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="fe0c7-616">Vedere [Routing](xref:fundamentals/routing) per informazioni dettagliate sul routing avanzato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-616">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="fe0c7-617">Impostazione del middleware di routing</span><span class="sxs-lookup"><span data-stu-id="fe0c7-617">Setting up Routing Middleware</span></span>

<span data-ttu-id="fe0c7-618">Nel metodo *Configure* può apparire codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-618">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="fe0c7-619">Nella chiamata a `UseMvc`, si usa `MapRoute` per creare una singola route, a cui si farà riferimento come alla route predefinita (`default`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-619">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="fe0c7-620">La maggior parte delle app MVC userà una route con un modello simile alla route `default`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-620">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="fe0c7-621">Il modello di route `"{controller=Home}/{action=Index}/{id?}"` può corrispondere a un percorso URL come `/Products/Details/5` ed estrae i valori di route `{ controller = Products, action = Details, id = 5 }` suddividendo il percorso in token.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-621">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="fe0c7-622">MVC tenterà di individuare un controller denominato `ProductsController` e di eseguire l'azione `Details`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-622">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="fe0c7-623">Si noti che in questo esempio l'associazione di modelli usa il valore di `id = 5` per impostare il parametro `id` su `5` quando si richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-623">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="fe0c7-624">Per maggiori dettagli, vedere [Associazione di modelli](../models/model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-624">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="fe0c7-625">Usando la route `default`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-625">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="fe0c7-626">Il modello di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-626">The route template:</span></span>

* <span data-ttu-id="fe0c7-627">`{controller=Home}` definisce `Home` come oggetto `controller` predefinito</span><span class="sxs-lookup"><span data-stu-id="fe0c7-627">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="fe0c7-628">`{action=Index}` definisce `Index` come oggetto `action` predefinito</span><span class="sxs-lookup"><span data-stu-id="fe0c7-628">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="fe0c7-629">`{id?}` definisce `id` come facoltativo</span><span class="sxs-lookup"><span data-stu-id="fe0c7-629">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="fe0c7-630">I parametri di route predefiniti e facoltativi non devono necessariamente essere presenti nel percorso URL per trovare una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-630">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="fe0c7-631">Vedere [Riferimento per i modelli di route](xref:fundamentals/routing#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-631">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="fe0c7-632">`"{controller=Home}/{action=Index}/{id?}"` può corrispondere al percorso URL `/` e restituirà i valori di route `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-632">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="fe0c7-633">I valori per `controller` e `action` usano i valori predefiniti, `id` non genera un valore perché non esiste un segmento corrispondente nel percorso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-633">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="fe0c7-634">MVC usa questi valori di route per selezionare l'azione `HomeController` e `Index`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-634">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="fe0c7-635">Usando questa definizione del controller e il modello di route, l'azione `HomeController.Index` viene eseguita per tutti i percorsi URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-635">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="fe0c7-636">Il metodo pratico `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-636">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="fe0c7-637">Può essere usato per sostituire:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-637">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="fe0c7-638">`UseMvc` e `UseMvcWithDefaultRoute` aggiungono un'istanza di `RouterMiddleware` alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-638">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="fe0c7-639">MVC non interagisce direttamente con il middleware e usa il routing per gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-639">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="fe0c7-640">MVC è connesso alle route attraverso un'istanza di `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-640">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="fe0c7-641">Il codice all'interno di `UseMvc` è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-641">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="fe0c7-642">`UseMvc` non definisce direttamente le route, aggiunge un segnaposto alla raccolta di route per la route `attribute`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-642">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="fe0c7-643">L'overload `UseMvc(Action<IRouteBuilder>)` consente di aggiungere le proprie route e supporta anche il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-643">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="fe0c7-644">`UseMvc` e tutte le relative varianti aggiungono un segnaposto per l'attributo Route: il routing degli attributi è sempre disponibile indipendentemente dalla modalità di configurazione `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-644">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="fe0c7-645">`UseMvcWithDefaultRoute` definisce una route predefinita e supporta il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-645">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="fe0c7-646">La sezione [Routing con attributi](#attribute-routing-ref-label) include maggiori dettagli sul routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-646">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="fe0c7-647">Routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-647">Conventional routing</span></span>

<span data-ttu-id="fe0c7-648">La route predefinita (`default`):</span><span class="sxs-lookup"><span data-stu-id="fe0c7-648">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="fe0c7-649">Il codice precedente è un esempio di routing convenzionale.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-649">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="fe0c7-650">Questo stile è denominato routing convenzionale perché stabilisce una *convenzione* per i percorsi URL:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-650">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="fe0c7-651">Il primo segmento di percorso viene mappato al nome del controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-651">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="fe0c7-652">Il secondo esegue il mapping al nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-652">The second maps to the action name.</span></span>
* <span data-ttu-id="fe0c7-653">Il terzo segmento viene usato per un `id`facoltativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-653">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="fe0c7-654">`id` esegue il mapping a un'entità del modello.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-654">`id` maps to a model entity.</span></span>

<span data-ttu-id="fe0c7-655">Usando la route `default`, il percorso URL `/Products/List` esegue il mapping all'azione `ProductsController.List` e `/Blog/Article/17` lo esegue a `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-655">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="fe0c7-656">Questo mapping si basa **solo** sui nomi dell'azione e del controller e non su spazi dei nomi, percorsi dei file di origine o parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-656">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="fe0c7-657">L'uso del routing convenzionale con la route predefinita consente di compilare rapidamente l'applicazione senza dover creare un nuovo modello di URL per ogni azione che si definisce.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-657">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="fe0c7-658">Per un'applicazione con azioni di stile CRUD, la coerenza degli URL tra i controller può semplificare il codice e rendere l'interfaccia utente più prevedibile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-658">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="fe0c7-659">L'oggetto `id` è definito come facoltativo nel modello di route, ovvero le azioni possono essere eseguite senza l'ID specificato come parte dell'URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-659">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="fe0c7-660">In genere, se si omette `id` dall'URL, il valore viene impostato su `0` dall'associazione del modello e di conseguenza non viene rilevata alcuna entità nel database corrispondente a `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-660">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="fe0c7-661">Il routing con attributi offre un controllo con granularità fine che consente di rendere l'ID obbligatorio per alcune azioni e non per altre.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-661">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="fe0c7-662">Per convenzione la documentazione include i parametri facoltativi come `id` quando è probabile che appaiano nell'uso corretto.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-662">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="fe0c7-663">Route multiple</span><span class="sxs-lookup"><span data-stu-id="fe0c7-663">Multiple routes</span></span>

<span data-ttu-id="fe0c7-664">È possibile aggiungere più route all'interno di `UseMvc` aggiungendo altre chiamate a `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-664">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="fe0c7-665">Questa operazione consente di definire più convenzioni o di aggiungere route convenzionali dedicate a un'azione specifica, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-665">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="fe0c7-666">La route `blog` è una *route convenzionale dedicata*, vale a dire che usa il sistema di routing convenzionale, ma è dedicata a un'azione specifica.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-666">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="fe0c7-667">Poiché `controller` e `action` non appaiono nel modello di route come parametri, possono avere solo i valori predefiniti e quindi questa route eseguirà sempre il mapping all'azione `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-667">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="fe0c7-668">Le route sono ordinate nella raccolta di route e verranno elaborate nell'ordine in cui vengono aggiunte.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-668">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="fe0c7-669">Quindi in questo esempio la route `blog` viene tentata prima della route `default`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-669">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-670">Le *Route convenzionali dedicate* usano spesso parametri di route **catch-all** come `{*article}` per acquisire la parte rimanente del percorso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-670">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="fe0c7-671">In questo modo una route può diventare troppo "greedy", ovvero corrispondere agli URL che dovrebbero corrispondere ad altre route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-671">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="fe0c7-672">Posizionare le route "greedy" più avanti nella tabella delle route per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-672">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="fe0c7-673">Fallback</span><span class="sxs-lookup"><span data-stu-id="fe0c7-673">Fallback</span></span>

<span data-ttu-id="fe0c7-674">Come parte dell'elaborazione della richiesta, MVC verifica che i valori di route possano essere usati per trovare un controller e un'azione nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-674">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="fe0c7-675">Se i valori di route non corrispondano a un'azione, la route non viene considerata una corrispondenza e viene tentata la route successiva.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-675">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="fe0c7-676">Questa operazione si chiama *fallback* e viene eseguita allo scopo di semplificare i casi in cui le route convenzionali si sovrappongono.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-676">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="fe0c7-677">Rimozione dell'ambiguità delle azioni</span><span class="sxs-lookup"><span data-stu-id="fe0c7-677">Disambiguating actions</span></span>

<span data-ttu-id="fe0c7-678">Quando due azioni corrispondono in base al routing, MVC deve rimuovere l'ambiguità per scegliere il candidato migliore, altrimenti genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-678">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="fe0c7-679">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="fe0c7-679">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="fe0c7-680">Questo controller definisce due azioni corrispondenti al percorso URL `/Products/Edit/17` e indirizzano i dati `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-680">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="fe0c7-681">Si tratta di un modello tipico per i controller MVC in cui `Edit(int)` visualizza un modulo per modificare un prodotto e `Edit(int, Product)` elabora il modulo inviato.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-681">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="fe0c7-682">Per poter eseguire l'operazione, MVC deve scegliere `Edit(int, Product)` se la richiesta è un `POST` HTTP e `Edit(int)` quando il verbo HTTP è un altro elemento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-682">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="fe0c7-683">`HttpPostAttribute` (`[HttpPost]`) è un'implementazione di `IActionConstraint` che consente di selezionare l'azione solo quando il verbo HTTP è `POST`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-683">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="fe0c7-684">La presenza di un oggetto `IActionConstraint` fa sì che `Edit(int, Product)` sia una corrispondenza migliore di `Edit(int)`, quindi `Edit(int, Product)` viene tentata per prima.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-684">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="fe0c7-685">È necessario scrivere implementazioni personalizzate di `IActionConstraint` solo in scenari specializzati, ma è importante comprendere il ruolo degli attributi, ad esempio `HttpPostAttribute`. Attributi simili vengono definiti per altri verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-685">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="fe0c7-686">Nel routing convenzionale è normale che le azioni usino lo stesso nome di azione quando fanno parte di un flusso di lavoro `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-686">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="fe0c7-687">La comodità offerta da questo modello sarà più evidente dopo aver esaminato la sezione relativa a [IActionConstraint](#understanding-iactionconstraint).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-687">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="fe0c7-688">Se vi sono più route corrispondenti e MVC non è in grado di trovare la route migliore, viene generata un'eccezione `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-688">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="fe0c7-689">Nomi di route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-689">Route names</span></span>

<span data-ttu-id="fe0c7-690">Le stringhe `"blog"` e `"default"` degli esempi seguenti sono nomi di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-690">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="fe0c7-691">I nomi di route consentono di assegnare a una route un nome logico, in modo che la route denominata possa essere usata per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-691">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="fe0c7-692">Questo semplifica notevolmente la creazione degli URL quando l'ordinamento delle route può rendere complessa la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-692">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="fe0c7-693">I nomi di route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-693">Route names must be unique application-wide.</span></span>

<span data-ttu-id="fe0c7-694">I nomi di route non influiscono sulla corrispondenza degli URL o sulla gestione delle richieste, vengono usati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-694">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="fe0c7-695">L'articolo sul [routing](xref:fundamentals/routing) contiene informazioni dettagliate sulla generazione di URL, inclusa la generazione negli helper specifici di MVC.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-695">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="fe0c7-696">Routing con attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-696">Attribute routing</span></span>

<span data-ttu-id="fe0c7-697">Il routing con attributi usa un set di attributi per eseguire il mapping delle azioni direttamente ai modelli di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-697">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="fe0c7-698">Nell'esempio seguente viene usato l'oggetto `app.UseMvc();` nel metodo `Configure` e non viene passata alcuna route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-698">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="fe0c7-699">`HomeController` corrisponde a un set di URL simile a quello a cui corrisponderebbe la route predefinita `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-699">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="fe0c7-700">L'azione `HomeController.Index()` verrà eseguita per tutti i percorsi URL, `/`, `/Home` o `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-700">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-701">In questo esempio viene evidenziata un'importante differenza a livello di programmazione tra il routing con attributi e il routing convenzionale.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-701">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="fe0c7-702">Il routing con attributi richiede più input per specificare una route, la route convenzionale predefinita gestisce le route in modo più conciso.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-702">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="fe0c7-703">Tuttavia, il routing con attributi consente, e richiede, un controllo preciso dei modelli route da applicare a ogni azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-703">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="fe0c7-704">Con il routing con attributi i nomi del controller e delle azioni **non** hanno alcun ruolo nella selezione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-704">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="fe0c7-705">In questo esempio viene verificata la corrispondenza degli stessi URL dell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-705">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="fe0c7-706">I modelli di route precedenti non definiscono i parametri di route per `action`, `area` e `controller`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-706">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="fe0c7-707">In realtà, questi parametri di route non sono consentiti nelle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-707">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="fe0c7-708">Poiché il modello di route è già associato a un'azione, non avrebbe senso analizzare il nome dell'azione dall'URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-708">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="fe0c7-709">Routing con attributi Http[Verb]</span><span class="sxs-lookup"><span data-stu-id="fe0c7-709">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="fe0c7-710">Il routing con attributi può usare anche gli attributi `Http[Verb]`, ad esempio `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-710">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="fe0c7-711">Tutti questi attributi possono accettare un modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-711">All of these attributes can accept a route template.</span></span> <span data-ttu-id="fe0c7-712">Questo esempio illustra due azioni che corrispondono allo stesso modello di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-712">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="fe0c7-713">Per un percorso URL come `/products` l'azione `ProductsApi.ListProducts` verrà eseguita quando il verbo HTTP è `GET` e `ProductsApi.CreateProduct` verrà eseguita quando il verbo HTTP è `POST`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-713">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="fe0c7-714">Nel routing con attributi l'URL viene prima confrontato con il set di modelli di route definiti dagli attributi della route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-714">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="fe0c7-715">Quando un modello di route corrisponde, vengono applicati i vincoli `IActionConstraint` per determinare quali azioni possono essere eseguite.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-715">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="fe0c7-716">Quando si compila un'API REST, è raro che si voglia usare `[Route(...)]` su un metodo di azione perché l'azione accetterà tutti i metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-716">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="fe0c7-717">Si consiglia di usare l'oggetto `Http*Verb*Attributes`, più specifico, per essere precisi circa gli elementi supportati dall'API.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-717">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="fe0c7-718">I client delle API REST devono sapere quali percorsi e verbi HTTP devono essere mappati a operazioni logiche specifiche.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-718">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="fe0c7-719">Poiché una route con attributi si applica a un'azione specifica, è facile fare in modo che i parametri siano richiesti come parte della definizione del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-719">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="fe0c7-720">In questo esempio `id` è richiesto come parte del percorso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-720">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="fe0c7-721">L'azione `ProductsApi.GetProduct(int)` verrà eseguita per un percorso URL come `/products/3`, ma non per un percorso URL come `/products`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-721">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="fe0c7-722">Vedere [Routing](xref:fundamentals/routing) per una descrizione completa dei modelli di route e delle opzioni correlate.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-722">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="fe0c7-723">Nome di route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-723">Route Name</span></span>

<span data-ttu-id="fe0c7-724">Nell’esempio di codice riportato di seguito viene definito un *nome di route* di `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-724">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="fe0c7-725">I nomi di route possono essere usati per generare un URL in base a un percorso specifico.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-725">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="fe0c7-726">I nomi di route non influiscono sul comportamento del routing riguardo alla corrispondenza degli URL e vengono usati solo per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-726">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="fe0c7-727">I nomi di route devono essere univoci a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-727">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-728">Confrontare questa opzione con la *route predefinita* convenzionale, che definisce il parametro `id` come facoltativo (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-728">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="fe0c7-729">La possibilità di specificare in modo preciso le API presenta dei vantaggi, ad esempio consente di inviare `/products` e `/products/5` ad azioni  differenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-729">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="fe0c7-730">Combinazione di route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-730">Combining routes</span></span>

<span data-ttu-id="fe0c7-731">Per rendere il routing con attributi meno ripetitivo, gli attributi di route del controller vengono combinati con gli attributi di route delle singole azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-731">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="fe0c7-732">I modelli di route definiti per il controller vengono anteposti ai modelli di route delle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-732">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="fe0c7-733">Inserendo un attributo di route nel controller, **tutte** le azioni presenti nel controller useranno il routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-733">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="fe0c7-734">In questo esempio il percorso URL `/products` può corrispondere a `ProductsApi.ListProducts` e il percorso URL `/products/5` può corrispondere a `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-734">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="fe0c7-735">Entrambe le azioni corrispondono solo a `GET` HTTP perché sono contrassegnate con l'`HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-735">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="fe0c7-736">I modelli di route applicati a un'azione che iniziano con `/` o `~/` non vengono combinati con i modelli di route applicati al controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-736">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="fe0c7-737">Questo esempio illustra la corrispondenza di un set di percorsi URL simile alla *route predefinita*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-737">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="fe0c7-738">Ordinamento delle route con attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-738">Ordering attribute routes</span></span>

<span data-ttu-id="fe0c7-739">Diversamente dalle route convenzionali, che vengono eseguite in un ordine definito, il routing degli attributi compila un albero e trova la corrispondenza di tutte le route simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-739">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="fe0c7-740">Si comporta come se le voci delle route seguissero un ordine ideale in cui le route più specifiche vengono probabilmente eseguite prima delle route più generali.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-740">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="fe0c7-741">Ad esempio, una route come `blog/search/{topic}` è più specifica di una route come `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-741">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="fe0c7-742">In termini logici la route `blog/search/{topic}` viene eseguita per prima, per impostazione predefinita, perché questo è l'ordine più plausibile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-742">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="fe0c7-743">Usando il routing convenzionale, lo sviluppatore ordina le route in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-743">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="fe0c7-744">Nelle route con attributi si può configurare un ordine usando la proprietà `Order` di tutti gli attributi di route specificati dal framework.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-744">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="fe0c7-745">Le route vengono elaborate in base a un ordinamento crescente della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-745">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="fe0c7-746">L'ordine predefinito è `0`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-746">The default order is `0`.</span></span> <span data-ttu-id="fe0c7-747">L'impostazione di una route usando `Order = -1` viene eseguita prima delle route per cui non è impostato un ordine.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-747">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="fe0c7-748">L'impostazione di una route usando `Order = 1` viene eseguito dopo l'ordinamento predefinito delle route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-748">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="fe0c7-749">Evitare la dipendenza da `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-749">Avoid depending on `Order`.</span></span> <span data-ttu-id="fe0c7-750">Se lo spazio URL richiede i valori in un ordine esplicito per indirizzare correttamente i dati, si può probabilmente creare confusione anche per i client.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-750">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="fe0c7-751">In genere il routing con attributi seleziona la route corretta con la corrispondenza di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-751">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="fe0c7-752">Se l'ordine predefinito usato per la generazione di URL non funziona, usare il nome della route come override è in genere più semplice che applicare la proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-752">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="fe0c7-753">Il routing di Razor Pages e il routing del controller MVC condividono un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-753">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="fe0c7-754">Per informazioni sull'ordine di routing negli argomenti di Razor Pages, vedere [Convenzioni di route e app per Razor Pages in ASP.NET Core: Ordine della route](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-754">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="fe0c7-755">Sostituzione dei token nei modelli di route ([controller], [action], [area])</span><span class="sxs-lookup"><span data-stu-id="fe0c7-755">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="fe0c7-756">Per praticità, le route con attributi supportano la *sostituzione dei token* eseguita racchiudendo i token tra parentesi quadre (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-756">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="fe0c7-757">I token `[action]`, `[area]` e `[controller]` vengono sostituiti con i valori di nome dell'azione, nome dell'area e nome del controller dall'azione in cui è definita la route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-757">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="fe0c7-758">Nell'esempio seguente le azioni corrispondono ai percorsi URL come descritto nei commenti:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-758">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="fe0c7-759">La sostituzione dei token avviene come ultimo passaggio della creazione delle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-759">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="fe0c7-760">L'esempio precedente si comporterà come il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-760">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="fe0c7-761">Le route con attributi possono anche essere combinate con l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-761">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="fe0c7-762">Ciò risulta particolarmente efficace se combinato con la sostituzione dei token.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-762">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="fe0c7-763">La sostituzione dei token si applica anche ai nomi di route definiti dalle route con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-763">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="fe0c7-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` genera un nome di route univoco per ogni azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="fe0c7-765">Per verificare la corrispondenza del delimitatore letterale della sostituzione di token `[` o `]`, eseguirne l'escape ripetendo il carattere (`[[` o `]]`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-765">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="fe0c7-766">Usare un trasformatore di parametri per personalizzare la sostituzione dei token</span><span class="sxs-lookup"><span data-stu-id="fe0c7-766">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="fe0c7-767">La sostituzione dei token può essere personalizzata usando un trasformatore di parametri.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-767">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="fe0c7-768">Un trasformatore di parametri implementa `IOutboundParameterTransformer` e trasforma il valore dei parametri.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-768">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="fe0c7-769">Ad esempio, un trasformatore di parametri `SlugifyParameterTransformer` personalizzato cambia il valore di route `SubscriptionManagement` in `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-769">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="fe0c7-770">`RouteTokenTransformerConvention` è una convenzione del modello di applicazione che:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-770">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="fe0c7-771">Applica un trasformatore di parametri a tutte le route di attributi in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-771">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="fe0c7-772">Personalizza i valori dei token delle route di attributi quando vengono sostituiti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-772">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="fe0c7-773">La convenzione `RouteTokenTransformerConvention` è registrata come un'opzione in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-773">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="fe0c7-774">Route multiple</span><span class="sxs-lookup"><span data-stu-id="fe0c7-774">Multiple Routes</span></span>

<span data-ttu-id="fe0c7-775">Il routing con attributi supporta la definizione di più route che raggiungono la stessa azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-775">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="fe0c7-776">L'uso più comune è simulare il comportamento della *route convenzionale predefinita* come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-776">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="fe0c7-777">Inserire più attributi di route nel controller significa che verranno tutti combinati con tutti gli attributi di route nei metodi dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-777">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="fe0c7-778">Quando più attributi di route (che implementano `IActionConstraint`) sono inseriti in un'azione, ogni vincolo dell'azione viene combinato con il modello di route dall'attributo che lo definisce.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-778">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="fe0c7-779">Benché l'uso di più route per le azioni possa sembrare efficace, è preferibile mantenere lo spazio URL dell'applicazione semplice e ben definito.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-779">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="fe0c7-780">Usare più route per le azioni solo se necessario, ad esempio per supportare i client esistenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-780">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="fe0c7-781">Definizione di parametri facoltativi, valori predefiniti e vincoli della route con attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-781">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="fe0c7-782">Le route con attributi supportano la stessa sintassi inline delle route convenzionali per specificare i parametri facoltativi, i valori predefiniti e i vincoli.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-782">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="fe0c7-783">Vedere [Riferimento per i modelli di route](xref:fundamentals/routing#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-783">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="fe0c7-784">Attributi di route personalizzati che usano `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-784">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="fe0c7-785">Tutti gli attributi di route disponibili nel framework (`[Route(...)]`, `[HttpGet(...)]` e così via) implementano l'interfaccia `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-785">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="fe0c7-786">MVC cerca gli attributi per le classi del controller e i metodi delle azioni all'avvio dell'app e usa quelli che implementano `IRouteTemplateProvider` per compilare il set iniziale di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-786">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="fe0c7-787">È possibile implementare `IRouteTemplateProvider` per definire i propri attributi di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-787">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="fe0c7-788">Ogni `IRouteTemplateProvider` consente di definire una singola route con un modello di route, un ordinamento e un nome personalizzati:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-788">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="fe0c7-789">L'attributo dell'esempio precedente imposta automaticamente `Template` su `"api/[controller]"` quando si applica `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-789">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="fe0c7-790">Uso del modello applicativo per personalizzare le route con attributi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-790">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="fe0c7-791">Il *modello applicativo* è un modello a oggetti creato all'avvio con tutti i metadati usati da MVC per indirizzare ed eseguire le azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-791">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="fe0c7-792">Il *modello applicativo* include tutti i dati raccolti da attributi di route (attraverso `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-792">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="fe0c7-793">È possibile scrivere *convenzioni* per modificare il modello applicativo in fase di avvio per personalizzare il comportamento del routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-793">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="fe0c7-794">Questa sezione illustra un semplice esempio di personalizzazione del routing con il modello applicativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-794">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="fe0c7-795">Routing misto: routing con attributi e routing convenzionale</span><span class="sxs-lookup"><span data-stu-id="fe0c7-795">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="fe0c7-796">Le applicazioni MVC sono in grado di usare una combinazione di routing convenzionale e routing con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-796">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="fe0c7-797">In genere le route convenzionali vengono usate per i controller che gestiscono le pagine HTML per i browser e le route con attributi per i controller che gestiscono le API REST.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-797">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="fe0c7-798">Le azioni vengono indirizzate in modo convenzionale o con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-798">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="fe0c7-799">Se una route viene inserita nel controller o nell'azione, viene indirizzata con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-799">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="fe0c7-800">Le azioni che definiscono le route con attributi non possono essere raggiunte usando le route convenzionali e viceversa.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-800">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="fe0c7-801">**Qualsiasi** attributo di route del controller rende tutte le azioni presenti nel controller indirizzate con attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-801">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-802">Ciò che distingue i due tipi di sistema di routing è il processo applicato dopo che si è verificata la corrispondenza di un URL con un modello di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-802">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="fe0c7-803">Nel routing convenzionale i valori di route della corrispondenza vengono usati per scegliere l'azione e il controller da una tabella di ricerca di tutte le azioni indirizzate in modo convenzionale.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-803">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="fe0c7-804">Nel routing con attributi ogni modello è già associato a un'azione e non è necessaria alcuna ulteriore ricerca.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-804">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="fe0c7-805">Segmenti complessi</span><span class="sxs-lookup"><span data-stu-id="fe0c7-805">Complex segments</span></span>

<span data-ttu-id="fe0c7-806">I segmenti complessi (ad esempio, `[Route("/dog{token}cat")]`), vengono elaborati individuando corrispondenze per i valori letterali da destra a sinistra in modalità non-greedy.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-806">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="fe0c7-807">Vedere [il codice sorgente](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) per una descrizione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-807">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="fe0c7-808">Per altre informazioni, vedere [questo problema](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-808">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="fe0c7-809">Generazione di URL</span><span class="sxs-lookup"><span data-stu-id="fe0c7-809">URL Generation</span></span>

<span data-ttu-id="fe0c7-810">Le applicazioni MVC sono in grado di usare le funzionalità di generazione di URL del routing per generare i collegamenti URL alle azioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-810">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="fe0c7-811">La generazione di URL evita la codifica degli URL e consente di rendere il codice più affidabile e gestibile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-811">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="fe0c7-812">Questa sezione illustra le funzionalità di generazione di URL disponibili in MVC e tratta solo le nozioni di base sul funzionamento della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-812">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="fe0c7-813">Vedere [Routing](xref:fundamentals/routing) per una descrizione dettagliata della generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-813">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="fe0c7-814">L'interfaccia `IUrlHelper` è l'elemento sottostante dell'infrastruttura tra MVC e il routing per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-814">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="fe0c7-815">È possibile trovare un'istanza di `IUrlHelper` disponibile usando la proprietà `Url` in controller, visualizzazioni e componenti di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-815">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="fe0c7-816">In questo esempio l'interfaccia `IUrlHelper` viene usata attraverso la proprietà `Controller.Url` per generare un URL a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-816">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-817">Se l'applicazione usa la route convenzionale predefinita, il valore della variabile `url` sarà la stringa del percorso URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-817">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="fe0c7-818">Il percorso URL viene creato dal routing combinando i valori di route della richiesta corrente (valori di ambiente) con i valori passati a `Url.Action` e sostituendo tali valori nel modello di route:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-818">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="fe0c7-819">Il valore di ogni parametro di route del modello di route viene sostituito attraverso la corrispondenza dei nomi con i valori e i valori di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-819">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="fe0c7-820">Per un parametro di route senza un valore si può usare un valore predefinito, se ne esiste uno, oppure ignorare il parametro se è facoltativo (come nel caso di `id` in questo esempio).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-820">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="fe0c7-821">La generazione di URL avrà esito negativo se un parametro di route obbligatorio non ha un valore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-821">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="fe0c7-822">Se la generazione di URL non riesce per una route, viene tentata la route successiva finché non vengono tentate tutte le route o viene trovata una corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-822">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="fe0c7-823">Nell'esempio di `Url.Action` precedente si presuppone che il routing sia convenzionale, ma la generazione di URL funziona in modo analogo con il routing con attributi, sebbene i concetti siano differenti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-823">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="fe0c7-824">Con il routing convenzionale i valori di route vengono usati per espandere un modello e i valori di route per `controller` e `action` in genere appaiono in quel modello. Questo procedimento funziona perché gli URL corrispondenti in base al routing rispettano una *convenzione*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-824">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="fe0c7-825">Nel routing con attributi i valori di route per `controller` e `action` non possono essere visualizzati nel modello ma vengono usati per individuare il modello da usare.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-825">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="fe0c7-826">Questo esempio usa il routing con attributi:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-826">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="fe0c7-827">MVC compila una tabella di ricerca di tutte le azioni indirizzate con attributi e stabilisce la corrispondenza dei valori `controller` e `action` per selezionare il modello di route da usare per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-827">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="fe0c7-828">Nell'esempio precedente viene generato `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-828">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="fe0c7-829">Generazione di URL in base al nome dell'azione</span><span class="sxs-lookup"><span data-stu-id="fe0c7-829">Generating URLs by action name</span></span>

<span data-ttu-id="fe0c7-830">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-830">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="fe0c7-831">`Action`) e tutti gli overload correlati sono basati sull'idea che si voglia definire l'oggetto a cui ci si sta collegando specificando un nome di controller e un nome di azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-831">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-832">Quando si usa `Url.Action`, vengono specificati i valori di route correnti per `controller` e `action`: il valore di `controller` e `action` sono parte *dei valori di* *ambiente* **e** .</span><span class="sxs-lookup"><span data-stu-id="fe0c7-832">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="fe0c7-833">Il metodo `Url.Action`, usa sempre i valori correnti di `action` e `controller` e genererà un percorso URL che indirizza all'azione corrente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-833">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="fe0c7-834">Il routing tenta di usare i valori nei valori di ambiente per ottenere le informazioni non specificate durante la generazione di un URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-834">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="fe0c7-835">Usando una route come `{a}/{b}/{c}/{d}` e i valori di ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, il routing ha informazioni sufficienti per generare un URL senza valori aggiuntivi, poiché tutti i parametri di route hanno un valore.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-835">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="fe0c7-836">Se è stato aggiunto il valore `{ d = Donovan }`, il valore `{ d = David }` viene ignorato e il percorso URL generato è `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-836">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="fe0c7-837">I percorsi URL sono gerarchici.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-837">URL paths are hierarchical.</span></span> <span data-ttu-id="fe0c7-838">Nell'esempio precedente, se è stato aggiunto il valore `{ c = Cheryl }`, entrambi i valori `{ c = Carol, d = David }` vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-838">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="fe0c7-839">In questo caso non è più disponibile un valore per `d` e la generazione dell'URL avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-839">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="fe0c7-840">Sarà necessario specificare un valore per `c` e `d`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-840">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="fe0c7-841">In apparenza questo problema si può risolvere con la route predefinita (`{controller}/{action}/{id?}`), ma in realtà questo comportamento si verifica raramente perché `Url.Action` specifica sempre in modo esplicito un valore `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-841">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="fe0c7-842">Anche gli overload di `Url.Action` di maggiore durata accettano anche un oggetto *valori di route* aggiuntivo per specificare i valori per i parametri di route diversi da `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-842">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="fe0c7-843">In genere viene usato con `id` come `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-843">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="fe0c7-844">Per convenzione l'oggetto *valori di route* è in genere un oggetto di tipo anonimo, ma può anche essere un `IDictionary<>` o un *normale oggetto .NET*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-844">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="fe0c7-845">I valori di route aggiuntivi che non corrispondono a parametri di route vengono inseriti nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-845">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="fe0c7-846">Per creare un URL assoluto, usare un overload che accetta un `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="fe0c7-846">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="fe0c7-847">Generazione di URL in base alla route</span><span class="sxs-lookup"><span data-stu-id="fe0c7-847">Generating URLs by route</span></span>

<span data-ttu-id="fe0c7-848">Il codice precedente illustra come generare un URL passando il nome del controller e dell'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-848">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="fe0c7-849">`IUrlHelper` specifica anche la famiglia di metodi `Url.RouteUrl`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-849">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="fe0c7-850">Questi metodi sono simili a `Url.Action`, ma non copiano i valori correnti di `action` e `controller` nei valori di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-850">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="fe0c7-851">L'uso più comune consiste nello specificare un nome di route per usare un percorso specifico per generare l'URL, in genere *senza* specificare un nome di controller o azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-851">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="fe0c7-852">Generazione di URL in HTML</span><span class="sxs-lookup"><span data-stu-id="fe0c7-852">Generating URLs in HTML</span></span>

<span data-ttu-id="fe0c7-853">`IHtmlHelper` specifica i metodi `HtmlHelper``Html.BeginForm` e `Html.ActionLink` per generare rispettivamente gli elementi `<form>` e `<a>`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-853">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="fe0c7-854">Questi metodi usano il metodo `Url.Action` per generare un URL e accettano argomenti simili.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-854">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="fe0c7-855">Gli oggetti `Url.RouteUrl` complementi di `HtmlHelper` sono `Html.BeginRouteForm` e `Html.RouteLink` e hanno una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-855">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="fe0c7-856">Gli helper tag generano gli URL attraverso l'helper tag `form` e l'helper tag `<a>`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-856">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="fe0c7-857">Entrambi usano `IUrlHelper` per la propria implementazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-857">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="fe0c7-858">Per altre informazioni, vedere [Uso dei moduli](../views/working-with-forms.md).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-858">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="fe0c7-859">All'interno delle visualizzazioni `IUrlHelper` è reso disponibile dalla proprietà `Url` per tutte le generazioni di URL ad hoc che non rientrano nelle situazioni descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-859">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="fe0c7-860">Generazione di URL nei risultati delle azioni</span><span class="sxs-lookup"><span data-stu-id="fe0c7-860">Generating URLS in Action Results</span></span>

<span data-ttu-id="fe0c7-861">Negli esempi precedenti è stato usato `IUrlHelper` in un controller, ma l'uso più comune in un controller è generare un URL come parte del risultato di un'azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-861">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="fe0c7-862">Le classi di base `ControllerBase` e `Controller` offrono metodi pratici per i risultati delle azioni che fanno riferimento a un'altra azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-862">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="fe0c7-863">Un uso tipico è eseguire il reindirizzamento dopo aver accettato l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-863">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="fe0c7-864">I metodi factory dei risultati delle azioni seguono un modello simile ai metodi per `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-864">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="fe0c7-865">Caso speciale per le route convenzionali dedicate</span><span class="sxs-lookup"><span data-stu-id="fe0c7-865">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="fe0c7-866">Nel routing convenzionale è possibile usare un tipo speciale di definizione di route denominata *route convenzionale dedicata*.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-866">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="fe0c7-867">Nell'esempio seguente la route denominata `blog` è una route convenzionale dedicata.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-867">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="fe0c7-868">Usando queste definizioni di route, `Url.Action("Index", "Home")` genera il percorso URL `/` con la route `default` per un motivo ben preciso.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-868">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="fe0c7-869">Si potrebbe pensare che i valori di route `{ controller = Home, action = Index }` siano sufficienti per generare un URL usando `blog` e che il risultato sia `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-869">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="fe0c7-870">Le route convenzionali dedicate si basano su un comportamento speciale dei valori predefiniti per i quali non esiste un parametro di route corrispondente che impedisce alla route di essere troppo "greedy" con la generazione degli URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-870">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="fe0c7-871">In questo caso i valori predefiniti sono `{ controller = Blog, action = Article }` e né `controller` né `action` vengono visualizzati come parametri di route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-871">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="fe0c7-872">Quando il routing esegue la generazione di URL, i valori specificati devono corrispondere ai valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-872">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="fe0c7-873">La generazione di URL che usa `blog` avrà esito negativo perché i valori `{ controller = Home, action = Index }` non corrispondono a `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-873">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="fe0c7-874">Il routing quindi esegue il fallback per provare `default`, che ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-874">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="fe0c7-875">Aree</span><span class="sxs-lookup"><span data-stu-id="fe0c7-875">Areas</span></span>

<span data-ttu-id="fe0c7-876">Le [aree](areas.md) sono una funzionalità di MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi di routing separato (per le azioni del controller) e struttura di cartelle (per le visualizzazioni).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-876">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="fe0c7-877">L'uso delle aree consente a un'applicazione di usare più controller con lo stesso nome, purché abbiano *aree* diverse.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-877">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="fe0c7-878">Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area` a `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-878">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="fe0c7-879">Questa sezione illustra il modo in cui il routing interagisce con le aree. Vedere [Aree](areas.md) per informazioni dettagliate sull'uso delle aree con le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-879">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="fe0c7-880">Nell'esempio seguente MVC viene configurato in modo da usare la route convenzionale predefinita e una *route di area* per un'area denominata `Blog`:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-880">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="fe0c7-881">Nella corrispondenza di un percorso URL come `/Manage/Users/AddUser` la prima route produrrà i valori di route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-881">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="fe0c7-882">Il valore di route `area` è generato da un valore predefinito per `area`, infatti la route creata da `MapAreaRoute` equivale alla seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-882">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="fe0c7-883">`MapAreaRoute` crea una route che usa sia un valore predefinito che un vincolo per `area` usando il nome di area specificato, in questo caso `Blog`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-883">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="fe0c7-884">Il valore predefinito assicura che la route generi sempre `{ area = Blog, ... }`, il vincolo richiede il valore `{ area = Blog, ... }` per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-884">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="fe0c7-885">Il routing convenzionale dipende dall'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-885">Conventional routing is order-dependent.</span></span> <span data-ttu-id="fe0c7-886">In generale, le route con aree devono essere posizionate prima delle altre nella tabella di route poiché sono più specifiche rispetto alle route senza un'area.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-886">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="fe0c7-887">Se si usa l'esempio precedente, i valori di route corrispondono all'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="fe0c7-887">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="fe0c7-888">`AreaAttribute` è ciò che denota un controller come parte di un'area, questo controller in particolare si trova nell'area `Blog`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-888">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="fe0c7-889">I controller senza un attributo `[Area]` non sono membri di alcuna area e **non** corrispondono quando il valore di route `area` viene specificato dal routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-889">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="fe0c7-890">Nell'esempio seguente solo il primo controller indicato può corrispondere ai valori di route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-890">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="fe0c7-891">Lo spazio dei nomi di ogni controller viene indicato qui per motivi di completezza, per evitare conflitti di denominazione nei controller e la generazione di errori del compilatore.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-891">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="fe0c7-892">Gli spazi dei nomi delle classi non hanno effetto sul routing di MVC.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-892">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="fe0c7-893">I primi due controller sono membri di aree e corrispondono solo quando viene specificato il relativo nome di area dal valore di route `area`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-893">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="fe0c7-894">Il terzo controller non è un membro di un'area e può corrispondere solo quando non vengono specificati valori per `area` dal routing.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-894">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-895">In termini di corrispondenza con *nessun valore*, l'assenza del valore `area` è come se il valore per `area` fosse Null o la stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-895">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="fe0c7-896">Quando si esegue un'azione all'interno di un'area, il valore di route per `area` sarà disponibile come *valore di ambiente* per il routing da usare per la generazione di URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-896">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="fe0c7-897">Ciò significa che per impostazione predefinita le aree funzionano *temporaneamente* per la generazione di URL, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-897">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="fe0c7-898">Informazioni su IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="fe0c7-898">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="fe0c7-899">Questa sezione offre un'analisi approfondita degli elementi interni del framework e del modo in cui MVC sceglie un'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-899">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="fe0c7-900">Un'applicazione tipica non richiede un oggetto `IActionConstraint` personalizzato</span><span class="sxs-lookup"><span data-stu-id="fe0c7-900">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="fe0c7-901">Probabilmente l'utente ha già usato `IActionConstraint` anche se non ha familiarità con l'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-901">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="fe0c7-902">L'attributo `[HttpGet]` e gli attributi `[Http-VERB]` dello stesso tipo implementano `IActionConstraint` per limitare l'esecuzione di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-902">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="fe0c7-903">Se si usa la route convenzionale predefinita, il percorso URL `/Products/Edit` produce i valori `{ controller = Products, action = Edit }`, che corrispondono a **entrambe** le azioni illustrate qui.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-903">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="fe0c7-904">Nella terminologia di `IActionConstraint` si dice che entrambe le azioni sono considerate candidati poiché corrispondono entrambe ai dati della route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-904">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="fe0c7-905">Quando viene eseguito `HttpGetAttribute` indica che *Edit()* è una corrispondenza per *GET* e non è una corrispondenza per qualsiasi altro verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-905">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="fe0c7-906">L'azione `Edit(...)` non ha tutti i vincoli definiti e quindi corrisponde a qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-906">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="fe0c7-907">Presupponendo l'uso di un oggetto `POST`, corrisponde solo `Edit(...)`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-907">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="fe0c7-908">Per un oggetto `GET` possono invece corrispondere entrambe le azioni, tuttavia un'azione con `IActionConstraint` viene sempre considerata *migliore* rispetto a un'azione senza tale oggetto.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-908">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="fe0c7-909">Quindi, dal momento che `Edit()` ha `[HttpGet]` è considerata più specifica e verrà selezionata se entrambe le azioni possono corrispondere.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-909">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="fe0c7-910">Concettualmente, `IActionConstraint` è una forma di *overload*, ma anziché sostituire i metodi con lo stesso nome, supporta l'overload tra le azioni che corrispondono allo stesso URL.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-910">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="fe0c7-911">Anche il routing con attributi usa `IActionConstraint` e può accadere che azioni di controller diversi vengano entrambe considerate candidati.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-911">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="fe0c7-912">Implementazione di IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="fe0c7-912">Implementing IActionConstraint</span></span>

<span data-ttu-id="fe0c7-913">Il modo più semplice di implementare un oggetto `IActionConstraint` è creare una classe derivata da `System.Attribute` e inserirla nelle azioni e nel controller.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-913">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="fe0c7-914">MVC rileverà automaticamente tutti gli oggetti `IActionConstraint` applicati come attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-914">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="fe0c7-915">È possibile usare il modello di applicazione per applicare i vincoli e questo è probabilmente l'approccio più flessibile, poiché consente di metaprogrammare le modalità di applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-915">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="fe0c7-916">Nell'esempio seguente un vincolo sceglie un'azione in base a un *codice paese* dei dati della route.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-916">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="fe0c7-917">Vedere l'[esempio completo su GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="fe0c7-917">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="fe0c7-918">L'utente è responsabile dell'implementazione del metodo `Accept` e della scelta di un "ordine" per il vincolo da eseguire.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-918">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="fe0c7-919">In questo caso il metodo `Accept` restituisce `true` per indicare che l'azione è una corrispondenza quando il valore di route `country` corrisponde.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-919">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="fe0c7-920">Questo comportamento è diverso da quello di `RouteValueAttribute` poiché viene consentito il fallback a un'azione senza attributi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-920">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="fe0c7-921">L'esempio indica che se si definisce un'azione `en-US`, per il codice paese, ad esempio `fr-FR`, viene eseguito il fallback a un controller più generico a cui non è applicato `[CountrySpecific(...)]`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-921">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="fe0c7-922">La proprietà `Order` decide a quale *fase* appartiene il vincolo.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-922">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="fe0c7-923">I vincoli relativi alle azioni vengono eseguiti in gruppi basati su `Order`.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-923">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="fe0c7-924">Ad esempio, tutti gli attributi del metodo HTTP specificati dal framework usano lo stesso valore `Order` in modo da essere eseguiti nella stessa fase.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-924">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="fe0c7-925">È possibile usare un numero illimitato di fasi per implementare i criteri necessari.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-925">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="fe0c7-926">Quando si sceglie un valore per `Order` considerare se il vincolo dovrà essere applicato o meno prima dei metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-926">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="fe0c7-927">I numeri più bassi vengono eseguiti per primi.</span><span class="sxs-lookup"><span data-stu-id="fe0c7-927">Lower numbers run first.</span></span>

::: moniker-end
