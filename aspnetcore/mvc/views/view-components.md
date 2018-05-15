---
title: Componenti di visualizzazione in ASP.NET Core
author: rick-anderson
description: Informazioni sull'uso dei componenti di visualizzazione in ASP.NET Core e su come aggiungere tali componenti alle app.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: a3614024c7f776e4502bc049180ae1c965e31db4
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="daa9c-103">Componenti di visualizzazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="daa9c-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="daa9c-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="daa9c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="daa9c-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="daa9c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="daa9c-106">Introduzione ai componenti di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-106">Introducing view components</span></span>

<span data-ttu-id="daa9c-107">I componenti di visualizzazione sono una nuova funzionalità di ASP.NET Core MVC, hanno aspetti comuni con le visualizzazioni parziali, ma sono molto più efficienti.</span><span class="sxs-lookup"><span data-stu-id="daa9c-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="daa9c-108">I componenti di visualizzazione non usano l'associazione di modelli. Dipendono soltanto dai dati specificati in fase di chiamata.</span><span class="sxs-lookup"><span data-stu-id="daa9c-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="daa9c-109">Un componente di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="daa9c-109">A view component:</span></span>

* <span data-ttu-id="daa9c-110">Esegue il rendering di un blocco anziché di un'intera risposta.</span><span class="sxs-lookup"><span data-stu-id="daa9c-110">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="daa9c-111">Include la stessa separazione dei concetti e gli stessi vantaggi per i test individuati nel controller e nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-111">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="daa9c-112">Può contenere parametri e logica di business.</span><span class="sxs-lookup"><span data-stu-id="daa9c-112">Can have parameters and business logic.</span></span>
* <span data-ttu-id="daa9c-113">In genere viene richiamato da una pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="daa9c-113">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="daa9c-114">I componenti di visualizzazione possono essere impiegati in un punto qualsiasi della logica di rendering riutilizzabile che risulta troppo complessa per una visualizzazione parziale, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="daa9c-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="daa9c-115">Menu di spostamento dinamici</span><span class="sxs-lookup"><span data-stu-id="daa9c-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="daa9c-116">Tag cloud (nel quale viene eseguita una query del database)</span><span class="sxs-lookup"><span data-stu-id="daa9c-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="daa9c-117">Pannello di accesso</span><span class="sxs-lookup"><span data-stu-id="daa9c-117">Login panel</span></span>
* <span data-ttu-id="daa9c-118">Carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="daa9c-118">Shopping cart</span></span>
* <span data-ttu-id="daa9c-119">Articoli recentemente pubblicati</span><span class="sxs-lookup"><span data-stu-id="daa9c-119">Recently published articles</span></span>
* <span data-ttu-id="daa9c-120">Contenuto dell'intestazione laterale in un blog tradizionale</span><span class="sxs-lookup"><span data-stu-id="daa9c-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="daa9c-121">Pannello di accesso che viene eseguito in ogni pagina e che visualizza il collegamento di accesso o di disconnessione, a seconda dello stato di accesso dell'utente</span><span class="sxs-lookup"><span data-stu-id="daa9c-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="daa9c-122">Un componente di visualizzazione è costituito da due parti: la classe (in genere derivata da [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) e il risultato restituito (in genere una visualizzazione).</span><span class="sxs-lookup"><span data-stu-id="daa9c-122">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="daa9c-123">Come per i controller, un componente di visualizzazione può essere un oggetto POCO. Molti sviluppatori preferiscono tuttavia sfruttare i metodi e le proprietà disponibili derivando da `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="daa9c-124">Creazione di un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-124">Creating a view component</span></span>

<span data-ttu-id="daa9c-125">Questa sezione contiene i requisiti principali per creare un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="daa9c-126">Più avanti in questo articolo, vengono esaminati nel dettaglio tutti i passaggi e viene creato un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="daa9c-127">Classe del componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-127">The view component class</span></span>

<span data-ttu-id="daa9c-128">È possibile creare una classe del componente di visualizzazione in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="daa9c-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="daa9c-129">Derivando da *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="daa9c-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="daa9c-130">Assegnando a una classe con l'attributo `[ViewComponent]` o derivando da una classe con l'attributo `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="daa9c-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="daa9c-131">Creando una classe in cui il nome termina con il suffisso *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="daa9c-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="daa9c-132">Come i controller, i componenti di visualizzazione devono essere classi pubbliche, non devono essere classi annidate e astratte.</span><span class="sxs-lookup"><span data-stu-id="daa9c-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="daa9c-133">Il nome del componente di visualizzazione corrisponde al nome della classe privato del suffisso "ViewComponent".</span><span class="sxs-lookup"><span data-stu-id="daa9c-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="daa9c-134">È anche possibile specificarlo in modo esplicito usando la proprietà `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="daa9c-135">Una classe del componente di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="daa9c-135">A view component class:</span></span>

* <span data-ttu-id="daa9c-136">Supporta pienamente l'[inserimento delle dipendenze ](../../fundamentals/dependency-injection.md) del costruttore</span><span class="sxs-lookup"><span data-stu-id="daa9c-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="daa9c-137">Non partecipa al ciclo di vita del controller, non è quindi possibile usare i [filtri](../controllers/filters.md) in un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="daa9c-138">Metodi del componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-138">View component methods</span></span>

<span data-ttu-id="daa9c-139">Un componente di visualizzazione definisce la propria logica in un metodo `InvokeAsync` che restituisce `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="daa9c-140">I parametri vengono rilevati direttamente dalla chiamata del componente di visualizzazione e non dall'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="daa9c-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="daa9c-141">Un componente di visualizzazione non gestisce mai direttamente una richiesta.</span><span class="sxs-lookup"><span data-stu-id="daa9c-141">A view component never directly handles a request.</span></span> <span data-ttu-id="daa9c-142">In genere, inizializza un modello e lo passa a una visualizzazione chiamando il metodo `View`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="daa9c-143">Riepilogando, i metodi del componente di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="daa9c-143">In summary, view component methods:</span></span>

* <span data-ttu-id="daa9c-144">Definiscono un metodo `InvokeAsync` che restituisce `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="daa9c-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="daa9c-145">In genere, inizializzano un modello e lo passano a una visualizzazione chiamando il metodo `ViewComponent` `View`</span><span class="sxs-lookup"><span data-stu-id="daa9c-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="daa9c-146">I parametri vengono rilevati dal metodo di chiamata, non da HTTP e non esiste un'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="daa9c-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="daa9c-147">Non sono raggiungibili direttamente come endpoint HTTP. Sono richiamati dal codice, solitamente in una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="daa9c-148">Un componente di visualizzazione non gestisce mai una richiesta</span><span class="sxs-lookup"><span data-stu-id="daa9c-148">A view component never handles a request</span></span>
* <span data-ttu-id="daa9c-149">Sono sottoposti a overload sulla firma e non sui dettagli dalla richiesta HHTP corrente</span><span class="sxs-lookup"><span data-stu-id="daa9c-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="daa9c-150">Percorso di ricerca della visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-150">View search path</span></span>

<span data-ttu-id="daa9c-151">Il runtime esegue la ricerca della visualizzazione nei percorsi seguenti:</span><span class="sxs-lookup"><span data-stu-id="daa9c-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="daa9c-152">Views/\<nome_controller>/Components/\<nome_componente_visualizzazione>/\<nome_visualizzazione></span><span class="sxs-lookup"><span data-stu-id="daa9c-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="daa9c-153">Views/Shared/Components/\<nome_componente_visualizzazione>/\<nome_visualizzazione></span><span class="sxs-lookup"><span data-stu-id="daa9c-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="daa9c-154">Il nome di visualizzazione predefinito per un componente di visualizzazione è *Default*, quindi il file della visualizzazione viene solitamente denominato *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="daa9c-155">È possibile specificare un nome di visualizzazione diverso quando si crea il risultato del componente di visualizzazione o quando si chiama il metodo `View`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="daa9c-156">Si consiglia di denominare il file di visualizzazione *Default.cshtml* e usare il percorso *Views/Shared/Components/\<nome_ccomponente_visualizzazione>/\<nome_visualizzazione>*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="daa9c-157">Il componente di visualizzazione `PriorityList` in questo esempio usa *Views/Shared/Components/PriorityList/Default.cshtml* per la visualizzazione del componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="daa9c-158">Chiamata di un componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-158">Invoking a view component</span></span>

<span data-ttu-id="daa9c-159">Per usare il componente di visualizzazione, chiamare il codice seguente all'interno di una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="daa9c-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="daa9c-160">I parametri saranno passati al metodo `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="daa9c-161">Il componente di visualizzazione `PriorityList` sviluppato nell'articolo viene richiamato dal file di visualizzazione *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="daa9c-162">Nell'esempio seguente il metodo `InvokeAsync` viene chiamato con due parametri:</span><span class="sxs-lookup"><span data-stu-id="daa9c-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="daa9c-163">Chiamata di un componente di visualizzazione come helper tag</span><span class="sxs-lookup"><span data-stu-id="daa9c-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="daa9c-164">Per ASP.NET Core 1.1 e versioni successive, è possibile richiamare un componente di visualizzazione come [helper tag](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="daa9c-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="daa9c-165">La classe scritta usando la convenzione Pascal e i parametri del metodo per gli helper tag vengono convertiti nel formato corrispondente [kebab case minuscolo](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="daa9c-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="daa9c-166">Per richiamare un componente di visualizzazione, l'helper tag usa l'elemento `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="daa9c-167">Il componente di visualizzazione viene specificato nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="daa9c-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="daa9c-168">Nota: per usare un componente di visualizzazione come helper tag, è necessario registrare l'assembly contenente il componente di visualizzazione usando la direttiva `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="daa9c-169">Ad esempio, se il componente di visualizzazione si trova in un assembly denominato "MyWebApp", aggiungere la direttiva seguente al file `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="daa9c-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="daa9c-170">È possibile registrare un componente di visualizzazione come helper tag per qualsiasi file che fa riferimento al componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="daa9c-171">Vedere [Gestione dell'ambito dell'helper tag](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) per altre informazioni su come registrare gli helper tag.</span><span class="sxs-lookup"><span data-stu-id="daa9c-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="daa9c-172">Il metodo `InvokeAsync` usato in questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="daa9c-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="daa9c-173">Nel markup dell'helper tag:</span><span class="sxs-lookup"><span data-stu-id="daa9c-173">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="daa9c-174">Nell'esempio precedente il componente di visualizzazione `PriorityList` diventa `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="daa9c-175">I parametri per il componente di visualizzazione vengono passati come attributi nel formato kebab case minuscolo.</span><span class="sxs-lookup"><span data-stu-id="daa9c-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="daa9c-176">Richiamo di un componente di visualizzazione direttamente da un controller</span><span class="sxs-lookup"><span data-stu-id="daa9c-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="daa9c-177">I componenti di visualizzazione sono solitamente richiamati da una visualizzazione, ma possono essere richiamati direttamente da un metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="daa9c-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="daa9c-178">A differenza dei controller i componenti di visualizzazione non definiscono endpoint. È tuttavia possibile implementare semplicemente un'azione del controller in modo che venga restituito il contenuto di un oggetto `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="daa9c-179">In questo esempio il componente di visualizzazione viene chiamato direttamente dal controller:</span><span class="sxs-lookup"><span data-stu-id="daa9c-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="daa9c-180">Procedura dettagliata: creazione di un componente di visualizzazione semplice</span><span class="sxs-lookup"><span data-stu-id="daa9c-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="daa9c-181">[Scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilare e testare il codice di avvio.</span><span class="sxs-lookup"><span data-stu-id="daa9c-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="daa9c-182">Si tratta di un progetto semplice con un controller `Todo` che visualizza un elenco di elementi *Todo*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Elenco di elementi ToDo](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="daa9c-184">Aggiungere una classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="daa9c-184">Add a ViewComponent class</span></span>

<span data-ttu-id="daa9c-185">Creare una cartella *ViewComponents* e aggiungere la classe `PriorityListViewComponent` seguente:</span><span class="sxs-lookup"><span data-stu-id="daa9c-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="daa9c-186">Note riguardanti il codice:</span><span class="sxs-lookup"><span data-stu-id="daa9c-186">Notes on the code:</span></span>

* <span data-ttu-id="daa9c-187">Le classi del componente di visualizzazione possono essere contenute in **qualsiasi** cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="daa9c-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="daa9c-188">Poiché il nome della classe PriorityList**ViewComponent** termina con il suffisso **ViewComponent**, il runtime usa la stringa "PriorityList" per fare riferimento al componente della classe da una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="daa9c-189">Questo aspetto viene poi spiegato in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="daa9c-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="daa9c-190">L'attributo `[ViewComponent]` può modificare il nome usato per fare riferimento a un componente di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="daa9c-191">Ad esempio, sarebbe stato possibile denominare la classe `XYZ` e applicare l'attributo `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="daa9c-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="daa9c-192">L'attributo `[ViewComponent]` precedente indica al selettore del componente di visualizzazione di usare il nome `PriorityList` per eseguire la ricerca delle visualizzazioni associate al componente e di usare la stringa "PriorityList" per fare riferimento al componente della classe da una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="daa9c-193">Questo aspetto viene poi spiegato in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="daa9c-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="daa9c-194">Il componente usa l'[inserimento delle dipendenze](../../fundamentals/dependency-injection.md) per rendere disponibile il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="daa9c-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="daa9c-195">`InvokeAsync` espone un metodo che può essere chiamato da una visualizzazione e può accettare un numero arbitrario di argomenti.</span><span class="sxs-lookup"><span data-stu-id="daa9c-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="daa9c-196">Il metodo `InvokeAsync` restituisce il set di elementi `ToDo` che soddisfano i parametri `isDone` e `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="daa9c-197">Creare la visualizzazione Razor del componente di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-197">Create the view component Razor view</span></span>

* <span data-ttu-id="daa9c-198">Creare la cartella *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="daa9c-199">Il nome di questa cartella **deve** essere *Components*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="daa9c-200">Creare la cartella *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="daa9c-201">Il nome di questa cartella deve corrispondere al nome della classe del componente di visualizzazione oppure al nome della classe privato del suffisso (se è stata adottata la convenzione ed è stato usato il suffisso *ViewComponent* nel nome della classe).</span><span class="sxs-lookup"><span data-stu-id="daa9c-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="daa9c-202">Se è stato usato l'attributo `ViewComponent`, il nome della classe dovrà corrispondere alla designazione dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="daa9c-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="daa9c-203">Creare una visualizzazione Razor *Views/Shared/Components/PriorityList/Default.cshtml*:[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="daa9c-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="daa9c-204">La visualizzazione Razor accetta un elenco di oggetti `TodoItem` e li visualizza.</span><span class="sxs-lookup"><span data-stu-id="daa9c-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="daa9c-205">Se il metodo `InvokeAsync` del componente di visualizzazione non passa il nome della visualizzazione (come in questo esempio), per convenzione viene usato *Default* come nome della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="daa9c-206">Più avanti nell'esercitazione viene illustrato come passare il nome della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="daa9c-207">Per sostituire lo stile predefinito per un controller specifico, aggiungere una visualizzazione alla cartella di visualizzazione specifica del controller, ad esempio *Views/Todo/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="daa9c-208">Se il componente di visualizzazione è specifico del controller, è possibile aggiungerlo alla cartella specifica del controller (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="daa9c-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="daa9c-209">Aggiungere un oggetto `div` contenente una chiamata al componente dell'elenco priorità alla fine del file *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="daa9c-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="daa9c-210">Il markup `@await Component.InvokeAsync` illustra la sintassi per chiamare i componenti di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="daa9c-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="daa9c-211">Il primo argomento corrisponde al nome del componente che si vuole richiamare o chiamare.</span><span class="sxs-lookup"><span data-stu-id="daa9c-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="daa9c-212">I parametri successivi vengono passati al componente.</span><span class="sxs-lookup"><span data-stu-id="daa9c-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="daa9c-213">`InvokeAsync` può accettare un numero arbitrario di argomenti.</span><span class="sxs-lookup"><span data-stu-id="daa9c-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="daa9c-214">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="daa9c-214">Test the app.</span></span> <span data-ttu-id="daa9c-215">La figura seguente illustra l'elenco ToDo e gli elementi con priorità:</span><span class="sxs-lookup"><span data-stu-id="daa9c-215">The following image shows the ToDo list and the priority items:</span></span>

![elenco todo ed elementi con priorità](view-components/_static/pi.png)

<span data-ttu-id="daa9c-217">È anche possibile chiamare il componente di visualizzazione direttamente dal controller:</span><span class="sxs-lookup"><span data-stu-id="daa9c-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementi con priorità dall'azione IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="daa9c-219">Impostazione di un nome di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-219">Specifying a view name</span></span>

<span data-ttu-id="daa9c-220">Per un componente di visualizzazione, in alcune condizioni è possibile dover specificare una visualizzazione non predefinita.</span><span class="sxs-lookup"><span data-stu-id="daa9c-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="daa9c-221">Il codice seguente illustra come specificare la visualizzazione "PVC" dal metodo `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="daa9c-222">Aggiornare il metodo `InvokeAsync` nella classe `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="daa9c-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="daa9c-223">Copia il file *Views/Shared/Components/PriorityList/Default.cshtml* in una visualizzazione denominata *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="daa9c-224">Aggiungere un'intestazione per indicare che viene usata una visualizzazione PVC.</span><span class="sxs-lookup"><span data-stu-id="daa9c-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="daa9c-225">Aggiornare *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="daa9c-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="daa9c-226">Eseguire l'app e verificare la visualizzazione PVC.</span><span class="sxs-lookup"><span data-stu-id="daa9c-226">Run the app and verify PVC view.</span></span>

![Componente di visualizzazione con priorità](view-components/_static/pvc.png)

<span data-ttu-id="daa9c-228">Se non viene eseguito il rendering della visualizzazione PVC, verificare che si stia chiamando il componente di visualizzazione con priorità pari a 4 o superiore.</span><span class="sxs-lookup"><span data-stu-id="daa9c-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="daa9c-229">Esaminare il percorso di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="daa9c-229">Examine the view path</span></span>

* <span data-ttu-id="daa9c-230">Modificare il parametro relativo alla priorità impostandolo su tre o priorità inferiore perché la visualizzazione con priorità non venga restituita.</span><span class="sxs-lookup"><span data-stu-id="daa9c-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="daa9c-231">Rinominare temporaneamente la cartella *Views/Todo/Components/PriorityList/Default.cshtml* in *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="daa9c-232">Testare l'app. Verrà visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="daa9c-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="daa9c-233">Copiare *Views/Todo/Components/PriorityList/1Default.cshtml* in *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="daa9c-234">Aggiungere alcuni markup alla visualizzazione del componente di visualizzazione Todo in *Shared* per indicare che la visualizzazione è presente nella cartella *Shared*.</span><span class="sxs-lookup"><span data-stu-id="daa9c-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="daa9c-235">Testare la visualizzazione del componente **Shared**.</span><span class="sxs-lookup"><span data-stu-id="daa9c-235">Test the **Shared** component view.</span></span>

![Output di ToDo con visualizzazione del componente Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="daa9c-237">Esclusione di "stringhe magiche"</span><span class="sxs-lookup"><span data-stu-id="daa9c-237">Avoiding magic strings</span></span>

<span data-ttu-id="daa9c-238">Per garantire la sicurezza in fase di compilazione, è possibile sostituire il nome del componente di compilazione hardcoded con il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="daa9c-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="daa9c-239">Creare il componente di visualizzazione senza il suffisso "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="daa9c-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="daa9c-240">Aggiungere un'istruzione `using` al file di visualizzazione Razor e usare l'operatore `nameof`:</span><span class="sxs-lookup"><span data-stu-id="daa9c-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="daa9c-241">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="daa9c-241">Additional resources</span></span>

* [<span data-ttu-id="daa9c-242">Inserimento di dipendenze in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="daa9c-242">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
