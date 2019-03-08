---
title: Convenzioni di route e app per Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come le convenzioni di route e del provider di modello di app consentono di controllare routing, individuazione ed elaborazione delle pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c160d93e22fc5b3511ba4e5539cce8576346898b
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665544"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="b2d0e-103">Convenzioni di route e app per Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2d0e-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="b2d0e-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b2d0e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b2d0e-105">Informazioni su come usare le [convenzioni di route e del provider di modello di app](xref:mvc/controllers/application-model#conventions) della pagina per controllare routing, individuazione ed elaborazione nelle app Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="b2d0e-106">Quando è necessario configurare la route di una pagina personalizzata per le singole pagine, configurare il routing alle pagine con la [convenzione AddPageRoute](#configure-a-page-route) descritta più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="b2d0e-107">Per specificare una route di pagina, aggiungere segmenti di route o aggiungere parametri a una route, usare la pagina `@page` direttiva.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="b2d0e-108">Per altre informazioni, vedere [route personalizzata](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="b2d0e-109">Sono presenti parole riservate non possono essere usati come segmenti di route o i nomi dei parametri.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="b2d0e-110">Per altre informazioni, vedere [Routing: Routing nomi riservati](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="b2d0e-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2d0e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="b2d0e-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="b2d0e-112">Scenario</span></span> | <span data-ttu-id="b2d0e-113">L'esempio illustra come eseguire le seguenti operazioni:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="b2d0e-114">Convenzioni del modello</span><span class="sxs-lookup"><span data-stu-id="b2d0e-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="b2d0e-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="b2d0e-115">Conventions.Add</span></span><ul><li><span data-ttu-id="b2d0e-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="b2d0e-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="b2d0e-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="b2d0e-119">Aggiungere un modello e un'intestazione di route alle pagine di un'app.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="b2d0e-120">Convenzioni per le azioni di route di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="b2d0e-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="b2d0e-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="b2d0e-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="b2d0e-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="b2d0e-124">Aggiungere un modello di route alle pagine in una cartella e a una pagina singola.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="b2d0e-125">Convenzioni per le azioni del modello di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="b2d0e-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="b2d0e-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="b2d0e-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="b2d0e-128">ConfigureFilter (classe di filtro, espressione lambda o factory di filtro)</span><span class="sxs-lookup"><span data-stu-id="b2d0e-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="b2d0e-129">Aggiungere un'intestazione alle pagine in una cartella, aggiungere un'intestazione a una pagina singola e configurare una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione alle pagine di un'app.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="b2d0e-130">Convenzioni di pagine Razor vengono aggiunti e configurati tramite il <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> nella raccolta di servizio nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="b2d0e-131">Gli esempi di convenzione seguenti sono illustrati più avanti in questo argomento:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="b2d0e-132">Ordine della route</span><span class="sxs-lookup"><span data-stu-id="b2d0e-132">Route order</span></span>

<span data-ttu-id="b2d0e-133">Le route specificano un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per l'elaborazione (route corrispondente).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="b2d0e-134">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="b2d0e-134">Order</span></span>            | <span data-ttu-id="b2d0e-135">Comportamento</span><span class="sxs-lookup"><span data-stu-id="b2d0e-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="b2d0e-136">-1</span><span class="sxs-lookup"><span data-stu-id="b2d0e-136">-1</span></span>               | <span data-ttu-id="b2d0e-137">La route viene elaborata prima che vengano elaborate altre route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="b2d0e-138">0</span><span class="sxs-lookup"><span data-stu-id="b2d0e-138">0</span></span>                | <span data-ttu-id="b2d0e-139">Non è specificato l'ordine (valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-139">Order isn't specified (default value).</span></span> <span data-ttu-id="b2d0e-140">Non assegnare `Order` (`Order = null`) per impostazione predefinita la route `Order` su 0 (zero) per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="b2d0e-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="b2d0e-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="b2d0e-142">Specifica l'ordine di elaborazione delle route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="b2d0e-143">Per convenzione, viene stabilita l'elaborazione delle route:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="b2d0e-144">Le route vengono elaborate in ordine sequenziale (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="b2d0e-145">Quando le route hanno gli stessi `Order`, più route specifica esiste una corrispondenza per prime, seguite dalle route meno specifiche.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="b2d0e-146">Quando le route con lo stesso `Order` e lo stesso numero di parametri corrisponde un URL della richiesta, le route vengono elaborate nell'ordine in cui vengono aggiunti al <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="b2d0e-147">Evitare se possibile, a seconda di un ordine di elaborazione delle route stabilita.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="b2d0e-148">In genere, il routing seleziona la route corretta con un URL corrispondente.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="b2d0e-149">Se è necessario impostare route `Order` le proprietà per instradare le richieste correttamente, lo schema di routing dell'app è probabilmente creare confusione ai client e fragile mantenere.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="b2d0e-150">Cercare di semplificare lo schema di routing dell'app.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="b2d0e-151">L'app di esempio richiede una route esplicita l'elaborazione dell'ordine per illustrare diversi scenari di routing usando una singola app.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="b2d0e-152">Tuttavia, è consigliabile tentare di evitare la pratica di route impostazione `Order` nelle app di produzione.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="b2d0e-153">Il routing di Razor Pages e il routing del controller MVC condividono un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="b2d0e-154">Informazioni sull'ordine della route negli argomenti di MVC sono disponibile all'indirizzo [Routing ad azioni del controller: Ordinamento delle route con attributi](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="b2d0e-155">Convenzioni del modello</span><span class="sxs-lookup"><span data-stu-id="b2d0e-155">Model conventions</span></span>

<span data-ttu-id="b2d0e-156">Aggiungere un delegato per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> per aggiungere [modellare convenzioni](xref:mvc/controllers/application-model#conventions) che si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="b2d0e-157">Aggiungere una convenzione del modello di route a tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="b2d0e-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="b2d0e-158">Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> costruzione del modello istanze che vengono applicate durante la route di pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="b2d0e-159">L'app di esempio aggiunge un modello di route `{globalTemplate?}` a tutte le pagine nell'app:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="b2d0e-160">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `1`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="b2d0e-161">In questo modo la route seguente comportamento nell'app di esempio di corrispondenza:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="b2d0e-162">Un modello di route per `TheContactPage/{text?}` viene aggiunto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="b2d0e-163">La route di pagina di contatto dispone di un ordine predefinito `null` (`Order = 0`), in modo che corrisponda alla prima il `{globalTemplate?}` del modello di route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="b2d0e-164">Un `{aboutTemplate?}` del modello di route viene aggiunto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="b2d0e-165">Al modello `{aboutTemplate?}` viene assegnato un `Order` di `2`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="b2d0e-166">Quando viene richiesta la pagina di informazioni in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non in `RouteData.Values["aboutTemplate"]` (`Order = 2`) a causa dell'impostazione della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="b2d0e-167">Un `{otherPagesTemplate?}` del modello di route viene aggiunto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="b2d0e-168">Al modello `{otherPagesTemplate?}` viene assegnato un `Order` di `2`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="b2d0e-169">Quando una o più pagine nel *Pages/OtherPages* cartella viene richiesta con un parametro di route (ad esempio, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="b2d0e-170">Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="b2d0e-171">Si basano sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="b2d0e-172">Opzioni di pagine Razor, ad esempio l'aggiunta <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, vengono aggiunti quando MVC viene aggiunto alla raccolta di servizio nel `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b2d0e-173">Per un esempio completo, vedere [l'app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-173">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="b2d0e-174">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![La pagina di informazioni viene richiesta con un segmento di route di GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="b2d0e-177">Aggiungere una convenzione del modello di app per tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="b2d0e-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="b2d0e-178">Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze che vengono applicate durante la costruzione del modello di app di pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="b2d0e-179">Per dimostrare questa e altre convenzioni più avanti in questo argomento, l'app di esempio include una classe `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="b2d0e-180">Il costruttore della classe accetta una stringa `name` e una matrice di stringhe `values`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="b2d0e-181">Questi valori vengono usati nel relativo metodo `OnResultExecuting` per impostare un'intestazione di risposta.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="b2d0e-182">La classe completa è illustrata nella sezione [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="b2d0e-183">L'app di esempio usa la classe `AddHeaderAttribute` per aggiungere un'intestazione, `GlobalHeader`, a tutte le pagine nell'app:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="b2d0e-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="b2d0e-185">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="b2d0e-187">Aggiungere una convenzione del modello di gestore a tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="b2d0e-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="b2d0e-188">Uso <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze che vengono applicate durante la costruzione del modello di gestore di pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="b2d0e-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="b2d0e-190">Convenzioni per le azioni di route di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-190">Page route action conventions</span></span>

<span data-ttu-id="b2d0e-191">Il provider di modello di route predefinito che deriva da <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione delle route di pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="b2d0e-192">Convenzione del modello di route cartella</span><span class="sxs-lookup"><span data-stu-id="b2d0e-192">Folder route model convention</span></span>

<span data-ttu-id="b2d0e-193">Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per tutte le pagine nella cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="b2d0e-194">L'app di esempio usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per aggiungere un modello di route `{otherPagesTemplate?}` alle pagine nella cartella *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="b2d0e-195">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="b2d0e-196">Ciò garantisce che il modello per `{globalTemplate?}` (in precedenza nell'argomento consentono di impostare `1`) viene assegnata la priorità per i dati della route prima posizione del valore quando viene specificato un valore singolo di route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="b2d0e-197">Se una pagina nel *Pages/OtherPages* cartella viene richiesta con un valore di parametro di route (ad esempio, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="b2d0e-198">Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="b2d0e-199">Si basano sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="b2d0e-200">Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![La pagina Page1 nella cartella OtherPages viene richiesta con un segmento di route di GlobalRouteValue e OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="b2d0e-203">Convenzione del modello di route pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-203">Page route model convention</span></span>

<span data-ttu-id="b2d0e-204">Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per la pagina con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="b2d0e-205">L'app di esempio usa `AddPageRouteModelConvention` per aggiungere un modello di route `{aboutTemplate?}` alla pagina di informazioni:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="b2d0e-206">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="b2d0e-207">Ciò garantisce che il modello per `{globalTemplate?}` (in precedenza nell'argomento consentono di impostare `1`) viene assegnata la priorità per i dati della route prima posizione del valore quando viene specificato un valore singolo di route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="b2d0e-208">Se la pagina di informazioni viene richiesta con un valore di parametro di route in corrispondenza `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non `RouteData.Values["aboutTemplate"]` (`Order = 2`) a causa dell'impostazione di `Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="b2d0e-209">Laddove possibile, non vengono impostate le `Order`, in modo da `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="b2d0e-210">Si basano sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="b2d0e-211">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![La pagina di informazioni viene richiesta con segmenti di route per GlobalRouteValue e AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="b2d0e-214">Usare un trasformatore di parametri per personalizzare le route di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="b2d0e-215">Le route di pagina generate da ASP.NET Core possono essere personalizzate usando un trasformatore di parametro.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="b2d0e-216">Un trasformatore di parametri implementa `IOutboundParameterTransformer` e trasforma il valore dei parametri.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="b2d0e-217">Ad esempio, un trasformatore di parametri `SlugifyParameterTransformer` personalizzato cambia il valore di route `SubscriptionManagement` in `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="b2d0e-218">Il `PageRouteTransformerConvention` convenzione del modello di route pagina un trasformatore parametro si applica ai segmenti di nome file e cartelle delle route di pagina generata automaticamente in un'app.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="b2d0e-219">Ad esempio, le pagine Razor file alla */Pages/SubscriptionManagement/ViewAll.cshtml* avrebbe relativa route riscrivere `/SubscriptionManagement/ViewAll` a `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="b2d0e-220">`PageRouteTransformerConvention` Trasforma solo segmenti generati automaticamente di una route di pagina provenienti dalla cartella di Razor Pages e nome file.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="b2d0e-221">Non trasforma i segmenti di route aggiunti con il `@page` direttiva.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="b2d0e-222">La convenzione anche non trasforma route aggiunte da <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="b2d0e-223">Il `PageRouteTransformerConvention` viene registrata come un'opzione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add(
                    new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a><span data-ttu-id="b2d0e-224">Configurare una route di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-224">Configure a page route</span></span>

<span data-ttu-id="b2d0e-225">Usare <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> per configurare una route a una pagina nel percorso di pagina specificato.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="b2d0e-226">I collegamenti generati alla pagina usano la route specificata.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="b2d0e-227">`AddPageRoute` usa `AddPageRouteModelConvention` per stabilire la route.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="b2d0e-228">L'app di esempio crea una route a `/TheContactPage` per *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="b2d0e-229">La pagina di contatto può anche essere raggiunta in corrispondenza di `/Contact` tramite la route predefinita.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="b2d0e-230">La route personalizzata dell'app di esempio alla pagina di contatto consente un segmento di route `text` facoltativo (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="b2d0e-231">La pagina include anche tale segmento facoltativo nella relativa istruzione `@page` nel caso in cui il visitatore acceda alla pagina in corrispondenza della relativa route `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="b2d0e-232">Si noti che l'URL generato per il collegamento **Contatto** nella pagina sottoposta a rendering riflette la route aggiornata:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Collegamento alla pagina di contatto dell'app di esempio nella barra di spostamento](razor-pages-conventions/_static/contact-link.png)

![L'esame del collegamento alla pagina di contatto nell'HTML sottoposto a rendering indica che href è impostato su '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="b2d0e-235">Visitare la pagina di contatto nella route normale, `/Contact`, o nella route personalizzata `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="b2d0e-236">Se si specifica un segmento di route `text` aggiuntivo, la pagina visualizza il segmento codificato in formato HTML specificato dall'utente:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Esempio di browser Microsoft Edge in cui viene specificato un segmento di route facoltativo 'text' corrispondente a 'TextValue' nell'URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="b2d0e-239">Convenzioni per le azioni del modello di pagina</span><span class="sxs-lookup"><span data-stu-id="b2d0e-239">Page model action conventions</span></span>

<span data-ttu-id="b2d0e-240">Il provider di modello di pagina predefinito che implementa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> chiama le convenzioni progettate per offrire punti di estendibilità per la configurazione di modelli di pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="b2d0e-241">Queste convenzioni sono utili durante la compilazione e la modifica degli scenari di individuazione ed elaborazione delle pagine.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="b2d0e-242">Per gli esempi in questa sezione, l'app di esempio Usa un' `AddHeaderAttribute` classe, ovvero un <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, che applica un'intestazione di risposta:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="b2d0e-243">Tramite le convenzioni, nell'esempio viene illustrato come applicare l'attributo a tutte le pagine in una cartella e a una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="b2d0e-244">**Convenzione per il modello di app cartella**</span><span class="sxs-lookup"><span data-stu-id="b2d0e-244">**Folder app model convention**</span></span>

<span data-ttu-id="b2d0e-245">Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> che chiama un'azione su <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> istanze per tutte le pagine nella cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="b2d0e-246">Nell'esempio viene illustrato l'uso di `AddFolderApplicationModelConvention` aggiungendo un'intestazione, `OtherPagesHeader`, alle pagine all'interno della cartella *OtherPages* dell'app:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="b2d0e-247">Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina OtherPages/Page1 indicano che è stato aggiunto l'elemento OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="b2d0e-249">**Convenzione per il modello di app cartella**</span><span class="sxs-lookup"><span data-stu-id="b2d0e-249">**Page app model convention**</span></span>

<span data-ttu-id="b2d0e-250">Uso <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> per creare e aggiungere un' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> che chiama un'azione su di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> per la pagina con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="b2d0e-251">Nell'esempio viene illustrato l'uso di `AddPageApplicationModelConvention` aggiungendo un'intestazione, `AboutHeader`, alla pagina di informazioni:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="b2d0e-252">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="b2d0e-254">**Configurare un filtro**</span><span class="sxs-lookup"><span data-stu-id="b2d0e-254">**Configure a filter**</span></span>

<span data-ttu-id="b2d0e-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Consente di configurare il filtro specificato da applicare.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="b2d0e-256">È possibile implementare una classe di filtro, ma l'app di esempio illustra come implementare un filtro in un'espressione lambda, che viene implementata in background come una factory che restituisce un filtro:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="b2d0e-257">Viene usato il modello di app della pagina per verificare il percorso relativo per i segmenti che portano alla pagina Page2 nella cartella *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="b2d0e-258">Se la condizione viene superata, viene aggiunta un'intestazione.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="b2d0e-259">In caso contrario, viene applicato `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="b2d0e-260">`EmptyFilter` è un [filtro azione](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="b2d0e-261">Poiché i filtri azione vengono ignorati da Razor Pages, `EmptyFilter` non esegue nessuna operazione come previsto se il percorso non contiene `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="b2d0e-262">Richiedere la pagina Page2 dell'esempio in `localhost:5000/OtherPages/Page2` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![L'elemento OtherPagesPage2Header viene aggiunto alla risposta per Page2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="b2d0e-264">**Configurare una factory di filtro**</span><span class="sxs-lookup"><span data-stu-id="b2d0e-264">**Configure a filter factory**</span></span>

<span data-ttu-id="b2d0e-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Configura la factory specificata per applicare [filtri](xref:mvc/controllers/filters) a tutte le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="b2d0e-266">L'app di esempio illustra un esempio dell'uso di una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) aggiungendo un'intestazione, `FilterFactoryHeader`, con due valori per le pagine dell'app:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="b2d0e-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="b2d0e-268">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="b2d0e-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina informazioni indicano che sono state aggiunte due intestazioni FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="b2d0e-270">Filtri MVC e il filtro di pagina (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="b2d0e-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="b2d0e-271">I [filtri azione](xref:mvc/controllers/filters#action-filters) MVC vengono ignorati da Razor Pages, poiché vengono usati i metodi del gestore.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="b2d0e-272">Sono disponibili è possibile utilizzare altri tipi di filtri MVC: [Authorization](xref:mvc/controllers/filters#authorization-filters), [eccezioni](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), e [risultato](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="b2d0e-273">Per altre informazioni, vedere l'argomento [Filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="b2d0e-274">Il filtro di pagina (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) è un filtro che si applica a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b2d0e-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="b2d0e-275">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="b2d0e-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2d0e-276">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b2d0e-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
