---
title: Convenzioni di route e app per Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come le convenzioni di route e del provider di modello di app consentono di controllare routing, individuazione ed elaborazione delle pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c93f169c422d260f738faba4812861521f383e51
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914982"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="0a81a-103">Convenzioni di route e app per Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a81a-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="0a81a-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a81a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a81a-105">Informazioni su come usare le [convenzioni di route e del provider di modello di app](xref:mvc/controllers/application-model#conventions) della pagina per controllare routing, individuazione ed elaborazione nelle app Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0a81a-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="0a81a-106">Quando è necessario configurare la route di una pagina personalizzata per le singole pagine, configurare il routing alle pagine con la [convenzione AddPageRoute](#configure-a-page-route) descritta più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="0a81a-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="0a81a-107">Per specificare una route di pagina, aggiungere segmenti di route o aggiungere parametri a una route, usare la `@page` direttiva della pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="0a81a-108">Per altre informazioni, vedere [route personalizzate](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="0a81a-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="0a81a-109">Sono disponibili parole riservate che non possono essere usate come segmenti di route o nomi di parametro.</span><span class="sxs-lookup"><span data-stu-id="0a81a-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="0a81a-110">Per ulteriori informazioni, vedere [routing: Nomi](xref:fundamentals/routing#reserved-routing-names)di routing riservati.</span><span class="sxs-lookup"><span data-stu-id="0a81a-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="0a81a-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a81a-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="0a81a-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="0a81a-112">Scenario</span></span> | <span data-ttu-id="0a81a-113">L'esempio illustra come eseguire le seguenti operazioni:</span><span class="sxs-lookup"><span data-stu-id="0a81a-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="0a81a-114">Convenzioni del modello</span><span class="sxs-lookup"><span data-stu-id="0a81a-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="0a81a-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="0a81a-115">Conventions.Add</span></span><ul><li><span data-ttu-id="0a81a-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="0a81a-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0a81a-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="0a81a-119">Aggiungere un modello e un'intestazione di route alle pagine di un'app.</span><span class="sxs-lookup"><span data-stu-id="0a81a-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="0a81a-120">Convenzioni per le azioni di route di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="0a81a-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="0a81a-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="0a81a-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="0a81a-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="0a81a-124">Aggiungere un modello di route alle pagine in una cartella e a una pagina singola.</span><span class="sxs-lookup"><span data-stu-id="0a81a-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="0a81a-125">Convenzioni per le azioni del modello di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="0a81a-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="0a81a-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0a81a-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0a81a-128">ConfigureFilter (classe di filtro, espressione lambda o factory di filtro)</span><span class="sxs-lookup"><span data-stu-id="0a81a-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="0a81a-129">Aggiungere un'intestazione alle pagine in una cartella, aggiungere un'intestazione a una pagina singola e configurare una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) per aggiungere un'intestazione alle pagine di un'app.</span><span class="sxs-lookup"><span data-stu-id="0a81a-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="0a81a-130">Le convenzioni Razor Pages vengono aggiunte e configurate mediante <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> il <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> metodo di estensione a nella `Startup` raccolta di servizi nella classe.</span><span class="sxs-lookup"><span data-stu-id="0a81a-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="0a81a-131">Gli esempi di convenzione seguenti sono illustrati più avanti in questo argomento:</span><span class="sxs-lookup"><span data-stu-id="0a81a-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention(
                    "/About", model => { ... });
                options.Conventions.AddPageRoute(
                    "/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention(
                    "/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="0a81a-132">Ordine Route</span><span class="sxs-lookup"><span data-stu-id="0a81a-132">Route order</span></span>

<span data-ttu-id="0a81a-133">Le route specificano un oggetto per l' <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> elaborazione (corrispondenza della route).</span><span class="sxs-lookup"><span data-stu-id="0a81a-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="0a81a-134">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="0a81a-134">Order</span></span>            | <span data-ttu-id="0a81a-135">Comportamento</span><span class="sxs-lookup"><span data-stu-id="0a81a-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="0a81a-136">-1</span><span class="sxs-lookup"><span data-stu-id="0a81a-136">-1</span></span>               | <span data-ttu-id="0a81a-137">La route viene elaborata prima dell'elaborazione di altre route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="0a81a-138">0</span><span class="sxs-lookup"><span data-stu-id="0a81a-138">0</span></span>                | <span data-ttu-id="0a81a-139">L'ordine non è specificato (valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="0a81a-139">Order isn't specified (default value).</span></span> <span data-ttu-id="0a81a-140">Non assegnando `Order` (`Order = null`) il percorso `Order` viene impostato su 0 (zero) per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="0a81a-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="0a81a-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="0a81a-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="0a81a-142">Specifica l'ordine di elaborazione della route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="0a81a-143">L'elaborazione delle route viene stabilita per convenzione:</span><span class="sxs-lookup"><span data-stu-id="0a81a-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="0a81a-144">Le route vengono elaborate in ordine sequenziale (-1, 0, 1 &hellip; , 2, n).</span><span class="sxs-lookup"><span data-stu-id="0a81a-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="0a81a-145">Quando le route hanno lo `Order`stesso, la route più specifica viene confrontata per prima, seguita da route meno specifiche.</span><span class="sxs-lookup"><span data-stu-id="0a81a-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="0a81a-146">Quando le route con lo `Order` stesso numero di parametri corrispondono a un URL di richiesta, le route vengono elaborate nell'ordine in cui sono state aggiunte <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>a.</span><span class="sxs-lookup"><span data-stu-id="0a81a-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="0a81a-147">Se possibile, evitare di basarsi su un ordine di elaborazione di Route definito.</span><span class="sxs-lookup"><span data-stu-id="0a81a-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="0a81a-148">In genere, il routing seleziona la route corretta con l'URL corrispondente.</span><span class="sxs-lookup"><span data-stu-id="0a81a-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="0a81a-149">Se è necessario impostare le `Order` proprietà della route in modo che le richieste vengano indirizzate correttamente, lo schema di routing dell'app è probabilmente confuso per i client ed è fragile da mantenere.</span><span class="sxs-lookup"><span data-stu-id="0a81a-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="0a81a-150">Cercare di semplificare lo schema di routing dell'app.</span><span class="sxs-lookup"><span data-stu-id="0a81a-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="0a81a-151">L'app di esempio richiede un ordine esplicito di elaborazione della route per illustrare diversi scenari di routing usando una singola app.</span><span class="sxs-lookup"><span data-stu-id="0a81a-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="0a81a-152">Tuttavia, è consigliabile provare a evitare la procedura di impostazione della `Order` route nelle app di produzione.</span><span class="sxs-lookup"><span data-stu-id="0a81a-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="0a81a-153">Il routing di Razor Pages e il routing del controller MVC condividono un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="0a81a-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="0a81a-154">Le informazioni sull'ordine di route negli argomenti MVC sono disponibili [in routing alle azioni del controller: Ordinamento delle route](xref:mvc/controllers/routing#ordering-attribute-routes)degli attributi.</span><span class="sxs-lookup"><span data-stu-id="0a81a-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="0a81a-155">Convenzioni del modello</span><span class="sxs-lookup"><span data-stu-id="0a81a-155">Model conventions</span></span>

<span data-ttu-id="0a81a-156">Aggiungere un delegato per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> per aggiungere le convenzioni del [modello](xref:mvc/controllers/application-model#conventions) che si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0a81a-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="0a81a-157">Aggiungere una convenzione del modello di route a tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="0a81a-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="0a81a-158">Utilizzare <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oggetto alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze di che vengono applicate durante la costruzione del modello di route della pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="0a81a-159">L'app di esempio aggiunge un modello di route `{globalTemplate?}` a tutte le pagine nell'app:</span><span class="sxs-lookup"><span data-stu-id="0a81a-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-160">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `1`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="0a81a-161">In questo modo si garantisce il comportamento di corrispondenza Route seguente nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="0a81a-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="0a81a-162">Un modello di route `TheContactPage/{text?}` per viene aggiunto più avanti nell'argomento.</span><span class="sxs-lookup"><span data-stu-id="0a81a-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="0a81a-163">La route della pagina di contatto ha un ordine `null` predefinito`Order = 0`di (), quindi corrisponde prima `{globalTemplate?}` del modello di route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="0a81a-164">Un `{aboutTemplate?}` modello di route verrà aggiunto più avanti nell'argomento.</span><span class="sxs-lookup"><span data-stu-id="0a81a-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0a81a-165">Al modello `{aboutTemplate?}` viene assegnato un `Order` di `2`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0a81a-166">Quando viene richiesta la pagina di informazioni in `/About/RouteDataValue`, "RouteDataValue" viene caricato in `RouteData.Values["globalTemplate"]` (`Order = 1`) e non in `RouteData.Values["aboutTemplate"]` (`Order = 2`) a causa dell'impostazione della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="0a81a-167">Un `{otherPagesTemplate?}` modello di route verrà aggiunto più avanti nell'argomento.</span><span class="sxs-lookup"><span data-stu-id="0a81a-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0a81a-168">Al modello `{otherPagesTemplate?}` viene assegnato un `Order` di `2`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0a81a-169">Quando viene richiesta una pagina nella cartella *pages/OtherPages* con un parametro di route (ad esempio `/OtherPages/Page1/RouteDataValue`,), "RouteDataValue" viene caricato `RouteData.Values["globalTemplate"]` in`Order = 1`() e `RouteData.Values["otherPagesTemplate"]` non`Order = 2`() a causa dell'impostazione della Proprietà`Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81a-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0a81a-170">Laddove possibile, non impostare `Order`, che `Order = 0`restituisce.</span><span class="sxs-lookup"><span data-stu-id="0a81a-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0a81a-171">Basarsi sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="0a81a-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0a81a-172">Vengono aggiunte opzioni di Razor Pages, <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>ad esempio l'aggiunta di, quando MVC viene aggiunto alla raccolta `Startup.ConfigureServices`di servizi in.</span><span class="sxs-lookup"><span data-stu-id="0a81a-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0a81a-173">Per un esempio completo, vedere [l'app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="0a81a-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-174">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![La pagina di informazioni viene richiesta con un segmento di route di GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="0a81a-177">Aggiungere una convenzione del modello di app a tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="0a81a-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="0a81a-178">Usare <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oggetto alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze che vengono applicate durante la costruzione del modello di app di pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="0a81a-179">Per dimostrare questa e altre convenzioni più avanti in questo argomento, l'app di esempio include una classe `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="0a81a-180">Il costruttore della classe accetta una stringa `name` e una matrice di stringhe `values`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="0a81a-181">Questi valori vengono usati nel relativo metodo `OnResultExecuting` per impostare un'intestazione di risposta.</span><span class="sxs-lookup"><span data-stu-id="0a81a-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="0a81a-182">La classe completa è illustrata nella sezione [Convenzioni per le azioni del modello di pagina](#page-model-action-conventions) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="0a81a-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="0a81a-183">L'app di esempio usa la classe `AddHeaderAttribute` per aggiungere un'intestazione, `GlobalHeader`, a tutte le pagine nell'app:</span><span class="sxs-lookup"><span data-stu-id="0a81a-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a81a-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="0a81a-185">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="0a81a-187">Aggiungere una convenzione del modello di gestore a tutte le pagine</span><span class="sxs-lookup"><span data-stu-id="0a81a-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="0a81a-188">Utilizzare <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> oggetto alla raccolta di <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> istanze di che vengono applicate durante la costruzione del modello del gestore di pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a81a-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="0a81a-190">Convenzioni per le azioni di route di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-190">Page route action conventions</span></span>

<span data-ttu-id="0a81a-191">Il provider del modello di route predefinito che deriva <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> da richiama le convenzioni progettate per fornire punti di estendibilità per la configurazione delle route di pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="0a81a-192">Convenzione del modello di route della cartella</span><span class="sxs-lookup"><span data-stu-id="0a81a-192">Folder route model convention</span></span>

<span data-ttu-id="0a81a-193">Utilizzare <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oggetto che richiama un'azione su <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per tutte le pagine nella cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="0a81a-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="0a81a-194">L'app di esempio usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> per aggiungere un modello di route `{otherPagesTemplate?}` alle pagine nella cartella *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="0a81a-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="0a81a-195">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0a81a-196">In questo modo si garantisce che `{globalTemplate?}` al modello per (impostato in precedenza `1`nell'argomento a) venga assegnata la priorità per la prima posizione del valore di dati della route quando viene specificato un singolo valore di route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0a81a-197">Se una pagina nella cartella *pages/OtherPages* è richiesta con un valore di parametro di route (ad `/OtherPages/Page1/RouteDataValue`esempio,), "RouteDataValue" viene `RouteData.Values["globalTemplate"]` caricato`Order = 1`in () `RouteData.Values["otherPagesTemplate"]` e`Order = 2`non () a causa dell'impostazione della Proprietà`Order` proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81a-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0a81a-198">Laddove possibile, non impostare `Order`, che `Order = 0`restituisce.</span><span class="sxs-lookup"><span data-stu-id="0a81a-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0a81a-199">Basarsi sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="0a81a-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0a81a-200">Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![La pagina Page1 nella cartella OtherPages viene richiesta con un segmento di route di GlobalRouteValue e OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="0a81a-203">Convenzione del modello di route della pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-203">Page route model convention</span></span>

<span data-ttu-id="0a81a-204">Utilizzare <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> oggetto che richiama un'azione sull'oggetto <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> per la pagina con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="0a81a-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="0a81a-205">L'app di esempio usa `AddPageRouteModelConvention` per aggiungere un modello di route `{aboutTemplate?}` alla pagina di informazioni:</span><span class="sxs-lookup"><span data-stu-id="0a81a-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="0a81a-206">La proprietà <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> per <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> è impostata su `2`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0a81a-207">In questo modo si garantisce che `{globalTemplate?}` al modello per (impostato in precedenza `1`nell'argomento a) venga assegnata la priorità per la prima posizione del valore di dati della route quando viene specificato un singolo valore di route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0a81a-208">Se la pagina about è richiesta con un valore di parametro di `/About/RouteDataValue`route in, "RouteDataValue" viene `RouteData.Values["globalTemplate"]` caricato`Order = 1`in () `RouteData.Values["aboutTemplate"]` e`Order = 2`non () a causa `Order` dell'impostazione della proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81a-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0a81a-209">Laddove possibile, non impostare `Order`, che `Order = 0`restituisce.</span><span class="sxs-lookup"><span data-stu-id="0a81a-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0a81a-210">Basarsi sul routing per selezionare la route corretta.</span><span class="sxs-lookup"><span data-stu-id="0a81a-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0a81a-211">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About/GlobalRouteValue/AboutRouteValue` ed esaminare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![La pagina di informazioni viene richiesta con segmenti di route per GlobalRouteValue e AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="0a81a-214">Usare un trasformatore di parametri per personalizzare le route di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="0a81a-215">Le route di pagina generate da ASP.NET Core possono essere personalizzate usando un trasformatore di parametro.</span><span class="sxs-lookup"><span data-stu-id="0a81a-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="0a81a-216">Un trasformatore di parametri implementa `IOutboundParameterTransformer` e trasforma il valore dei parametri.</span><span class="sxs-lookup"><span data-stu-id="0a81a-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="0a81a-217">Ad esempio, un trasformatore di parametri `SlugifyParameterTransformer` personalizzato cambia il valore di route `SubscriptionManagement` in `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="0a81a-218">La `PageRouteTransformerConvention` convenzione del modello di route di pagina applica un trasformatore di parametri ai segmenti di cartella e nome file delle route di pagina generate automaticamente in un'app.</span><span class="sxs-lookup"><span data-stu-id="0a81a-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="0a81a-219">Ad esempio, il file Razor Pages in */pages/SubscriptionManagement/ViewAll.cshtml* avrebbe la route riscritta da `/SubscriptionManagement/ViewAll` a. `/subscription-management/view-all`</span><span class="sxs-lookup"><span data-stu-id="0a81a-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="0a81a-220">`PageRouteTransformerConvention`trasforma solo i segmenti generati automaticamente di una route di pagina che provengono dalla Razor Pages cartella e dal nome file.</span><span class="sxs-lookup"><span data-stu-id="0a81a-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="0a81a-221">Non trasforma i segmenti di route aggiunti con `@page` la direttiva.</span><span class="sxs-lookup"><span data-stu-id="0a81a-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="0a81a-222">La convenzione non trasforma inoltre le route aggiunte <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>da.</span><span class="sxs-lookup"><span data-stu-id="0a81a-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="0a81a-223">Il `PageRouteTransformerConvention` è registrato come opzione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0a81a-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="0a81a-224">Configurare una route di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-224">Configure a page route</span></span>

<span data-ttu-id="0a81a-225">Utilizzare <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> per configurare una route a una pagina nel percorso di pagina specificato.</span><span class="sxs-lookup"><span data-stu-id="0a81a-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="0a81a-226">I collegamenti generati alla pagina usano la route specificata.</span><span class="sxs-lookup"><span data-stu-id="0a81a-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="0a81a-227">`AddPageRoute` usa `AddPageRouteModelConvention` per stabilire la route.</span><span class="sxs-lookup"><span data-stu-id="0a81a-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="0a81a-228">L'app di esempio crea una route a `/TheContactPage` per *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a81a-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="0a81a-229">La pagina di contatto può anche essere raggiunta in corrispondenza di `/Contact` tramite la route predefinita.</span><span class="sxs-lookup"><span data-stu-id="0a81a-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="0a81a-230">La route personalizzata dell'app di esempio alla pagina di contatto consente un segmento di route `text` facoltativo (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="0a81a-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="0a81a-231">La pagina include anche tale segmento facoltativo nella relativa istruzione `@page` nel caso in cui il visitatore acceda alla pagina in corrispondenza della relativa route `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="0a81a-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="0a81a-232">Si noti che l'URL generato per il collegamento **Contatto** nella pagina sottoposta a rendering riflette la route aggiornata:</span><span class="sxs-lookup"><span data-stu-id="0a81a-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Collegamento alla pagina di contatto dell'app di esempio nella barra di spostamento](razor-pages-conventions/_static/contact-link.png)

![L'esame del collegamento alla pagina di contatto nell'HTML sottoposto a rendering indica che href è impostato su '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="0a81a-235">Visitare la pagina di contatto nella route normale, `/Contact`, o nella route personalizzata `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="0a81a-236">Se si specifica un segmento di route `text` aggiuntivo, la pagina visualizza il segmento codificato in formato HTML specificato dall'utente:</span><span class="sxs-lookup"><span data-stu-id="0a81a-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Esempio di browser Microsoft Edge in cui viene specificato un segmento di route facoltativo 'text' corrispondente a 'TextValue' nell'URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="0a81a-239">Convenzioni per le azioni del modello di pagina</span><span class="sxs-lookup"><span data-stu-id="0a81a-239">Page model action conventions</span></span>

<span data-ttu-id="0a81a-240">Il provider del modello di pagina predefinito <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> che implementa le convenzioni Invokes progettate per fornire punti di estendibilità per la configurazione dei modelli di pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="0a81a-241">Queste convenzioni sono utili durante la compilazione e la modifica degli scenari di individuazione ed elaborazione delle pagine.</span><span class="sxs-lookup"><span data-stu-id="0a81a-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="0a81a-242">Per gli esempi in questa sezione, l'app di esempio usa `AddHeaderAttribute` una classe, che è <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>un oggetto, che applica un'intestazione di risposta:</span><span class="sxs-lookup"><span data-stu-id="0a81a-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-243">Tramite le convenzioni, nell'esempio viene illustrato come applicare l'attributo a tutte le pagine in una cartella e a una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="0a81a-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="0a81a-244">**Convenzione per il modello di app cartella**</span><span class="sxs-lookup"><span data-stu-id="0a81a-244">**Folder app model convention**</span></span>

<span data-ttu-id="0a81a-245">Utilizzare <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oggetto che richiama un'azione sulle <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> istanze di per tutte le pagine nella cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="0a81a-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="0a81a-246">Nell'esempio viene illustrato l'uso di `AddFolderApplicationModelConvention` aggiungendo un'intestazione, `OtherPagesHeader`, alle pagine all'interno della cartella *OtherPages* dell'app:</span><span class="sxs-lookup"><span data-stu-id="0a81a-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="0a81a-247">Richiedere la pagina Page1 dell'esempio in `localhost:5000/OtherPages/Page1` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina OtherPages/Page1 indicano che è stato aggiunto l'elemento OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="0a81a-249">**Convenzione per il modello di app cartella**</span><span class="sxs-lookup"><span data-stu-id="0a81a-249">**Page app model convention**</span></span>

<span data-ttu-id="0a81a-250">Utilizzare <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> per creare e aggiungere un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> oggetto che richiama un'azione sull'oggetto <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> per la pagina con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="0a81a-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="0a81a-251">Nell'esempio viene illustrato l'uso di `AddPageApplicationModelConvention` aggiungendo un'intestazione, `AboutHeader`, alla pagina di informazioni:</span><span class="sxs-lookup"><span data-stu-id="0a81a-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="0a81a-252">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina di informazioni indicano che è stato aggiunto l'elemento AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="0a81a-254">**Configurare un filtro**</span><span class="sxs-lookup"><span data-stu-id="0a81a-254">**Configure a filter**</span></span>

<span data-ttu-id="0a81a-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>Configura il filtro specificato da applicare.</span><span class="sxs-lookup"><span data-stu-id="0a81a-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="0a81a-256">È possibile implementare una classe di filtro, ma l'app di esempio illustra come implementare un filtro in un'espressione lambda, che viene implementata in background come una factory che restituisce un filtro:</span><span class="sxs-lookup"><span data-stu-id="0a81a-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="0a81a-257">Viene usato il modello di app della pagina per verificare il percorso relativo per i segmenti che portano alla pagina Page2 nella cartella *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="0a81a-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="0a81a-258">Se la condizione viene superata, viene aggiunta un'intestazione.</span><span class="sxs-lookup"><span data-stu-id="0a81a-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="0a81a-259">In caso contrario, viene applicato `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="0a81a-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="0a81a-260">`EmptyFilter` è un [filtro azione](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="0a81a-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="0a81a-261">Poiché i filtri azione vengono ignorati da Razor pages `EmptyFilter` , non ha alcun effetto come previsto se il percorso `OtherPages/Page2`non contiene.</span><span class="sxs-lookup"><span data-stu-id="0a81a-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="0a81a-262">Richiedere la pagina Page2 dell'esempio in `localhost:5000/OtherPages/Page2` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![L'elemento OtherPagesPage2Header viene aggiunto alla risposta per Page2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="0a81a-264">**Configurare una factory di filtro**</span><span class="sxs-lookup"><span data-stu-id="0a81a-264">**Configure a filter factory**</span></span>

<span data-ttu-id="0a81a-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>Configura la factory specificata per applicare i [filtri](xref:mvc/controllers/filters) a tutti i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0a81a-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="0a81a-266">L'app di esempio illustra un esempio dell'uso di una [factory di filtro](xref:mvc/controllers/filters#ifilterfactory) aggiungendo un'intestazione, `FilterFactoryHeader`, con due valori per le pagine dell'app:</span><span class="sxs-lookup"><span data-stu-id="0a81a-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="0a81a-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a81a-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a81a-268">Richiedere la pagina di informazioni dell'esempio in `localhost:5000/About` ed esaminare le intestazioni per visualizzare il risultato:</span><span class="sxs-lookup"><span data-stu-id="0a81a-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Le intestazioni di risposta della pagina informazioni indicano che sono state aggiunte due intestazioni FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="0a81a-270">Filtri MVC e il filtro di pagina (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="0a81a-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="0a81a-271">I [filtri azione](xref:mvc/controllers/filters#action-filters) MVC vengono ignorati da Razor Pages, poiché vengono usati i metodi del gestore.</span><span class="sxs-lookup"><span data-stu-id="0a81a-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="0a81a-272">Altri tipi di filtri MVC sono disponibili per l'uso: [Autorizzazione](xref:mvc/controllers/filters#authorization-filters), [eccezione](xref:mvc/controllers/filters#exception-filters), [risorsa](xref:mvc/controllers/filters#resource-filters)e [risultato](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="0a81a-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="0a81a-273">Per altre informazioni, vedere l'argomento [Filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="0a81a-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="0a81a-274">Il filtro di pagina<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>() è un filtro che si applica a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0a81a-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="0a81a-275">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0a81a-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a81a-276">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0a81a-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
