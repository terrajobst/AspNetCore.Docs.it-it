---
title: Filtri in ASP.NET Core
author: Rick-Anderson
description: Informazioni sul funzionamento dei filtri e su come usarli in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 03335811766ea3a1455901199863c6da0e35f7e4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662785"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="cc72b-103">Filtri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc72b-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cc72b-104">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cc72b-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cc72b-105">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="cc72b-106">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="cc72b-107">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="cc72b-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="cc72b-108">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="cc72b-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="cc72b-109">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="cc72b-110">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="cc72b-111">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="cc72b-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="cc72b-112">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="cc72b-113">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span> <span data-ttu-id="cc72b-114">I filtri non funzionano direttamente con i [componenti Razor](xref:blazor/components).</span><span class="sxs-lookup"><span data-stu-id="cc72b-114">Filters don't work directly with [Razor components](xref:blazor/components).</span></span> <span data-ttu-id="cc72b-115">Un filtro può influire indirettamente su un componente solo quando:</span><span class="sxs-lookup"><span data-stu-id="cc72b-115">A filter can only indirectly affect a component when:</span></span>

* <span data-ttu-id="cc72b-116">Il componente è incorporato in una pagina o in una vista.</span><span class="sxs-lookup"><span data-stu-id="cc72b-116">The component is embedded in a page or view.</span></span>
* <span data-ttu-id="cc72b-117">La pagina o il controller/Visualizzazione usa il filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-117">The page or controller/view uses the filter.</span></span>

<span data-ttu-id="cc72b-118">[Visualizzare o scaricare un esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="cc72b-118">[View or download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="cc72b-119">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="cc72b-119">How filters work</span></span>

<span data-ttu-id="cc72b-120">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="cc72b-120">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span> <span data-ttu-id="cc72b-121">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="cc72b-121">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altri middleware, middleware di routing, selezione dell'azione e pipeline di chiamata dell'azione.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="cc72b-124">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-124">Filter types</span></span>

<span data-ttu-id="cc72b-125">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="cc72b-125">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="cc72b-126">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-126">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="cc72b-127">I filtri di autorizzazione consentono di cortocircuitare la pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-127">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="cc72b-128">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="cc72b-128">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="cc72b-129">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-129">Run after authorization.</span></span>  
  * <span data-ttu-id="cc72b-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> esegue il codice prima del resto della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="cc72b-131">Ad esempio, `OnResourceExecuting` esegue codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="cc72b-131">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="cc72b-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> esegue il codice una volta completato il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="cc72b-133">[Filtri azione](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="cc72b-133">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="cc72b-134">Eseguire il codice immediatamente prima e dopo la chiamata di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-134">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="cc72b-135">Consente di modificare gli argomenti passati in un'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-135">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="cc72b-136">Può modificare il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-136">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="cc72b-137">**Non** sono supportate in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc72b-137">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="cc72b-138">I [filtri eccezioni](#exception-filters) applicano i criteri globali alle eccezioni non gestite che si verificano prima della scrittura del corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-138">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="cc72b-139">I [filtri risultati](#result-filters) eseguono il codice immediatamente prima e dopo l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-139">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="cc72b-140">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-140">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="cc72b-141">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="cc72b-141">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="cc72b-142">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-142">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="cc72b-145">Implementazione</span><span class="sxs-lookup"><span data-stu-id="cc72b-145">Implementation</span></span>

<span data-ttu-id="cc72b-146">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-146">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="cc72b-147">I filtri sincroni eseguono codice prima e dopo la fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-147">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="cc72b-148">La chiamata di <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*>, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-148">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="cc72b-149">La chiamata di <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="cc72b-150">I filtri asincroni definiscono un metodo di `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-150">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="cc72b-151">Ad esempio, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="cc72b-151">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="cc72b-152">Nel codice precedente, il `SampleAsyncActionFilter` dispone di un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-152">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="cc72b-153">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="cc72b-153">Multiple filter stages</span></span>

<span data-ttu-id="cc72b-154">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="cc72b-154">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="cc72b-155">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa:</span><span class="sxs-lookup"><span data-stu-id="cc72b-155">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="cc72b-156">Sincrono: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-156">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="cc72b-157">Asincrono: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-157">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="cc72b-158">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="cc72b-158">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="cc72b-159">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="cc72b-159">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="cc72b-160">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="cc72b-160">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="cc72b-161">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="cc72b-161">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="cc72b-162">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-162">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="cc72b-163">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="cc72b-163">Built-in filter attributes</span></span>

<span data-ttu-id="cc72b-164">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="cc72b-164">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="cc72b-165">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-165">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-166">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-166">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="cc72b-167">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="cc72b-167">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="cc72b-168">Usare uno strumento come gli [strumenti di sviluppo del browser](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) per esaminare le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-168">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="cc72b-169">In **intestazioni risposta**viene visualizzato `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-169">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="cc72b-170">Il codice seguente implementa un `ActionFilterAttribute` che:</span><span class="sxs-lookup"><span data-stu-id="cc72b-170">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="cc72b-171">Legge il titolo e il nome dal sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-171">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="cc72b-172">A differenza dell'esempio precedente, il codice seguente non richiede l'aggiunta di parametri di filtro al codice.</span><span class="sxs-lookup"><span data-stu-id="cc72b-172">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="cc72b-173">Aggiunge il titolo e il nome all'intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-173">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-174">Le opzioni di configurazione vengono fornite dal [sistema di configurazione](xref:fundamentals/configuration/index) usando il [modello di opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="cc72b-174">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="cc72b-175">Ad esempio, dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="cc72b-175">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="cc72b-176">Nel `StartUp.ConfigureServices` eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-176">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="cc72b-177">La classe `PositionOptions` viene aggiunta al contenitore del servizio con l'area di configurazione `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-177">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="cc72b-178">Il `MyActionFilterAttribute` viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="cc72b-178">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="cc72b-179">Il codice seguente illustra la classe `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-179">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="cc72b-180">Il codice seguente applica il `MyActionFilterAttribute` al metodo `Index2`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-180">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="cc72b-181">In **intestazioni di risposta**, `author: Rick Anderson`e `Editor: Joe Smith` viene visualizzato quando viene chiamato l'endpoint di `Sample/Index2`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-181">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="cc72b-182">Il codice seguente applica il `MyActionFilterAttribute` e il `AddHeaderAttribute` alla pagina Razor:</span><span class="sxs-lookup"><span data-stu-id="cc72b-182">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="cc72b-183">Non è possibile applicare i filtri ai metodi del gestore di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="cc72b-183">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="cc72b-184">Possono essere applicati al modello di pagina Razor o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="cc72b-184">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="cc72b-185">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="cc72b-185">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="cc72b-186">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="cc72b-186">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="cc72b-187">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="cc72b-187">Filter scopes and order of execution</span></span>

<span data-ttu-id="cc72b-188">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="cc72b-188">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="cc72b-189">Uso di un attributo in un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-189">Using an attribute on a controller action.</span></span> <span data-ttu-id="cc72b-190">Non è possibile applicare attributi di filtro ai metodi del gestore Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc72b-190">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="cc72b-191">Uso di un attributo in un controller o in una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="cc72b-191">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="cc72b-192">A livello globale per tutti i controller, le azioni e Razor Pages come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-192">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="cc72b-193">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="cc72b-193">Default order of execution</span></span>

<span data-ttu-id="cc72b-194">Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione predefinito dei filtri.</span><span class="sxs-lookup"><span data-stu-id="cc72b-194">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="cc72b-195">I filtri globali racchiudono i filtri di classe, che a loro volta racchiudono i filtri dei metodi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-195">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="cc72b-196">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="cc72b-196">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="cc72b-197">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="cc72b-197">The filter sequence:</span></span>

* <span data-ttu-id="cc72b-198">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="cc72b-198">The *before* code of global filters.</span></span>
  * <span data-ttu-id="cc72b-199">Il codice *before* dei filtri della pagina controller e Razor.</span><span class="sxs-lookup"><span data-stu-id="cc72b-199">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="cc72b-200">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-200">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="cc72b-201">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-201">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="cc72b-202">Il codice *after* dei filtri della pagina controller e Razor.</span><span class="sxs-lookup"><span data-stu-id="cc72b-202">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="cc72b-203">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="cc72b-203">The *after* code of global filters.</span></span>
  
<span data-ttu-id="cc72b-204">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-204">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="cc72b-205">Sequenza</span><span class="sxs-lookup"><span data-stu-id="cc72b-205">Sequence</span></span> | <span data-ttu-id="cc72b-206">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-206">Filter scope</span></span> | <span data-ttu-id="cc72b-207">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-207">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="cc72b-208">1</span><span class="sxs-lookup"><span data-stu-id="cc72b-208">1</span></span> | <span data-ttu-id="cc72b-209">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-209">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-210">2</span><span class="sxs-lookup"><span data-stu-id="cc72b-210">2</span></span> | <span data-ttu-id="cc72b-211">Controller o pagina Razor</span><span class="sxs-lookup"><span data-stu-id="cc72b-211">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="cc72b-212">3</span><span class="sxs-lookup"><span data-stu-id="cc72b-212">3</span></span> | <span data-ttu-id="cc72b-213">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-213">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-214">4</span><span class="sxs-lookup"><span data-stu-id="cc72b-214">4</span></span> | <span data-ttu-id="cc72b-215">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-215">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="cc72b-216">5</span><span class="sxs-lookup"><span data-stu-id="cc72b-216">5</span></span> | <span data-ttu-id="cc72b-217">Controller o pagina Razor</span><span class="sxs-lookup"><span data-stu-id="cc72b-217">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="cc72b-218">6</span><span class="sxs-lookup"><span data-stu-id="cc72b-218">6</span></span> | <span data-ttu-id="cc72b-219">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-219">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="cc72b-220">Filtri a livello di controller</span><span class="sxs-lookup"><span data-stu-id="cc72b-220">Controller level filters</span></span>

<span data-ttu-id="cc72b-221">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-221">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="cc72b-222">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="cc72b-222">These methods:</span></span>

* <span data-ttu-id="cc72b-223">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-223">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="cc72b-224">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-224">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="cc72b-225">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-225">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="cc72b-226">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-226">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="cc72b-227">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-227">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="cc72b-228">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="cc72b-228">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="cc72b-229">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-229">The `TestController`:</span></span>

* <span data-ttu-id="cc72b-230">Applica il `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione di `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-230">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="cc72b-231">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-231">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="cc72b-232">Passando a `https://localhost:5001/Test2/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-232">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="cc72b-233">Filtri a livello di controller impostare la proprietà [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) su `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-233">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="cc72b-234">I filtri a livello di controller **non** possono essere impostati per l'esecuzione dopo i filtri applicati ai metodi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-234">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="cc72b-235">L'ordine è illustrato nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="cc72b-235">Order is explained in the next section.</span></span>

<span data-ttu-id="cc72b-236">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="cc72b-236">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="cc72b-237">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="cc72b-237">Overriding the default order</span></span>

<span data-ttu-id="cc72b-238">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-238">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="cc72b-239">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-239">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="cc72b-240">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="cc72b-240">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="cc72b-241">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="cc72b-241">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="cc72b-242">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="cc72b-242">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="cc72b-243">La proprietà `Order` è impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="cc72b-243">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="cc72b-244">Considerare i due filtri azione nel controller seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-244">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="cc72b-245">In `StartUp.ConfigureServices`viene aggiunto un filtro globale:</span><span class="sxs-lookup"><span data-stu-id="cc72b-245">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="cc72b-246">I 3 filtri vengono eseguiti nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-246">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="cc72b-247">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="cc72b-247">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="cc72b-248">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="cc72b-248">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="cc72b-249">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="cc72b-249">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="cc72b-250">Come indicato in precedenza, i filtri a livello di controller impostano la proprietà [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) su `int.MinValue` per i filtri predefiniti, l'ambito determina l'ordine a meno che `Order` non sia impostato su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="cc72b-250">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="cc72b-251">Nel codice precedente `MySampleActionFilter` ha un ambito globale, quindi viene eseguito prima `MyAction2FilterAttribute`, che ha ambito del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-251">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="cc72b-252">Per eseguire `MyAction2FilterAttribute` prima, impostare l'ordine su `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-252">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="cc72b-253">Per impostare il filtro globale `MySampleActionFilter` eseguire per primo, impostare `Order` su `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-253">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="cc72b-254">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="cc72b-254">Cancellation and short-circuiting</span></span>

<span data-ttu-id="cc72b-255">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-255">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="cc72b-256">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="cc72b-256">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-257">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-257">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="cc72b-258">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-258">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="cc72b-259">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-259">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="cc72b-260">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-260">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="cc72b-261">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-261">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="cc72b-262">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="cc72b-262">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="cc72b-263">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-263">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="cc72b-264">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="cc72b-264">Dependency injection</span></span>

<span data-ttu-id="cc72b-265">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="cc72b-265">Filters can be added by type or by instance.</span></span> <span data-ttu-id="cc72b-266">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-266">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="cc72b-267">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-267">If a type is added, it's type-activated.</span></span> <span data-ttu-id="cc72b-268">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-268">A type-activated filter means:</span></span>

* <span data-ttu-id="cc72b-269">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-269">An instance is created for each request.</span></span>
* <span data-ttu-id="cc72b-270">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cc72b-270">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="cc72b-271">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cc72b-271">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="cc72b-272">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="cc72b-272">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="cc72b-273">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-273">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="cc72b-274">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-274">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="cc72b-275">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-275">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="cc72b-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="cc72b-277">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-277">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="cc72b-278">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-278">Loggers are available from DI.</span></span> <span data-ttu-id="cc72b-279">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-279">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="cc72b-280">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-280">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="cc72b-281">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="cc72b-281">Logging added to filters:</span></span>

* <span data-ttu-id="cc72b-282">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-282">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="cc72b-283">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="cc72b-283">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="cc72b-284">Le azioni di log dei filtri incorporati e gli eventi del Framework.</span><span class="sxs-lookup"><span data-stu-id="cc72b-284">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="cc72b-285">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="cc72b-285">ServiceFilterAttribute</span></span>

<span data-ttu-id="cc72b-286">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-286">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="cc72b-287"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-287">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="cc72b-288">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-288">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="cc72b-289">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-289">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="cc72b-290">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-290">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="cc72b-291">Quando si usa l'impostazione `ServiceFilterAttribute`, [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="cc72b-291">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="cc72b-292">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-292">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="cc72b-293">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="cc72b-293">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="cc72b-294">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-294">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="cc72b-295">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-295">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="cc72b-296">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="cc72b-296">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="cc72b-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="cc72b-298">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-298">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="cc72b-299">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-299">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="cc72b-300">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="cc72b-300">TypeFilterAttribute</span></span>

<span data-ttu-id="cc72b-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="cc72b-302">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-302">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="cc72b-303">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-303">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="cc72b-304">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-304">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="cc72b-305">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-305">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="cc72b-306">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-306">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="cc72b-307">Quando si usa l'impostazione `TypeFilterAttribute`, [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="cc72b-307">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="cc72b-308">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-308">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="cc72b-309">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-309">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="cc72b-310">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="cc72b-310">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="cc72b-311">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-311">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="cc72b-312">Filtri di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="cc72b-312">Authorization filters</span></span>

<span data-ttu-id="cc72b-313">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-313">Authorization filters:</span></span>

* <span data-ttu-id="cc72b-314">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-314">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="cc72b-315">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-315">Control access to action methods.</span></span>
* <span data-ttu-id="cc72b-316">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-316">Have a before method, but no after method.</span></span>

<span data-ttu-id="cc72b-317">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-317">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="cc72b-318">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-318">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="cc72b-319">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="cc72b-319">The built-in authorization filter:</span></span>

* <span data-ttu-id="cc72b-320">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-320">Calls the authorization system.</span></span>
* <span data-ttu-id="cc72b-321">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="cc72b-321">Does not authorize requests.</span></span>

<span data-ttu-id="cc72b-322">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-322">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="cc72b-323">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="cc72b-323">The exception will not be handled.</span></span>
* <span data-ttu-id="cc72b-324">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-324">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="cc72b-325">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-325">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="cc72b-326">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="cc72b-326">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="cc72b-327">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="cc72b-327">Resource filters</span></span>

<span data-ttu-id="cc72b-328">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="cc72b-328">Resource filters:</span></span>

* <span data-ttu-id="cc72b-329">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-329">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="cc72b-330">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-330">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="cc72b-331">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-331">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="cc72b-332">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-332">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="cc72b-333">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="cc72b-333">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="cc72b-334">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="cc72b-334">Resource filter examples:</span></span>

* <span data-ttu-id="cc72b-335">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cc72b-335">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="cc72b-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="cc72b-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="cc72b-337">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-337">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="cc72b-338">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="cc72b-338">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="cc72b-339">Filtri azioni</span><span class="sxs-lookup"><span data-stu-id="cc72b-339">Action filters</span></span>

<span data-ttu-id="cc72b-340">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc72b-340">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="cc72b-341">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-341">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="cc72b-342">Per ulteriori informazioni, vedere [Metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="cc72b-342">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="cc72b-343">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-343">Action filters:</span></span>

* <span data-ttu-id="cc72b-344">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-344">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="cc72b-345">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-345">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="cc72b-346">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-346">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="cc72b-347">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="cc72b-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-Abilita la lettura degli input in un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="cc72b-349"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-349"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="cc72b-350"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="cc72b-351">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-351">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="cc72b-352">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-352">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="cc72b-353">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-353">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="cc72b-354">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-354">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="cc72b-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="cc72b-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="cc72b-357">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="cc72b-357">Setting this property to null:</span></span>

  * <span data-ttu-id="cc72b-358">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="cc72b-358">Effectively handles the exception.</span></span>
  * <span data-ttu-id="cc72b-359">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-359">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="cc72b-360">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="cc72b-360">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="cc72b-361">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-361">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="cc72b-362">Restituisce `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-362">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="cc72b-363">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="cc72b-363">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="cc72b-364">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-364">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="cc72b-365">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="cc72b-365">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="cc72b-366">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="cc72b-366">Validate model state.</span></span>
* <span data-ttu-id="cc72b-367">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="cc72b-367">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-368">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-368">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="cc72b-369">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-369">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="cc72b-370">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="cc72b-371">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="cc72b-372">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="cc72b-372">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="cc72b-373">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="cc72b-373">Effectively handles an exception.</span></span>
  * <span data-ttu-id="cc72b-374">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-374">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="cc72b-375">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="cc72b-375">Exception filters</span></span>

<span data-ttu-id="cc72b-376">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-376">Exception filters:</span></span>

* <span data-ttu-id="cc72b-377">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-377">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="cc72b-378">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-378">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="cc72b-379">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="cc72b-379">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="cc72b-380">Il codice seguente verifica il filtro eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-380">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="cc72b-381">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-381">Exception filters:</span></span>

* <span data-ttu-id="cc72b-382">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-382">Don't have before and after events.</span></span>
* <span data-ttu-id="cc72b-383">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-383">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="cc72b-384">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-384">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="cc72b-385">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="cc72b-385">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="cc72b-386">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-386">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="cc72b-387">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-387">This stops propagation of the exception.</span></span> <span data-ttu-id="cc72b-388">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="cc72b-388">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="cc72b-389">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-389">Only an action filter can do that.</span></span>

<span data-ttu-id="cc72b-390">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-390">Exception filters:</span></span>

* <span data-ttu-id="cc72b-391">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-391">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="cc72b-392">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-392">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="cc72b-393">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-393">Prefer middleware for exception handling.</span></span> <span data-ttu-id="cc72b-394">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-394">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="cc72b-395">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="cc72b-395">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="cc72b-396">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="cc72b-396">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="cc72b-397">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="cc72b-397">Result filters</span></span>

<span data-ttu-id="cc72b-398">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="cc72b-398">Result filters:</span></span>

* <span data-ttu-id="cc72b-399">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="cc72b-399">Implement an interface:</span></span>
  * <span data-ttu-id="cc72b-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="cc72b-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="cc72b-402">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-402">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="cc72b-403">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="cc72b-403">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="cc72b-404">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="cc72b-404">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="cc72b-405">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-405">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="cc72b-406">Un'azione che restituisce una visualizzazione include l'elaborazione Razor come parte del <xref:Microsoft.AspNetCore.Mvc.ViewResult> in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-406">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="cc72b-407">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-407">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="cc72b-408">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="cc72b-408">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="cc72b-409">I filtri dei risultati vengono eseguiti solo quando un filtro azione o azione produce un risultato di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-409">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="cc72b-410">I filtri dei risultati non vengono eseguiti nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-410">Result filters are not executed when:</span></span>

* <span data-ttu-id="cc72b-411">Un filtro di autorizzazione o un filtro risorse cortocircui la pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-411">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="cc72b-412">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-412">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="cc72b-413">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-413">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="cc72b-414">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="cc72b-414">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="cc72b-415">Generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-415">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="cc72b-416">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-416">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="cc72b-417">Viene considerato un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-417">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="cc72b-418">Quando viene eseguito il <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> metodo, la risposta probabilmente è già stata inviata al client.</span><span class="sxs-lookup"><span data-stu-id="cc72b-418">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="cc72b-419">Se la risposta è già stata inviata al client, non è possibile modificarla.</span><span class="sxs-lookup"><span data-stu-id="cc72b-419">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="cc72b-420">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-420">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="cc72b-421">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-421">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="cc72b-422">L'impostazione di `Exception` su null gestisce efficacemente un'eccezione e impedisce che l'eccezione venga generata nuovamente in un secondo momento nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-422">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="cc72b-423">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-423">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="cc72b-424">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="cc72b-424">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="cc72b-425">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-425">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="cc72b-426">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-426">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="cc72b-427">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-427">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="cc72b-428">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-428">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="cc72b-429">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="cc72b-429">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="cc72b-430">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-430">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="cc72b-431">Sono inclusi i risultati dell'azione prodotti da:</span><span class="sxs-lookup"><span data-stu-id="cc72b-431">This includes action results produced by:</span></span>

* <span data-ttu-id="cc72b-432">Filtri di autorizzazione e filtri delle risorse che interessano il cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="cc72b-432">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="cc72b-433">Filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-433">Exception filters.</span></span>

<span data-ttu-id="cc72b-434">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="cc72b-434">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="cc72b-435">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="cc72b-435">IFilterFactory</span></span>

<span data-ttu-id="cc72b-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="cc72b-437">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-437">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="cc72b-438">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-438">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="cc72b-439">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-439">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="cc72b-440">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="cc72b-440">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="cc72b-441">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="cc72b-441">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="cc72b-442">Il filtro viene applicato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-442">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="cc72b-443">Testare il codice precedente eseguendo l' [esempio di download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="cc72b-443">Test the preceding code by running the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="cc72b-444">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="cc72b-444">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="cc72b-445">Accedere a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-445">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="cc72b-446">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-446">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="cc72b-447">**Autore:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="cc72b-447">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="cc72b-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="cc72b-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="cc72b-449">**interno:** `My header`</span><span class="sxs-lookup"><span data-stu-id="cc72b-449">**internal:** `My header`</span></span>

<span data-ttu-id="cc72b-450">Il codice precedente crea l'intestazione di risposta **Internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-450">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="cc72b-451">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="cc72b-451">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="cc72b-452">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="cc72b-452">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="cc72b-453">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="cc72b-453">Don't require passing parameters.</span></span>
* <span data-ttu-id="cc72b-454">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-454">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="cc72b-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="cc72b-456">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-456">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="cc72b-457">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-457">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="cc72b-458">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-458">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="cc72b-459">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-459">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="cc72b-460">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-460">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="cc72b-461">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-461">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="cc72b-462">Tuttavia, i filtri differiscono dal middleware in quanto fanno parte del runtime, il che significa che hanno accesso a contesto e costrutti.</span><span class="sxs-lookup"><span data-stu-id="cc72b-462">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="cc72b-463">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-463">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="cc72b-464">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-464">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="cc72b-465">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="cc72b-465">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="cc72b-466">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-466">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="cc72b-467">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="cc72b-467">Next actions</span></span>

* <span data-ttu-id="cc72b-468">Vedere [metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="cc72b-468">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="cc72b-469">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="cc72b-469">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cc72b-470">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cc72b-470">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cc72b-471">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-471">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="cc72b-472">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-472">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="cc72b-473">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="cc72b-473">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="cc72b-474">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="cc72b-474">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="cc72b-475">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-475">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="cc72b-476">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-476">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="cc72b-477">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="cc72b-477">Filters avoid duplicating code.</span></span> <span data-ttu-id="cc72b-478">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-478">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="cc72b-479">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-479">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="cc72b-480">[Visualizzare o scaricare un esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="cc72b-480">[View or download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="cc72b-481">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="cc72b-481">How filters work</span></span>

<span data-ttu-id="cc72b-482">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="cc72b-482">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="cc72b-483">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="cc72b-483">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altro middleware, il middleware di routing, la selezione dell'azione e la pipeline di chiamata di azioni ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="cc72b-486">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-486">Filter types</span></span>

<span data-ttu-id="cc72b-487">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="cc72b-487">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="cc72b-488">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-488">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="cc72b-489">I filtri di autorizzazione causano il corto circuito della pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-489">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="cc72b-490">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="cc72b-490">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="cc72b-491">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-491">Run after authorization.</span></span>  
  * <span data-ttu-id="cc72b-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> può eseguire codice prima del resto della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="cc72b-493">Ad esempio, `OnResourceExecuting` può eseguire codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="cc72b-493">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="cc72b-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> può eseguire codice dopo che il resto della pipeline è stato completato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="cc72b-495">I [filtri azione](#action-filters) possono eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singolo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-495">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="cc72b-496">Possono essere usati per modificare gli argomenti passati a un'azione e il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-496">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="cc72b-497">I filtri di azione **non** sono supportati in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc72b-497">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="cc72b-498">I [filtri eccezioni](#exception-filters) vengono usati per applicare i criteri globali a eccezioni non gestite che si verificano prima che qualsiasi elemento sia stato scritto nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-498">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="cc72b-499">I [filtri risultato](#result-filters) possono eseguire codice immediatamente prima e dopo l'esecuzione di risultati di azioni singole.</span><span class="sxs-lookup"><span data-stu-id="cc72b-499">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="cc72b-500">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-500">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="cc72b-501">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="cc72b-501">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="cc72b-502">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-502">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="cc72b-505">Implementazione</span><span class="sxs-lookup"><span data-stu-id="cc72b-505">Implementation</span></span>

<span data-ttu-id="cc72b-506">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-506">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="cc72b-507">I filtri sincroni possono eseguire codice prima (`On-Stage-Executing`) e dopo (`On-Stage-Executed`) la relativa fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-507">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="cc72b-508">La chiamata di `OnActionExecuting`, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-508">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="cc72b-509">La chiamata di `OnActionExecuted` avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-509">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="cc72b-510">I filtri asincroni definiscono un metodo `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-510">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="cc72b-511">Nel codice precedente `SampleAsyncActionFilter` ha un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-511">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="cc72b-512">Ognuno dei metodi `On-Stage-ExecutionAsync` accetta un oggetto `FilterType-ExecutionDelegate` che esegue la fase della pipeline del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-512">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="cc72b-513">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="cc72b-513">Multiple filter stages</span></span>

<span data-ttu-id="cc72b-514">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="cc72b-514">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="cc72b-515">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` e i relativi equivalenti asincroni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-515">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="cc72b-516">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="cc72b-516">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="cc72b-517">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="cc72b-517">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="cc72b-518">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="cc72b-518">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="cc72b-519">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="cc72b-519">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="cc72b-520">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-520">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="cc72b-521">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="cc72b-521">Built-in filter attributes</span></span>

<span data-ttu-id="cc72b-522">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="cc72b-522">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="cc72b-523">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-523">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-524">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-524">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="cc72b-525">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="cc72b-525">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="cc72b-526">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="cc72b-526">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="cc72b-527">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="cc72b-527">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="cc72b-528">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="cc72b-528">Filter scopes and order of execution</span></span>

<span data-ttu-id="cc72b-529">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="cc72b-529">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="cc72b-530">Usando un attributo di un'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-530">Using an attribute on an action.</span></span>
* <span data-ttu-id="cc72b-531">Usando un attributo di un controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-531">Using an attribute on a controller.</span></span>
* <span data-ttu-id="cc72b-532">A livello globale per tutti i controller e le azioni come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-532">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="cc72b-533">Il codice precedente aggiunge tre filtri a livello globale tramite la raccolta [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="cc72b-533">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="cc72b-534">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="cc72b-534">Default order of execution</span></span>

<span data-ttu-id="cc72b-535">Quando sono presenti più filtri *dello stesso tipo*, scope determina l'ordine predefinito di esecuzione del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-535">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="cc72b-536">Filtri globali circondano i filtri di classe.</span><span class="sxs-lookup"><span data-stu-id="cc72b-536">Global filters surround class filters.</span></span> <span data-ttu-id="cc72b-537">Filtri di classe filtri di metodo Racchiudi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-537">Class filters surround method filters.</span></span>

<span data-ttu-id="cc72b-538">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="cc72b-538">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="cc72b-539">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="cc72b-539">The filter sequence:</span></span>

* <span data-ttu-id="cc72b-540">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="cc72b-540">The *before* code of global filters.</span></span>
  * <span data-ttu-id="cc72b-541">Codice *before* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-541">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="cc72b-542">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-542">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="cc72b-543">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-543">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="cc72b-544">Codice *after* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-544">The *after* code of controller filters.</span></span>
* <span data-ttu-id="cc72b-545">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="cc72b-545">The *after* code of global filters.</span></span>
  
<span data-ttu-id="cc72b-546">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-546">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="cc72b-547">Sequenza</span><span class="sxs-lookup"><span data-stu-id="cc72b-547">Sequence</span></span> | <span data-ttu-id="cc72b-548">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-548">Filter scope</span></span> | <span data-ttu-id="cc72b-549">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-549">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="cc72b-550">1</span><span class="sxs-lookup"><span data-stu-id="cc72b-550">1</span></span> | <span data-ttu-id="cc72b-551">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-551">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-552">2</span><span class="sxs-lookup"><span data-stu-id="cc72b-552">2</span></span> | <span data-ttu-id="cc72b-553">Controller</span><span class="sxs-lookup"><span data-stu-id="cc72b-553">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-554">3</span><span class="sxs-lookup"><span data-stu-id="cc72b-554">3</span></span> | <span data-ttu-id="cc72b-555">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-555">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-556">4</span><span class="sxs-lookup"><span data-stu-id="cc72b-556">4</span></span> | <span data-ttu-id="cc72b-557">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-557">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="cc72b-558">5</span><span class="sxs-lookup"><span data-stu-id="cc72b-558">5</span></span> | <span data-ttu-id="cc72b-559">Controller</span><span class="sxs-lookup"><span data-stu-id="cc72b-559">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="cc72b-560">6</span><span class="sxs-lookup"><span data-stu-id="cc72b-560">6</span></span> | <span data-ttu-id="cc72b-561">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-561">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="cc72b-562">Questa sequenza mostra che:</span><span class="sxs-lookup"><span data-stu-id="cc72b-562">This sequence shows:</span></span>

* <span data-ttu-id="cc72b-563">Il filtro del metodo è annidato all'interno del filtro del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-563">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="cc72b-564">Il filtro del controller è annidato all'interno del filtro globale.</span><span class="sxs-lookup"><span data-stu-id="cc72b-564">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="cc72b-565">Filtri a livello di controller e pagina Razor</span><span class="sxs-lookup"><span data-stu-id="cc72b-565">Controller and Razor Page level filters</span></span>

<span data-ttu-id="cc72b-566">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-566">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="cc72b-567">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="cc72b-567">These methods:</span></span>

* <span data-ttu-id="cc72b-568">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-568">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="cc72b-569">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-569">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="cc72b-570">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-570">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="cc72b-571">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-571">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="cc72b-572">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-572">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="cc72b-573">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="cc72b-573">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="cc72b-574">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-574">The `TestController`:</span></span>

* <span data-ttu-id="cc72b-575">Applica il `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione di `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-575">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="cc72b-576">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-576">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="cc72b-577">Passando a `https://localhost:5001/Test/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc72b-577">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="cc72b-578">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="cc72b-578">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="cc72b-579">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="cc72b-579">Overriding the default order</span></span>

<span data-ttu-id="cc72b-580">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-580">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="cc72b-581">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-581">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="cc72b-582">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="cc72b-582">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="cc72b-583">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="cc72b-583">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="cc72b-584">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="cc72b-584">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="cc72b-585">La proprietà `Order` può essere impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="cc72b-585">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="cc72b-586">Prendere in considerazione gli stessi tre filtri di azione illustrati nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-586">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="cc72b-587">Se la proprietà `Order` del controller e dei filtri globali è impostata, rispettivamente, su 1 e su 2, l'ordine di esecuzione viene invertito.</span><span class="sxs-lookup"><span data-stu-id="cc72b-587">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="cc72b-588">Sequenza</span><span class="sxs-lookup"><span data-stu-id="cc72b-588">Sequence</span></span> | <span data-ttu-id="cc72b-589">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-589">Filter scope</span></span> | <span data-ttu-id="cc72b-590">Proprietà `Order`</span><span class="sxs-lookup"><span data-stu-id="cc72b-590">`Order` property</span></span> | <span data-ttu-id="cc72b-591">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-591">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="cc72b-592">1</span><span class="sxs-lookup"><span data-stu-id="cc72b-592">1</span></span> | <span data-ttu-id="cc72b-593">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-593">Method</span></span> | <span data-ttu-id="cc72b-594">0</span><span class="sxs-lookup"><span data-stu-id="cc72b-594">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="cc72b-595">2</span><span class="sxs-lookup"><span data-stu-id="cc72b-595">2</span></span> | <span data-ttu-id="cc72b-596">Controller</span><span class="sxs-lookup"><span data-stu-id="cc72b-596">Controller</span></span> | <span data-ttu-id="cc72b-597">1</span><span class="sxs-lookup"><span data-stu-id="cc72b-597">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="cc72b-598">3</span><span class="sxs-lookup"><span data-stu-id="cc72b-598">3</span></span> | <span data-ttu-id="cc72b-599">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-599">Global</span></span> | <span data-ttu-id="cc72b-600">2</span><span class="sxs-lookup"><span data-stu-id="cc72b-600">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="cc72b-601">4</span><span class="sxs-lookup"><span data-stu-id="cc72b-601">4</span></span> | <span data-ttu-id="cc72b-602">Global</span><span class="sxs-lookup"><span data-stu-id="cc72b-602">Global</span></span> | <span data-ttu-id="cc72b-603">2</span><span class="sxs-lookup"><span data-stu-id="cc72b-603">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="cc72b-604">5</span><span class="sxs-lookup"><span data-stu-id="cc72b-604">5</span></span> | <span data-ttu-id="cc72b-605">Controller</span><span class="sxs-lookup"><span data-stu-id="cc72b-605">Controller</span></span> | <span data-ttu-id="cc72b-606">1</span><span class="sxs-lookup"><span data-stu-id="cc72b-606">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="cc72b-607">6</span><span class="sxs-lookup"><span data-stu-id="cc72b-607">6</span></span> | <span data-ttu-id="cc72b-608">Metodo</span><span class="sxs-lookup"><span data-stu-id="cc72b-608">Method</span></span> | <span data-ttu-id="cc72b-609">0</span><span class="sxs-lookup"><span data-stu-id="cc72b-609">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="cc72b-610">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="cc72b-610">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="cc72b-611">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="cc72b-611">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="cc72b-612">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="cc72b-612">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="cc72b-613">Per i filtri predefiniti, l'ambito determina l'ordine, a meno che non si imposti `Order` su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="cc72b-613">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="cc72b-614">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="cc72b-614">Cancellation and short-circuiting</span></span>

<span data-ttu-id="cc72b-615">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-615">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="cc72b-616">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="cc72b-616">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-617">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-617">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="cc72b-618">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-618">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="cc72b-619">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-619">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="cc72b-620">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-620">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="cc72b-621">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-621">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="cc72b-622">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="cc72b-622">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="cc72b-623">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-623">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="cc72b-624">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="cc72b-624">Dependency injection</span></span>

<span data-ttu-id="cc72b-625">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="cc72b-625">Filters can be added by type or by instance.</span></span> <span data-ttu-id="cc72b-626">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-626">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="cc72b-627">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-627">If a type is added, it's type-activated.</span></span> <span data-ttu-id="cc72b-628">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-628">A type-activated filter means:</span></span>

* <span data-ttu-id="cc72b-629">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-629">An instance is created for each request.</span></span>
* <span data-ttu-id="cc72b-630">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cc72b-630">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="cc72b-631">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cc72b-631">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="cc72b-632">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="cc72b-632">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="cc72b-633">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-633">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="cc72b-634">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-634">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="cc72b-635">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-635">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="cc72b-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="cc72b-637">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-637">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="cc72b-638">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-638">Loggers are available from DI.</span></span> <span data-ttu-id="cc72b-639">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-639">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="cc72b-640">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-640">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="cc72b-641">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="cc72b-641">Logging added to filters:</span></span>

* <span data-ttu-id="cc72b-642">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-642">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="cc72b-643">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="cc72b-643">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="cc72b-644">I filtri predefiniti registrano azioni ed eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="cc72b-644">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="cc72b-645">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="cc72b-645">ServiceFilterAttribute</span></span>

<span data-ttu-id="cc72b-646">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-646">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="cc72b-647"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-647">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="cc72b-648">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-648">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="cc72b-649">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-649">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="cc72b-650">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-650">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="cc72b-651">Quando si usa l'impostazione `ServiceFilterAttribute`, [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="cc72b-651">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="cc72b-652">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-652">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="cc72b-653">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="cc72b-653">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="cc72b-654">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-654">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="cc72b-655">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-655">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="cc72b-656">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="cc72b-656">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="cc72b-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="cc72b-658">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-658">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="cc72b-659">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-659">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="cc72b-660">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="cc72b-660">TypeFilterAttribute</span></span>

<span data-ttu-id="cc72b-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="cc72b-662">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-662">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="cc72b-663">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="cc72b-663">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="cc72b-664">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-664">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="cc72b-665">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-665">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="cc72b-666">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-666">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="cc72b-667">Quando si usa l'impostazione `TypeFilterAttribute`, [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="cc72b-667">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="cc72b-668">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-668">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="cc72b-669">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-669">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="cc72b-670">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="cc72b-670">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="cc72b-671">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-671">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="cc72b-672">Filtri di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="cc72b-672">Authorization filters</span></span>

<span data-ttu-id="cc72b-673">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-673">Authorization filters:</span></span>

* <span data-ttu-id="cc72b-674">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-674">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="cc72b-675">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-675">Control access to action methods.</span></span>
* <span data-ttu-id="cc72b-676">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-676">Have a before method, but no after method.</span></span>

<span data-ttu-id="cc72b-677">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-677">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="cc72b-678">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-678">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="cc72b-679">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="cc72b-679">The built-in authorization filter:</span></span>

* <span data-ttu-id="cc72b-680">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-680">Calls the authorization system.</span></span>
* <span data-ttu-id="cc72b-681">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="cc72b-681">Does not authorize requests.</span></span>

<span data-ttu-id="cc72b-682">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-682">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="cc72b-683">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="cc72b-683">The exception will not be handled.</span></span>
* <span data-ttu-id="cc72b-684">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-684">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="cc72b-685">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-685">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="cc72b-686">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="cc72b-686">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="cc72b-687">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="cc72b-687">Resource filters</span></span>

<span data-ttu-id="cc72b-688">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="cc72b-688">Resource filters:</span></span>

* <span data-ttu-id="cc72b-689">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-689">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="cc72b-690">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-690">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="cc72b-691">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-691">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="cc72b-692">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-692">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="cc72b-693">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="cc72b-693">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="cc72b-694">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="cc72b-694">Resource filter examples:</span></span>

* <span data-ttu-id="cc72b-695">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cc72b-695">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="cc72b-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="cc72b-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="cc72b-697">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-697">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="cc72b-698">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="cc72b-698">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="cc72b-699">Filtri azioni</span><span class="sxs-lookup"><span data-stu-id="cc72b-699">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc72b-700">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc72b-700">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="cc72b-701">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-701">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="cc72b-702">Per ulteriori informazioni, vedere [Metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="cc72b-702">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="cc72b-703">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-703">Action filters:</span></span>

* <span data-ttu-id="cc72b-704">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-704">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="cc72b-705">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-705">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="cc72b-706">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-706">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="cc72b-707">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-707">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="cc72b-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: consente di leggere gli input per un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="cc72b-709"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="cc72b-709"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="cc72b-710"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="cc72b-711">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-711">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="cc72b-712">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-712">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="cc72b-713">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-713">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="cc72b-714">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-714">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="cc72b-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="cc72b-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="cc72b-717">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="cc72b-717">Setting this property to null:</span></span>

  * <span data-ttu-id="cc72b-718">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="cc72b-718">Effectively handles the exception.</span></span>
  * <span data-ttu-id="cc72b-719">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-719">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="cc72b-720">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="cc72b-720">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="cc72b-721">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-721">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="cc72b-722">Restituisce `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-722">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="cc72b-723">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="cc72b-723">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="cc72b-724">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-724">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="cc72b-725">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="cc72b-725">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="cc72b-726">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="cc72b-726">Validate model state.</span></span>
* <span data-ttu-id="cc72b-727">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="cc72b-727">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="cc72b-728">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="cc72b-728">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="cc72b-729">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-729">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="cc72b-730">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="cc72b-731">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="cc72b-732">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="cc72b-732">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="cc72b-733">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="cc72b-733">Effectively handles an exception.</span></span>
  * <span data-ttu-id="cc72b-734">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-734">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="cc72b-735">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="cc72b-735">Exception filters</span></span>

<span data-ttu-id="cc72b-736">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-736">Exception filters:</span></span>

* <span data-ttu-id="cc72b-737">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-737">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="cc72b-738">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-738">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="cc72b-739">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="cc72b-739">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="cc72b-740">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-740">Exception filters:</span></span>

* <span data-ttu-id="cc72b-741">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-741">Don't have before and after events.</span></span>
* <span data-ttu-id="cc72b-742">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-742">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="cc72b-743">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-743">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="cc72b-744">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="cc72b-744">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="cc72b-745">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="cc72b-745">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="cc72b-746">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-746">This stops propagation of the exception.</span></span> <span data-ttu-id="cc72b-747">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="cc72b-747">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="cc72b-748">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-748">Only an action filter can do that.</span></span>

<span data-ttu-id="cc72b-749">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="cc72b-749">Exception filters:</span></span>

* <span data-ttu-id="cc72b-750">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-750">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="cc72b-751">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="cc72b-751">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="cc72b-752">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-752">Prefer middleware for exception handling.</span></span> <span data-ttu-id="cc72b-753">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-753">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="cc72b-754">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="cc72b-754">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="cc72b-755">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="cc72b-755">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="cc72b-756">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="cc72b-756">Result filters</span></span>

<span data-ttu-id="cc72b-757">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="cc72b-757">Result filters:</span></span>

* <span data-ttu-id="cc72b-758">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="cc72b-758">Implement an interface:</span></span>
  * <span data-ttu-id="cc72b-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="cc72b-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="cc72b-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="cc72b-761">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-761">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="cc72b-762">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="cc72b-762">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="cc72b-763">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="cc72b-763">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="cc72b-764">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-764">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="cc72b-765">Un'azione che restituisce una visualizzazione include tutte le elaborazioni Razor come parte dell'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> eseguito.</span><span class="sxs-lookup"><span data-stu-id="cc72b-765">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="cc72b-766">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="cc72b-766">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="cc72b-767">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="cc72b-767">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="cc72b-768">I filtri dei risultati vengono eseguiti solo quando un filtro azione o azione produce un risultato di azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-768">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="cc72b-769">I filtri dei risultati non vengono eseguiti nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc72b-769">Result filters are not executed when:</span></span>

* <span data-ttu-id="cc72b-770">Un filtro di autorizzazione o un filtro risorse cortocircui la pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-770">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="cc72b-771">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-771">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="cc72b-772">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-772">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="cc72b-773">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="cc72b-773">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="cc72b-774">La generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-774">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="cc72b-775">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="cc72b-775">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="cc72b-776">È considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="cc72b-776">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="cc72b-777">Quando viene eseguito il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, è probabile che la risposta sia già stata inviata al client.</span><span class="sxs-lookup"><span data-stu-id="cc72b-777">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="cc72b-778">Se la risposta è già stata inviata al client, non è possibile modificarla ulteriormente.</span><span class="sxs-lookup"><span data-stu-id="cc72b-778">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="cc72b-779">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-779">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="cc72b-780">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-780">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="cc72b-781">Se si imposta `Exception` su Null l'eccezione viene gestita in modo efficace e si evita che venga generata nuovamente da ASP.NET Core in un punto successivo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-781">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="cc72b-782">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-782">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="cc72b-783">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="cc72b-783">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="cc72b-784">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-784">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="cc72b-785">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-785">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="cc72b-786">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="cc72b-786">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="cc72b-787">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="cc72b-787">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="cc72b-788">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="cc72b-788">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="cc72b-789">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cc72b-789">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="cc72b-790">Sono inclusi i risultati dell'azione prodotti da:</span><span class="sxs-lookup"><span data-stu-id="cc72b-790">This includes action results produced by:</span></span>

* <span data-ttu-id="cc72b-791">Filtri di autorizzazione e filtri delle risorse che interessano il cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="cc72b-791">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="cc72b-792">Filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="cc72b-792">Exception filters.</span></span>

<span data-ttu-id="cc72b-793">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="cc72b-793">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="cc72b-794">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="cc72b-794">IFilterFactory</span></span>

<span data-ttu-id="cc72b-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="cc72b-796">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-796">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="cc72b-797">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-797">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="cc72b-798">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="cc72b-798">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="cc72b-799">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="cc72b-799">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="cc72b-800">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="cc72b-800">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="cc72b-801">Il codice precedente può essere testato eseguendo l'[esempio scaricato](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="cc72b-801">The preceding code can be tested by running the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="cc72b-802">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="cc72b-802">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="cc72b-803">Accedere a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-803">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="cc72b-804">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="cc72b-804">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="cc72b-805">**Autore:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="cc72b-805">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="cc72b-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="cc72b-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="cc72b-807">**interno:** `My header`</span><span class="sxs-lookup"><span data-stu-id="cc72b-807">**internal:** `My header`</span></span>

<span data-ttu-id="cc72b-808">Il codice precedente crea l'intestazione di risposta **Internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-808">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="cc72b-809">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="cc72b-809">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="cc72b-810">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="cc72b-810">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="cc72b-811">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="cc72b-811">Don't require passing parameters.</span></span>
* <span data-ttu-id="cc72b-812">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-812">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="cc72b-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="cc72b-814">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="cc72b-814">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="cc72b-815">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cc72b-815">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="cc72b-816">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="cc72b-816">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="cc72b-817">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="cc72b-817">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="cc72b-818">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="cc72b-818">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="cc72b-819">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-819">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="cc72b-820">I filtri sono diversi dal middleware in quanto fanno parte del runtime di ASP.NET Core, quindi hanno accesso al contesto e ai costrutti di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc72b-820">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="cc72b-821">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="cc72b-821">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="cc72b-822">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="cc72b-822">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="cc72b-823">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="cc72b-823">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="cc72b-824">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="cc72b-824">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="cc72b-825">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="cc72b-825">Next actions</span></span>

* <span data-ttu-id="cc72b-826">Vedere [metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="cc72b-826">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="cc72b-827">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="cc72b-827">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
