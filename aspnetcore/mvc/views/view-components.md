---
title: Visualizza i componenti
author: rick-anderson
description: Visualizza i componenti sono destinati in qualsiasi punto che si dispone di logica di rendering riutilizzabili.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 65074ca02a1365db278d348d4e024121a6eb4634
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="view-components"></a><span data-ttu-id="f5788-103">Visualizza i componenti</span><span class="sxs-lookup"><span data-stu-id="f5788-103">View components</span></span>

<span data-ttu-id="f5788-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f5788-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f5788-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5788-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="f5788-106">Introduzione a componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-106">Introducing view components</span></span>

<span data-ttu-id="f5788-107">Nuovo ad ASP.NET MVC di base, i componenti di visualizzazione sono simili alle viste parziale, ma molto più potenti.</span><span class="sxs-lookup"><span data-stu-id="f5788-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="f5788-108">Visualizza i componenti non utilizzare l'associazione di modelli e dipendono solo i dati forniti durante la chiamata al suo interno.</span><span class="sxs-lookup"><span data-stu-id="f5788-108">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="f5788-109">Un componente di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="f5788-109">A view component:</span></span>

* <span data-ttu-id="f5788-110">Esegue il rendering di un blocco anziché un'intera risposta.</span><span class="sxs-lookup"><span data-stu-id="f5788-110">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="f5788-111">Include gli stesso la separazione di problemi e i vantaggi di testabilità trovati tra un controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-111">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="f5788-112">Può avere parametri e logica di business</span><span class="sxs-lookup"><span data-stu-id="f5788-112">Can have parameters and business logic</span></span>
* <span data-ttu-id="f5788-113">In genere viene richiamato da una pagina di layout</span><span class="sxs-lookup"><span data-stu-id="f5788-113">Is typically invoked from a layout page</span></span>

<span data-ttu-id="f5788-114">Visualizza i componenti sono destinati in qualsiasi punto che si dispone di logica di rendering riutilizzabile che è troppo complessa per una visualizzazione parziale, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f5788-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="f5788-115">Menu di navigazione dinamica</span><span class="sxs-lookup"><span data-stu-id="f5788-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="f5788-116">Cloud tag (in cui viene eseguita una query del database)</span><span class="sxs-lookup"><span data-stu-id="f5788-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="f5788-117">Pannello di accesso</span><span class="sxs-lookup"><span data-stu-id="f5788-117">Login panel</span></span>
* <span data-ttu-id="f5788-118">Carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="f5788-118">Shopping cart</span></span>
* <span data-ttu-id="f5788-119">Articoli pubblicati di recente</span><span class="sxs-lookup"><span data-stu-id="f5788-119">Recently published articles</span></span>
* <span data-ttu-id="f5788-120">Contenuto dell'intestazione laterale in un tipico blog</span><span class="sxs-lookup"><span data-stu-id="f5788-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="f5788-121">Un pannello di accesso che verrebbe eseguito in ogni pagina e visualizza entrambi i collegamenti per effettuare la disconnessione o di log, a seconda di log nello stato dell'utente</span><span class="sxs-lookup"><span data-stu-id="f5788-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="f5788-122">Un componente di visualizzazione è costituito da due parti: la classe (in genere derivato da [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) e il risultato restituisce (in genere una vista).</span><span class="sxs-lookup"><span data-stu-id="f5788-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="f5788-123">Ad esempio controller, un componente di visualizzazione può essere un POCO, ma desidera sfruttare i metodi e le proprietà disponibili derivandolo dalla maggior parte degli sviluppatori `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="f5788-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="f5788-124">Creazione di un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-124">Creating a view component</span></span>

<span data-ttu-id="f5788-125">In questa sezione contiene i requisiti di alto livello per creare un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="f5788-126">In un secondo momento nell'articolo, viene esaminare in dettaglio ogni passaggio e creare un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="f5788-127">La classe di componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-127">The view component class</span></span>

<span data-ttu-id="f5788-128">È possibile creare una classe di componente di visualizzazione da una delle seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5788-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="f5788-129">Derivazione da *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="f5788-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="f5788-130">La decorazione di una classe con il `[ViewComponent]` attributo o deriva da una classe con il `[ViewComponent]` attributo</span><span class="sxs-lookup"><span data-stu-id="f5788-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="f5788-131">Creazione di una classe il cui nome termina con il suffisso *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="f5788-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="f5788-132">Ad esempio controller, Visualizza i componenti devono essere classi di pubbliche, non annidata e non astratta.</span><span class="sxs-lookup"><span data-stu-id="f5788-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="f5788-133">Il nome del componente di visualizzazione è il nome di classe con il suffisso "ViewComponent" rimosso.</span><span class="sxs-lookup"><span data-stu-id="f5788-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="f5788-134">È possibile inoltre specificare in modo esplicito utilizzando il `ViewComponentAttribute.Name` proprietà.</span><span class="sxs-lookup"><span data-stu-id="f5788-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="f5788-135">Una classe di componente di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="f5788-135">A view component class:</span></span>

* <span data-ttu-id="f5788-136">Supporta completamente costruttore [inserimento di dipendenze](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="f5788-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="f5788-137">Non prendere parte del ciclo di vita di controller, ovvero non è possibile utilizzare [filtri](../controllers/filters.md) in un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="f5788-138">Metodi del componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-138">View component methods</span></span>

<span data-ttu-id="f5788-139">Un componente di visualizzazione definisce la logica in un `InvokeAsync` metodo che restituisce un `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="f5788-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="f5788-140">I parametri che provengano direttamente dalla chiamata di un componente di visualizzazione, non dall'associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="f5788-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="f5788-141">Un componente di visualizzazione mai direttamente gestisce una richiesta.</span><span class="sxs-lookup"><span data-stu-id="f5788-141">A view component never directly handles a request.</span></span> <span data-ttu-id="f5788-142">In genere, Inizializza un modello e la passa a una visualizzazione chiamando un componente di visualizzazione di `View` metodo.</span><span class="sxs-lookup"><span data-stu-id="f5788-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="f5788-143">In sintesi, visualizzare i metodi del componente:</span><span class="sxs-lookup"><span data-stu-id="f5788-143">In summary, view component methods:</span></span>

* <span data-ttu-id="f5788-144">Definire un `InvokeAsync` metodo che restituisce un`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="f5788-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="f5788-145">Inizializza un modello e la passa a una vista mediante la chiamata in genere il `ViewComponent` `View` (metodo)</span><span class="sxs-lookup"><span data-stu-id="f5788-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="f5788-146">Parametri provengono dal metodo di chiamata, non HTTP, non è disponibile alcuna associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="f5788-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="f5788-147">Sono non raggiungibile direttamente come un endpoint HTTP, che sta richiamati dal codice (in genere in una vista).</span><span class="sxs-lookup"><span data-stu-id="f5788-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="f5788-148">Un componente di visualizzazione mai gestisce una richiesta</span><span class="sxs-lookup"><span data-stu-id="f5788-148">A view component never handles a request</span></span>
* <span data-ttu-id="f5788-149">La firma, anziché i dettagli della richiesta HTTP corrente sono sottoposti a overload</span><span class="sxs-lookup"><span data-stu-id="f5788-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="f5788-150">Percorso di ricerca di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-150">View search path</span></span>

<span data-ttu-id="f5788-151">Il runtime esegue la ricerca per la visualizzazione nei percorsi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5788-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="f5788-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="f5788-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="f5788-153">Componenti/Shared/visualizzazioni/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="f5788-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="f5788-154">Il nome di visualizzazione predefinito per un componente di visualizzazione è *predefinito*, ovvero il file di visualizzazione in genere è denominato *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f5788-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="f5788-155">È possibile specificare un nome di visualizzazione diverso quando si crea il risultato di componente di visualizzazione o quando si chiama il `View` metodo.</span><span class="sxs-lookup"><span data-stu-id="f5788-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="f5788-156">Si consiglia denominare il file di visualizzazione *cshtml* e utilizzare il *componenti/Shared/visualizzazioni/\<view_component_name > /\<view_name >* percorso.</span><span class="sxs-lookup"><span data-stu-id="f5788-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="f5788-157">Il `PriorityList` del componente di visualizzazione utilizzato in questo esempio Usa *Views/Shared/Components/PriorityList/Default.cshtml* per la vista del componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="f5788-158">Richiamo di un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-158">Invoking a view component</span></span>

<span data-ttu-id="f5788-159">Per utilizzare il componente di visualizzazione, chiamare le operazioni seguenti all'interno di una vista:</span><span class="sxs-lookup"><span data-stu-id="f5788-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="f5788-160">I parametri verranno passati al `InvokeAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="f5788-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="f5788-161">Il `PriorityList` sviluppato nell'articolo del componente di visualizzazione viene richiamato dal *Views/Todo/Index.cshtml* file della vista.</span><span class="sxs-lookup"><span data-stu-id="f5788-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="f5788-162">Nell'esempio seguente, il `InvokeAsync` metodo viene chiamato con due parametri:</span><span class="sxs-lookup"><span data-stu-id="f5788-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="f5788-163">Richiamo di un componente di visualizzazione come un Helper Tag</span><span class="sxs-lookup"><span data-stu-id="f5788-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="f5788-164">Per ASP.NET Core 1.1 e versioni successive, è possibile richiamare un componente di visualizzazione come una [Helper di Tag](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="f5788-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="f5788-165">Base alla convezione Pascal classe e metodo parametri per gli helper di Tag vengono convertiti in loro [inferiore case kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="f5788-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="f5788-166">Utilizza l'Helper di Tag per richiamare un componente di visualizzazione di `<vc></vc>` elemento.</span><span class="sxs-lookup"><span data-stu-id="f5788-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="f5788-167">Il componente di visualizzazione viene specificato come segue:</span><span class="sxs-lookup"><span data-stu-id="f5788-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="f5788-168">Nota: Per utilizzare un componente di visualizzazione come un Helper Tag, è necessario registrare l'assembly contenente il componente di visualizzazione utilizzando il `@addTagHelper` direttiva.</span><span class="sxs-lookup"><span data-stu-id="f5788-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="f5788-169">Ad esempio, se il componente di visualizzazione si trova in un assembly denominato "MyWebApp", aggiungere la seguente direttiva per il `_ViewImports.cshtml` file:</span><span class="sxs-lookup"><span data-stu-id="f5788-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="f5788-170">È possibile registrare un componente di visualizzazione come un Helper Tag a qualsiasi file che fa riferimento il componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="f5788-171">Vedere [Gestione ambito Helper di Tag](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) per ulteriori informazioni su come registrare gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="f5788-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="f5788-172">Il `InvokeAsync` metodo utilizzato in questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="f5788-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="f5788-173">Nel markup di Helper di Tag:</span><span class="sxs-lookup"><span data-stu-id="f5788-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="f5788-174">Nell'esempio precedente, il `PriorityList` del componente di visualizzazione diventa `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="f5788-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="f5788-175">I parametri per il componente di visualizzazione vengono passati come attributi in lettere minuscole kebab.</span><span class="sxs-lookup"><span data-stu-id="f5788-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="f5788-176">Richiamo di un componente di visualizzazione direttamente da un controller</span><span class="sxs-lookup"><span data-stu-id="f5788-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="f5788-177">Visualizza i componenti vengono in genere richiamati da una vista, ma è possibile richiamare tali direttamente da un metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="f5788-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="f5788-178">Mentre i componenti di visualizzazione non definiscono endpoint come controller, è possibile implementare un'azione del controller che restituisce il contenuto di un `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="f5788-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="f5788-179">In questo esempio, il componente di visualizzazione viene chiamato direttamente dal controller:</span><span class="sxs-lookup"><span data-stu-id="f5788-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="f5788-180">Procedura dettagliata: Creazione di un componente di visualizzazione semplice</span><span class="sxs-lookup"><span data-stu-id="f5788-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="f5788-181">[Scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilare e testare il codice di avvio.</span><span class="sxs-lookup"><span data-stu-id="f5788-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="f5788-182">Si tratta di un progetto semplice con un `Todo` controller che visualizza un elenco di *Todo* elementi.</span><span class="sxs-lookup"><span data-stu-id="f5788-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Elenco di ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="f5788-184">Aggiungere una classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="f5788-184">Add a ViewComponent class</span></span>

<span data-ttu-id="f5788-185">Creare un *ViewComponents* cartella e aggiungere le seguenti `PriorityListViewComponent` classe:</span><span class="sxs-lookup"><span data-stu-id="f5788-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="f5788-186">Note sul codice:</span><span class="sxs-lookup"><span data-stu-id="f5788-186">Notes on the code:</span></span>

* <span data-ttu-id="f5788-187">Visualizzazione classi di componenti possono essere contenute in **qualsiasi** cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="f5788-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="f5788-188">Poiché il nome della classe PriorityList**ViewComponent** termina con il suffisso **ViewComponent**, il runtime utilizza la stringa "PriorityList" quando si fa riferimento il componente della classe da una vista.</span><span class="sxs-lookup"><span data-stu-id="f5788-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="f5788-189">Verrà illustrato che in modo più dettagliato più avanti.</span><span class="sxs-lookup"><span data-stu-id="f5788-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="f5788-190">Il `[ViewComponent]` attributo è possibile modificare il nome utilizzato per fare riferimento a un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="f5788-191">Ad esempio, è stato chiamato la classe `XYZ` e applicati i `ViewComponent` attributo:</span><span class="sxs-lookup"><span data-stu-id="f5788-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="f5788-192">Il `[ViewComponent]` attributo indica il selettore di componente di visualizzazione per utilizzare il nome `PriorityList` durante la ricerca per le visualizzazioni associate, il componente e di utilizzare la stringa "PriorityList" quando si fa riferimento il componente della classe da una vista.</span><span class="sxs-lookup"><span data-stu-id="f5788-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="f5788-193">Verrà illustrato che in modo più dettagliato più avanti.</span><span class="sxs-lookup"><span data-stu-id="f5788-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="f5788-194">Il componente utilizza [inserimento di dipendenze](../../fundamentals/dependency-injection.md) a disposizione il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="f5788-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="f5788-195">`InvokeAsync`espone un metodo che può essere chiamato da una vista e può accettare un numero arbitrario di argomenti.</span><span class="sxs-lookup"><span data-stu-id="f5788-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="f5788-196">Il `InvokeAsync` il metodo restituisce il set di `ToDo` gli elementi che soddisfano il `isDone` e `maxPriority` parametri.</span><span class="sxs-lookup"><span data-stu-id="f5788-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="f5788-197">Creare la visualizzazione Razor componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-197">Create the view component Razor view</span></span>

* <span data-ttu-id="f5788-198">Creare il *componenti/Shared/visualizzazioni* cartella.</span><span class="sxs-lookup"><span data-stu-id="f5788-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="f5788-199">Questa cartella **deve** denominato *componenti*.</span><span class="sxs-lookup"><span data-stu-id="f5788-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="f5788-200">Creare il *viste/Shared/componenti/PriorityList* cartella.</span><span class="sxs-lookup"><span data-stu-id="f5788-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="f5788-201">Il nome della cartella deve corrispondere il nome della classe di componente di visualizzazione, o il nome della classe meno il suffisso (se è seguito convenzione e utilizzata la *ViewComponent* suffisso nel nome della classe).</span><span class="sxs-lookup"><span data-stu-id="f5788-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="f5788-202">Se è stata utilizzata la `ViewComponent` attributo, il nome della classe dovrà corrispondere la designazione di attributo.</span><span class="sxs-lookup"><span data-stu-id="f5788-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="f5788-203">Creare un *Views/Shared/Components/PriorityList/Default.cshtml* visualizzazione Razor:[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="f5788-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="f5788-204">La visualizzazione Razor accetta un elenco di `TodoItem` e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="f5788-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="f5788-205">Se il componente di visualizzazione `InvokeAsync` metodo non passa il nome della visualizzazione (come in questo esempio), *predefinito* per convenzione viene utilizzato per il nome della vista.</span><span class="sxs-lookup"><span data-stu-id="f5788-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="f5788-206">Più avanti nell'esercitazione verrà illustrato come passare il nome della vista.</span><span class="sxs-lookup"><span data-stu-id="f5788-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="f5788-207">Per ignorare lo stile predefinito per un controller specifico, aggiungere una visualizzazione nella cartella di visualizzazione controller specifico (ad esempio *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="f5788-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="f5788-208">Se il componente di visualizzazione è specifici dei controller, è possibile aggiungere alla cartella controller specifico (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f5788-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="f5788-209">Aggiungere un `div` che contiene una chiamata per il componente alla fine dell'elenco di priorità il *Views/Todo/index.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="f5788-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="f5788-210">Il markup `@await Component.InvokeAsync` viene illustrata la sintassi per chiamare i componenti di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f5788-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="f5788-211">Il primo argomento è il nome del componente di cui che si desidera richiamare o chiamare.</span><span class="sxs-lookup"><span data-stu-id="f5788-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="f5788-212">I parametri successivi vengono passati al componente.</span><span class="sxs-lookup"><span data-stu-id="f5788-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="f5788-213">`InvokeAsync`può richiedere un numero arbitrario di argomenti.</span><span class="sxs-lookup"><span data-stu-id="f5788-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="f5788-214">Testare l'app.</span><span class="sxs-lookup"><span data-stu-id="f5788-214">Test the app.</span></span> <span data-ttu-id="f5788-215">La figura seguente mostra l'elenco di attività e gli elementi con priorità:</span><span class="sxs-lookup"><span data-stu-id="f5788-215">The following image shows the ToDo list and the priority items:</span></span>

![elementi di elenco e priorità attività](view-components/_static/pi.png)

<span data-ttu-id="f5788-217">È inoltre possibile chiamare il componente di visualizzazione direttamente dal controller:</span><span class="sxs-lookup"><span data-stu-id="f5788-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementi con priorità dall'azione IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="f5788-219">Specificare un nome di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-219">Specifying a view name</span></span>

<span data-ttu-id="f5788-220">Un componente di visualizzazione complesso potrebbe essere necessario specificare una vista non predefinita in alcune condizioni.</span><span class="sxs-lookup"><span data-stu-id="f5788-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="f5788-221">Il codice seguente viene illustrato come specificare la vista "PVC" dal `InvokeAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="f5788-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="f5788-222">Aggiornamento di `InvokeAsync` metodo la `PriorityListViewComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="f5788-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="f5788-223">Copia il *Views/Shared/Components/PriorityList/Default.cshtml* file da una vista denominata *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f5788-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="f5788-224">Aggiungere un'intestazione per indicare che la vista PVC è in uso.</span><span class="sxs-lookup"><span data-stu-id="f5788-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="f5788-225">Aggiornamento *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f5788-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="f5788-226">Eseguire l'app e verificare la visualizzazione PVC.</span><span class="sxs-lookup"><span data-stu-id="f5788-226">Run the app and verify PVC view.</span></span>

![Priorità del componente di visualizzazione](view-components/_static/pvc.png)

<span data-ttu-id="f5788-228">Se non è eseguito il rendering della visualizzazione PVC, verificare che si sta chiamando il componente di visualizzazione con una priorità pari a 4 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="f5788-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="f5788-229">Esaminare il percorso di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f5788-229">Examine the view path</span></span>

* <span data-ttu-id="f5788-230">Modificare il parametro di priorità pari o inferiore a tre, pertanto la visualizzazione di priorità non viene restituita.</span><span class="sxs-lookup"><span data-stu-id="f5788-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="f5788-231">Rinominare temporaneamente il *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f5788-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="f5788-232">Testare l'app, si otterrà l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="f5788-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="f5788-233">Copia *Views/Todo/Components/PriorityList/1Default.cshtml* a *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f5788-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="f5788-234">Aggiungere codice per il *Shared* Todo visualizzazione componente per indicare la vista è dal *Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="f5788-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="f5788-235">Test di **Shared** vista del componente.</span><span class="sxs-lookup"><span data-stu-id="f5788-235">Test the **Shared** component view.</span></span>

![Output di attività con vista del componente condiviso](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="f5788-237">Evitare stringhe chiave</span><span class="sxs-lookup"><span data-stu-id="f5788-237">Avoiding magic strings</span></span>

<span data-ttu-id="f5788-238">Se si desidera compilare la sicurezza di tempo, è possibile sostituire il nome del componente di visualizzazione a livello di codice con il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="f5788-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="f5788-239">Creare il componente di visualizzazione senza il suffisso "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="f5788-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="f5788-240">Aggiungere un `using` istruzione per il Razor consente di visualizzare file e utilizzare il `nameof` operatore:</span><span class="sxs-lookup"><span data-stu-id="f5788-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="f5788-241">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f5788-241">Additional Resources</span></span>

* [<span data-ttu-id="f5788-242">Inserimento di dipendenze in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f5788-242">Dependency injection into views</span></span>](dependency-injection.md)
