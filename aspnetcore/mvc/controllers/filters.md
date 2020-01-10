---
title: Filtri in ASP.NET Core
author: Rick-Anderson
description: Informazioni sul funzionamento dei filtri e su come usarli in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 759c150e7f35f3f6a52947edc5ef41448dc227fe
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828971"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="64f98-103">Filtri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64f98-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64f98-104">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="64f98-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="64f98-105">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="64f98-106">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="64f98-107">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="64f98-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="64f98-108">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="64f98-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="64f98-109">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="64f98-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="64f98-110">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="64f98-111">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="64f98-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="64f98-112">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="64f98-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="64f98-113">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="64f98-114">[Visualizzare o scaricare un esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="64f98-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="64f98-115">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="64f98-115">How filters work</span></span>

<span data-ttu-id="64f98-116">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="64f98-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="64f98-117">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="64f98-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altri middleware, middleware di routing, selezione dell'azione e pipeline di chiamata dell'azione.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="64f98-120">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-120">Filter types</span></span>

<span data-ttu-id="64f98-121">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="64f98-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="64f98-122">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="64f98-123">I filtri di autorizzazione consentono di cortocircuitare la pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="64f98-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="64f98-124">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="64f98-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="64f98-125">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-125">Run after authorization.</span></span>  
  * <span data-ttu-id="64f98-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> esegue il codice prima del resto della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="64f98-127">Ad esempio, `OnResourceExecuting` esegue codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="64f98-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="64f98-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> esegue il codice una volta completato il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="64f98-129">[Filtri azione](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="64f98-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="64f98-130">Eseguire il codice immediatamente prima e dopo la chiamata di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="64f98-131">Consente di modificare gli argomenti passati in un'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="64f98-132">Può modificare il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="64f98-133">**Non** sono supportate in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="64f98-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="64f98-134">I [filtri eccezioni](#exception-filters) applicano i criteri globali alle eccezioni non gestite che si verificano prima della scrittura del corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="64f98-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="64f98-135">I [filtri risultati](#result-filters) eseguono il codice immediatamente prima e dopo l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="64f98-136">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="64f98-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="64f98-137">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="64f98-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="64f98-138">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="64f98-141">Implementazione</span><span class="sxs-lookup"><span data-stu-id="64f98-141">Implementation</span></span>

<span data-ttu-id="64f98-142">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="64f98-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="64f98-143">I filtri sincroni eseguono codice prima e dopo la fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="64f98-144">La chiamata di <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*>, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="64f98-145">La chiamata di <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="64f98-146">I filtri asincroni definiscono un metodo di `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="64f98-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="64f98-147">Ad esempio, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="64f98-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="64f98-148">Nel codice precedente, il `SampleAsyncActionFilter` dispone di un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="64f98-149">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="64f98-149">Multiple filter stages</span></span>

<span data-ttu-id="64f98-150">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="64f98-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="64f98-151">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa:</span><span class="sxs-lookup"><span data-stu-id="64f98-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="64f98-152">Sincrono: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="64f98-153">Asincrono: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="64f98-154">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="64f98-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="64f98-155">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="64f98-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="64f98-156">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="64f98-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="64f98-157">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="64f98-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="64f98-158">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="64f98-159">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="64f98-159">Built-in filter attributes</span></span>

<span data-ttu-id="64f98-160">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="64f98-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="64f98-161">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="64f98-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-162">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="64f98-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="64f98-163">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="64f98-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="64f98-164">Usare uno strumento come gli [strumenti di sviluppo del browser](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) per esaminare le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="64f98-165">In **intestazioni risposta**viene visualizzato `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="64f98-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="64f98-166">Il codice seguente implementa un `ActionFilterAttribute` che:</span><span class="sxs-lookup"><span data-stu-id="64f98-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="64f98-167">Legge il titolo e il nome dal sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="64f98-168">A differenza dell'esempio precedente, il codice seguente non richiede l'aggiunta di parametri di filtro al codice.</span><span class="sxs-lookup"><span data-stu-id="64f98-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="64f98-169">Aggiunge il titolo e il nome all'intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="64f98-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-170">Le opzioni di configurazione vengono fornite dal [sistema di configurazione](xref:fundamentals/configuration/index) usando il [modello di opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="64f98-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="64f98-171">Ad esempio, dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="64f98-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="64f98-172">Nel `StartUp.ConfigureServices` eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="64f98-173">La classe `PositionOptions` viene aggiunta al contenitore del servizio con l'area di configurazione `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="64f98-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="64f98-174">Il `MyActionFilterAttribute` viene aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="64f98-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="64f98-175">Il codice seguente illustra la classe `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="64f98-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="64f98-176">Il codice seguente applica il `MyActionFilterAttribute` al metodo `Index2`:</span><span class="sxs-lookup"><span data-stu-id="64f98-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="64f98-177">In **intestazioni di risposta**, `author: Rick Anderson`e `Editor: Joe Smith` viene visualizzato quando viene chiamato l'endpoint di `Sample/Index2`.</span><span class="sxs-lookup"><span data-stu-id="64f98-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="64f98-178">Il codice seguente applica il `MyActionFilterAttribute` e il `AddHeaderAttribute` alla pagina Razor:</span><span class="sxs-lookup"><span data-stu-id="64f98-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="64f98-179">Non è possibile applicare i filtri ai metodi del gestore di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="64f98-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="64f98-180">Possono essere applicati al modello di pagina Razor o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="64f98-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="64f98-181">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="64f98-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="64f98-182">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="64f98-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="64f98-183">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="64f98-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="64f98-184">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="64f98-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="64f98-185">Uso di un attributo in un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="64f98-186">Non è possibile applicare attributi di filtro ai metodi del gestore Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="64f98-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="64f98-187">Uso di un attributo in un controller o in una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="64f98-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="64f98-188">A livello globale per tutti i controller, le azioni e Razor Pages come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="64f98-189">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="64f98-189">Default order of execution</span></span>

<span data-ttu-id="64f98-190">Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione predefinito dei filtri.</span><span class="sxs-lookup"><span data-stu-id="64f98-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="64f98-191">I filtri globali racchiudono i filtri di classe, che a loro volta racchiudono i filtri dei metodi.</span><span class="sxs-lookup"><span data-stu-id="64f98-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="64f98-192">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="64f98-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="64f98-193">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="64f98-193">The filter sequence:</span></span>

* <span data-ttu-id="64f98-194">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="64f98-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="64f98-195">Il codice *before* dei filtri della pagina controller e Razor.</span><span class="sxs-lookup"><span data-stu-id="64f98-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="64f98-196">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="64f98-197">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="64f98-198">Il codice *after* dei filtri della pagina controller e Razor.</span><span class="sxs-lookup"><span data-stu-id="64f98-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="64f98-199">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="64f98-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="64f98-200">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="64f98-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="64f98-201">Sequence</span><span class="sxs-lookup"><span data-stu-id="64f98-201">Sequence</span></span> | <span data-ttu-id="64f98-202">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-202">Filter scope</span></span> | <span data-ttu-id="64f98-203">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="64f98-204">1</span><span class="sxs-lookup"><span data-stu-id="64f98-204">1</span></span> | <span data-ttu-id="64f98-205">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-206">2</span><span class="sxs-lookup"><span data-stu-id="64f98-206">2</span></span> | <span data-ttu-id="64f98-207">Controller o pagina Razor</span><span class="sxs-lookup"><span data-stu-id="64f98-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="64f98-208">3\.</span><span class="sxs-lookup"><span data-stu-id="64f98-208">3</span></span> | <span data-ttu-id="64f98-209">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-210">4</span><span class="sxs-lookup"><span data-stu-id="64f98-210">4</span></span> | <span data-ttu-id="64f98-211">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="64f98-212">5</span><span class="sxs-lookup"><span data-stu-id="64f98-212">5</span></span> | <span data-ttu-id="64f98-213">Controller o pagina Razor</span><span class="sxs-lookup"><span data-stu-id="64f98-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="64f98-214">6</span><span class="sxs-lookup"><span data-stu-id="64f98-214">6</span></span> | <span data-ttu-id="64f98-215">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="64f98-216">Filtri a livello di controller</span><span class="sxs-lookup"><span data-stu-id="64f98-216">Controller level filters</span></span>

<span data-ttu-id="64f98-217">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="64f98-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="64f98-218">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="64f98-218">These methods:</span></span>

* <span data-ttu-id="64f98-219">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="64f98-220">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="64f98-221">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="64f98-222">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="64f98-223">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="64f98-224">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="64f98-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="64f98-225">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="64f98-225">The `TestController`:</span></span>

* <span data-ttu-id="64f98-226">Applica il `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione di `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="64f98-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="64f98-227">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="64f98-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="64f98-228">Passando a `https://localhost:5001/Test2/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="64f98-229">Filtri a livello di controller impostare la proprietà [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) su `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="64f98-229">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="64f98-230">I filtri a livello di controller **non** possono essere impostati per l'esecuzione dopo i filtri applicati ai metodi.</span><span class="sxs-lookup"><span data-stu-id="64f98-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="64f98-231">L'ordine è illustrato nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="64f98-231">Order is explained in the next section.</span></span>

<span data-ttu-id="64f98-232">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="64f98-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="64f98-233">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="64f98-233">Overriding the default order</span></span>

<span data-ttu-id="64f98-234">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="64f98-235">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="64f98-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="64f98-236">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="64f98-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="64f98-237">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="64f98-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="64f98-238">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="64f98-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="64f98-239">La proprietà `Order` è impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="64f98-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="64f98-240">Considerare i due filtri azione nel controller seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="64f98-241">In `StartUp.ConfigureServices`viene aggiunto un filtro globale:</span><span class="sxs-lookup"><span data-stu-id="64f98-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="64f98-242">I 3 filtri vengono eseguiti nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="64f98-243">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="64f98-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="64f98-244">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="64f98-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="64f98-245">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="64f98-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="64f98-246">Come indicato in precedenza, i filtri a livello di controller impostano la proprietà [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) su `int.MinValue` per i filtri predefiniti, l'ambito determina l'ordine a meno che `Order` non sia impostato su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="64f98-246">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="64f98-247">Nel codice precedente `MySampleActionFilter` ha un ambito globale, quindi viene eseguito prima `MyAction2FilterAttribute`, che ha ambito del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="64f98-248">Per eseguire `MyAction2FilterAttribute` prima, impostare l'ordine su `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="64f98-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="64f98-249">Per impostare il filtro globale `MySampleActionFilter` eseguire per primo, impostare `Order` su `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="64f98-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="64f98-250">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="64f98-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="64f98-251">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="64f98-252">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="64f98-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-253">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="64f98-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="64f98-254">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="64f98-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="64f98-255">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="64f98-256">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="64f98-257">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="64f98-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="64f98-258">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="64f98-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="64f98-259">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="64f98-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="64f98-260">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="64f98-260">Dependency injection</span></span>

<span data-ttu-id="64f98-261">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="64f98-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="64f98-262">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="64f98-263">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="64f98-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="64f98-264">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="64f98-264">A type-activated filter means:</span></span>

* <span data-ttu-id="64f98-265">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-265">An instance is created for each request.</span></span>
* <span data-ttu-id="64f98-266">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64f98-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="64f98-267">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64f98-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="64f98-268">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="64f98-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="64f98-269">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="64f98-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="64f98-270">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="64f98-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="64f98-271">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="64f98-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="64f98-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="64f98-273">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="64f98-274">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-274">Loggers are available from DI.</span></span> <span data-ttu-id="64f98-275">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="64f98-276">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="64f98-277">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="64f98-277">Logging added to filters:</span></span>

* <span data-ttu-id="64f98-278">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="64f98-279">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="64f98-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="64f98-280">Le azioni di log dei filtri incorporati e gli eventi del Framework.</span><span class="sxs-lookup"><span data-stu-id="64f98-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="64f98-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="64f98-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="64f98-282">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64f98-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="64f98-283"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="64f98-284">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="64f98-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="64f98-285">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="64f98-286">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="64f98-287">Quando si usa l'impostazione `ServiceFilterAttribute`, [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="64f98-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="64f98-288">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="64f98-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="64f98-289">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="64f98-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="64f98-290">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="64f98-291">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="64f98-292">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="64f98-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="64f98-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="64f98-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="64f98-294">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="64f98-295">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="64f98-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="64f98-296">TypeFilterAttribute</span></span>

<span data-ttu-id="64f98-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="64f98-298">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="64f98-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="64f98-299">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="64f98-300">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="64f98-301">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="64f98-302">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="64f98-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="64f98-303">Quando si usa l'impostazione `TypeFilterAttribute`, [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="64f98-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="64f98-304">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="64f98-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="64f98-305">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="64f98-306">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="64f98-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="64f98-307">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="64f98-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="64f98-308">Filtri di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="64f98-308">Authorization filters</span></span>

<span data-ttu-id="64f98-309">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="64f98-309">Authorization filters:</span></span>

* <span data-ttu-id="64f98-310">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="64f98-311">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-311">Control access to action methods.</span></span>
* <span data-ttu-id="64f98-312">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="64f98-313">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="64f98-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="64f98-314">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="64f98-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="64f98-315">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="64f98-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="64f98-316">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-316">Calls the authorization system.</span></span>
* <span data-ttu-id="64f98-317">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="64f98-317">Does not authorize requests.</span></span>

<span data-ttu-id="64f98-318">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="64f98-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="64f98-319">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="64f98-319">The exception will not be handled.</span></span>
* <span data-ttu-id="64f98-320">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="64f98-321">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="64f98-322">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="64f98-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="64f98-323">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="64f98-323">Resource filters</span></span>

<span data-ttu-id="64f98-324">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="64f98-324">Resource filters:</span></span>

* <span data-ttu-id="64f98-325">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="64f98-326">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="64f98-327">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="64f98-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="64f98-328">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="64f98-329">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="64f98-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="64f98-330">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="64f98-330">Resource filter examples:</span></span>

* <span data-ttu-id="64f98-331">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="64f98-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="64f98-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="64f98-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="64f98-333">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="64f98-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="64f98-334">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="64f98-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="64f98-335">Filtri azioni</span><span class="sxs-lookup"><span data-stu-id="64f98-335">Action filters</span></span>

<span data-ttu-id="64f98-336">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="64f98-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="64f98-337">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="64f98-338">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="64f98-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="64f98-339">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-339">Action filters:</span></span>

* <span data-ttu-id="64f98-340">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="64f98-341">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="64f98-342">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="64f98-343">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="64f98-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-Abilita la lettura degli input in un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="64f98-345"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="64f98-346"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="64f98-347">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="64f98-348">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="64f98-349">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="64f98-350">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="64f98-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="64f98-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="64f98-353">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="64f98-353">Setting this property to null:</span></span>

  * <span data-ttu-id="64f98-354">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="64f98-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="64f98-355">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="64f98-356">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="64f98-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="64f98-357">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="64f98-358">Restituisce un valore `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="64f98-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="64f98-359">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="64f98-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="64f98-360">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="64f98-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="64f98-361">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="64f98-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="64f98-362">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="64f98-362">Validate model state.</span></span>
* <span data-ttu-id="64f98-363">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="64f98-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-364">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="64f98-365">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="64f98-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="64f98-366">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="64f98-367">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="64f98-368">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="64f98-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="64f98-369">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="64f98-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="64f98-370">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="64f98-371">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="64f98-371">Exception filters</span></span>

<span data-ttu-id="64f98-372">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-372">Exception filters:</span></span>

* <span data-ttu-id="64f98-373">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="64f98-374">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="64f98-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="64f98-375">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="64f98-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="64f98-376">Il codice seguente verifica il filtro eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="64f98-377">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-377">Exception filters:</span></span>

* <span data-ttu-id="64f98-378">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-378">Don't have before and after events.</span></span>
* <span data-ttu-id="64f98-379">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64f98-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="64f98-380">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="64f98-381">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="64f98-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="64f98-382">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="64f98-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="64f98-383">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-383">This stops propagation of the exception.</span></span> <span data-ttu-id="64f98-384">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="64f98-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="64f98-385">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-385">Only an action filter can do that.</span></span>

<span data-ttu-id="64f98-386">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-386">Exception filters:</span></span>

* <span data-ttu-id="64f98-387">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="64f98-388">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="64f98-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="64f98-389">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="64f98-390">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="64f98-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="64f98-391">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="64f98-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="64f98-392">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="64f98-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="64f98-393">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="64f98-393">Result filters</span></span>

<span data-ttu-id="64f98-394">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="64f98-394">Result filters:</span></span>

* <span data-ttu-id="64f98-395">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="64f98-395">Implement an interface:</span></span>
  * <span data-ttu-id="64f98-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="64f98-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="64f98-398">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="64f98-399">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="64f98-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="64f98-400">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="64f98-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="64f98-401">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="64f98-402">Un'azione che restituisce una visualizzazione include l'elaborazione Razor come parte del <xref:Microsoft.AspNetCore.Mvc.ViewResult> in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="64f98-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="64f98-403">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="64f98-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="64f98-404">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="64f98-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="64f98-405">I filtri dei risultati vengono eseguiti solo quando un filtro azione o azione produce un risultato di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="64f98-406">I filtri dei risultati non vengono eseguiti nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="64f98-407">Un filtro di autorizzazione o un filtro risorse cortocircui la pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="64f98-408">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="64f98-409">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="64f98-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="64f98-410">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="64f98-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="64f98-411">Generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="64f98-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="64f98-412">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="64f98-413">Viene considerato un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="64f98-414">Quando viene eseguito il <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> metodo, la risposta probabilmente è già stata inviata al client.</span><span class="sxs-lookup"><span data-stu-id="64f98-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="64f98-415">Se la risposta è già stata inviata al client, non è possibile modificarla.</span><span class="sxs-lookup"><span data-stu-id="64f98-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="64f98-416">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="64f98-417">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="64f98-418">L'impostazione di `Exception` su null gestisce efficacemente un'eccezione e impedisce che l'eccezione venga generata nuovamente in un secondo momento nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="64f98-419">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="64f98-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="64f98-420">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="64f98-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="64f98-421">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="64f98-422">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="64f98-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="64f98-423">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="64f98-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="64f98-424">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="64f98-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="64f98-425">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="64f98-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="64f98-426">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="64f98-427">Sono inclusi i risultati dell'azione prodotti da:</span><span class="sxs-lookup"><span data-stu-id="64f98-427">This includes action results produced by:</span></span>

* <span data-ttu-id="64f98-428">Filtri di autorizzazione e filtri delle risorse che interessano il cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="64f98-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="64f98-429">Filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-429">Exception filters.</span></span>

<span data-ttu-id="64f98-430">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="64f98-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="64f98-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="64f98-431">IFilterFactory</span></span>

<span data-ttu-id="64f98-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="64f98-433">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="64f98-434">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="64f98-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="64f98-435">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="64f98-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="64f98-436">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="64f98-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="64f98-437">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="64f98-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="64f98-438">Il filtro viene applicato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="64f98-439">Testare il codice precedente eseguendo l' [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="64f98-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="64f98-440">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="64f98-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="64f98-441">Passare a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="64f98-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="64f98-442">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="64f98-443">**Autore:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="64f98-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="64f98-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="64f98-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="64f98-445">**interno:** `My header`</span><span class="sxs-lookup"><span data-stu-id="64f98-445">**internal:** `My header`</span></span>

<span data-ttu-id="64f98-446">Il codice precedente crea l'intestazione di risposta **Internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="64f98-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="64f98-447">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="64f98-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="64f98-448">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="64f98-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="64f98-449">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="64f98-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="64f98-450">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="64f98-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="64f98-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="64f98-452">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="64f98-453">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="64f98-454">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="64f98-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="64f98-455">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="64f98-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="64f98-456">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="64f98-457">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="64f98-458">Tuttavia, i filtri differiscono dal middleware in quanto fanno parte del runtime, il che significa che hanno accesso a contesto e costrutti.</span><span class="sxs-lookup"><span data-stu-id="64f98-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="64f98-459">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="64f98-460">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="64f98-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="64f98-461">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="64f98-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="64f98-462">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="64f98-463">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="64f98-463">Next actions</span></span>

* <span data-ttu-id="64f98-464">Vedere [metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="64f98-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="64f98-465">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="64f98-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64f98-466">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="64f98-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="64f98-467">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="64f98-468">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="64f98-469">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="64f98-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="64f98-470">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="64f98-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="64f98-471">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="64f98-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="64f98-472">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="64f98-473">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="64f98-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="64f98-474">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="64f98-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="64f98-475">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="64f98-476">[Visualizzare o scaricare un esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="64f98-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="64f98-477">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="64f98-477">How filters work</span></span>

<span data-ttu-id="64f98-478">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="64f98-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="64f98-479">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="64f98-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altro middleware, il middleware di routing, la selezione dell'azione e la pipeline di chiamata di azioni ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="64f98-482">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-482">Filter types</span></span>

<span data-ttu-id="64f98-483">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="64f98-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="64f98-484">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="64f98-485">I filtri di autorizzazione causano il corto circuito della pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="64f98-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="64f98-486">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="64f98-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="64f98-487">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-487">Run after authorization.</span></span>  
  * <span data-ttu-id="64f98-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> può eseguire codice prima del resto della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="64f98-489">Ad esempio, `OnResourceExecuting` può eseguire codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="64f98-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="64f98-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> può eseguire codice dopo che il resto della pipeline è stato completato.</span><span class="sxs-lookup"><span data-stu-id="64f98-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="64f98-491">I [filtri azione](#action-filters) possono eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singolo.</span><span class="sxs-lookup"><span data-stu-id="64f98-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="64f98-492">Possono essere usati per modificare gli argomenti passati a un'azione e il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="64f98-493">I filtri di azione **non** sono supportati in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="64f98-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="64f98-494">I [filtri eccezioni](#exception-filters) vengono usati per applicare i criteri globali a eccezioni non gestite che si verificano prima che qualsiasi elemento sia stato scritto nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="64f98-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="64f98-495">I [filtri risultato](#result-filters) possono eseguire codice immediatamente prima e dopo l'esecuzione di risultati di azioni singole.</span><span class="sxs-lookup"><span data-stu-id="64f98-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="64f98-496">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="64f98-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="64f98-497">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="64f98-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="64f98-498">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="64f98-501">Implementazione</span><span class="sxs-lookup"><span data-stu-id="64f98-501">Implementation</span></span>

<span data-ttu-id="64f98-502">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="64f98-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="64f98-503">I filtri sincroni possono eseguire codice prima (`On-Stage-Executing`) e dopo (`On-Stage-Executed`) la relativa fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="64f98-504">La chiamata di `OnActionExecuting`, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="64f98-505">La chiamata di `OnActionExecuted` avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="64f98-506">I filtri asincroni definiscono un metodo `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="64f98-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="64f98-507">Nel codice precedente `SampleAsyncActionFilter` ha un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="64f98-508">Ognuno dei metodi `On-Stage-ExecutionAsync` accetta un oggetto `FilterType-ExecutionDelegate` che esegue la fase della pipeline del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="64f98-509">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="64f98-509">Multiple filter stages</span></span>

<span data-ttu-id="64f98-510">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="64f98-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="64f98-511">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` e i relativi equivalenti asincroni.</span><span class="sxs-lookup"><span data-stu-id="64f98-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="64f98-512">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="64f98-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="64f98-513">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="64f98-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="64f98-514">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="64f98-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="64f98-515">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="64f98-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="64f98-516">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="64f98-517">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="64f98-517">Built-in filter attributes</span></span>

<span data-ttu-id="64f98-518">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="64f98-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="64f98-519">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="64f98-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-520">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="64f98-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="64f98-521">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="64f98-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="64f98-522">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="64f98-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="64f98-523">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="64f98-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="64f98-524">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="64f98-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="64f98-525">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="64f98-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="64f98-526">Usando un attributo di un'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="64f98-527">Usando un attributo di un controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="64f98-528">A livello globale per tutti i controller e le azioni come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="64f98-529">Il codice precedente aggiunge tre filtri a livello globale tramite la raccolta [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="64f98-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="64f98-530">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="64f98-530">Default order of execution</span></span>

<span data-ttu-id="64f98-531">Quando sono presenti più filtri *dello stesso tipo*, scope determina l'ordine predefinito di esecuzione del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="64f98-532">Filtri globali circondano i filtri di classe.</span><span class="sxs-lookup"><span data-stu-id="64f98-532">Global filters surround class filters.</span></span> <span data-ttu-id="64f98-533">Filtri di classe filtri di metodo Racchiudi.</span><span class="sxs-lookup"><span data-stu-id="64f98-533">Class filters surround method filters.</span></span>

<span data-ttu-id="64f98-534">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="64f98-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="64f98-535">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="64f98-535">The filter sequence:</span></span>

* <span data-ttu-id="64f98-536">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="64f98-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="64f98-537">Codice *before* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="64f98-538">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="64f98-539">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="64f98-540">Codice *after* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="64f98-541">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="64f98-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="64f98-542">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="64f98-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="64f98-543">Sequence</span><span class="sxs-lookup"><span data-stu-id="64f98-543">Sequence</span></span> | <span data-ttu-id="64f98-544">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-544">Filter scope</span></span> | <span data-ttu-id="64f98-545">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="64f98-546">1</span><span class="sxs-lookup"><span data-stu-id="64f98-546">1</span></span> | <span data-ttu-id="64f98-547">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-548">2</span><span class="sxs-lookup"><span data-stu-id="64f98-548">2</span></span> | <span data-ttu-id="64f98-549">Controller</span><span class="sxs-lookup"><span data-stu-id="64f98-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-550">3\.</span><span class="sxs-lookup"><span data-stu-id="64f98-550">3</span></span> | <span data-ttu-id="64f98-551">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-552">4</span><span class="sxs-lookup"><span data-stu-id="64f98-552">4</span></span> | <span data-ttu-id="64f98-553">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="64f98-554">5</span><span class="sxs-lookup"><span data-stu-id="64f98-554">5</span></span> | <span data-ttu-id="64f98-555">Controller</span><span class="sxs-lookup"><span data-stu-id="64f98-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="64f98-556">6</span><span class="sxs-lookup"><span data-stu-id="64f98-556">6</span></span> | <span data-ttu-id="64f98-557">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="64f98-558">Questa sequenza mostra che:</span><span class="sxs-lookup"><span data-stu-id="64f98-558">This sequence shows:</span></span>

* <span data-ttu-id="64f98-559">Il filtro del metodo è annidato all'interno del filtro del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="64f98-560">Il filtro del controller è annidato all'interno del filtro globale.</span><span class="sxs-lookup"><span data-stu-id="64f98-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="64f98-561">Filtri a livello di controller e pagina Razor</span><span class="sxs-lookup"><span data-stu-id="64f98-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="64f98-562">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="64f98-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="64f98-563">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="64f98-563">These methods:</span></span>

* <span data-ttu-id="64f98-564">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="64f98-565">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="64f98-566">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="64f98-567">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="64f98-568">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="64f98-569">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="64f98-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="64f98-570">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="64f98-570">The `TestController`:</span></span>

* <span data-ttu-id="64f98-571">Applica il `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione di `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="64f98-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="64f98-572">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="64f98-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="64f98-573">Passando a `https://localhost:5001/Test/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="64f98-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="64f98-574">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="64f98-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="64f98-575">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="64f98-575">Overriding the default order</span></span>

<span data-ttu-id="64f98-576">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="64f98-577">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="64f98-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="64f98-578">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="64f98-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="64f98-579">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="64f98-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="64f98-580">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="64f98-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="64f98-581">La proprietà `Order` può essere impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="64f98-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="64f98-582">Prendere in considerazione gli stessi tre filtri di azione illustrati nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="64f98-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="64f98-583">Se la proprietà `Order` del controller e dei filtri globali è impostata, rispettivamente, su 1 e su 2, l'ordine di esecuzione viene invertito.</span><span class="sxs-lookup"><span data-stu-id="64f98-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="64f98-584">Sequence</span><span class="sxs-lookup"><span data-stu-id="64f98-584">Sequence</span></span> | <span data-ttu-id="64f98-585">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-585">Filter scope</span></span> | <span data-ttu-id="64f98-586">Proprietà`Order`</span><span class="sxs-lookup"><span data-stu-id="64f98-586">`Order` property</span></span> | <span data-ttu-id="64f98-587">Filter - metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="64f98-588">1</span><span class="sxs-lookup"><span data-stu-id="64f98-588">1</span></span> | <span data-ttu-id="64f98-589">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-589">Method</span></span> | <span data-ttu-id="64f98-590">0</span><span class="sxs-lookup"><span data-stu-id="64f98-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="64f98-591">2</span><span class="sxs-lookup"><span data-stu-id="64f98-591">2</span></span> | <span data-ttu-id="64f98-592">Controller</span><span class="sxs-lookup"><span data-stu-id="64f98-592">Controller</span></span> | <span data-ttu-id="64f98-593">1</span><span class="sxs-lookup"><span data-stu-id="64f98-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="64f98-594">3\.</span><span class="sxs-lookup"><span data-stu-id="64f98-594">3</span></span> | <span data-ttu-id="64f98-595">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-595">Global</span></span> | <span data-ttu-id="64f98-596">2</span><span class="sxs-lookup"><span data-stu-id="64f98-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="64f98-597">4</span><span class="sxs-lookup"><span data-stu-id="64f98-597">4</span></span> | <span data-ttu-id="64f98-598">Global</span><span class="sxs-lookup"><span data-stu-id="64f98-598">Global</span></span> | <span data-ttu-id="64f98-599">2</span><span class="sxs-lookup"><span data-stu-id="64f98-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="64f98-600">5</span><span class="sxs-lookup"><span data-stu-id="64f98-600">5</span></span> | <span data-ttu-id="64f98-601">Controller</span><span class="sxs-lookup"><span data-stu-id="64f98-601">Controller</span></span> | <span data-ttu-id="64f98-602">1</span><span class="sxs-lookup"><span data-stu-id="64f98-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="64f98-603">6</span><span class="sxs-lookup"><span data-stu-id="64f98-603">6</span></span> | <span data-ttu-id="64f98-604">Metodo</span><span class="sxs-lookup"><span data-stu-id="64f98-604">Method</span></span> | <span data-ttu-id="64f98-605">0</span><span class="sxs-lookup"><span data-stu-id="64f98-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="64f98-606">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="64f98-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="64f98-607">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="64f98-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="64f98-608">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="64f98-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="64f98-609">Per i filtri predefiniti, l'ambito determina l'ordine, a meno che non si imposti `Order` su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="64f98-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="64f98-610">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="64f98-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="64f98-611">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="64f98-612">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="64f98-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-613">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="64f98-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="64f98-614">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="64f98-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="64f98-615">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="64f98-616">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="64f98-617">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="64f98-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="64f98-618">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="64f98-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="64f98-619">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="64f98-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="64f98-620">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="64f98-620">Dependency injection</span></span>

<span data-ttu-id="64f98-621">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="64f98-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="64f98-622">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="64f98-623">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="64f98-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="64f98-624">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="64f98-624">A type-activated filter means:</span></span>

* <span data-ttu-id="64f98-625">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="64f98-625">An instance is created for each request.</span></span>
* <span data-ttu-id="64f98-626">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64f98-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="64f98-627">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64f98-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="64f98-628">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="64f98-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="64f98-629">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="64f98-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="64f98-630">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="64f98-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="64f98-631">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="64f98-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="64f98-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="64f98-633">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="64f98-634">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-634">Loggers are available from DI.</span></span> <span data-ttu-id="64f98-635">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="64f98-636">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="64f98-637">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="64f98-637">Logging added to filters:</span></span>

* <span data-ttu-id="64f98-638">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="64f98-639">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="64f98-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="64f98-640">I filtri predefiniti registrano azioni ed eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="64f98-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="64f98-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="64f98-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="64f98-642">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64f98-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="64f98-643"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="64f98-644">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="64f98-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="64f98-645">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="64f98-646">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="64f98-647">Quando si usa l'impostazione `ServiceFilterAttribute`, [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="64f98-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="64f98-648">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="64f98-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="64f98-649">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="64f98-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="64f98-650">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="64f98-651">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="64f98-652">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="64f98-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="64f98-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="64f98-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="64f98-654">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="64f98-655">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="64f98-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="64f98-656">TypeFilterAttribute</span></span>

<span data-ttu-id="64f98-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="64f98-658">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="64f98-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="64f98-659">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="64f98-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="64f98-660">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="64f98-661">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="64f98-662">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="64f98-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="64f98-663">Quando si usa l'impostazione `TypeFilterAttribute`, [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="64f98-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="64f98-664">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="64f98-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="64f98-665">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="64f98-666">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="64f98-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="64f98-667">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="64f98-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="64f98-668">Filtri di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="64f98-668">Authorization filters</span></span>

<span data-ttu-id="64f98-669">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="64f98-669">Authorization filters:</span></span>

* <span data-ttu-id="64f98-670">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="64f98-671">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-671">Control access to action methods.</span></span>
* <span data-ttu-id="64f98-672">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="64f98-673">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="64f98-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="64f98-674">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="64f98-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="64f98-675">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="64f98-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="64f98-676">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-676">Calls the authorization system.</span></span>
* <span data-ttu-id="64f98-677">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="64f98-677">Does not authorize requests.</span></span>

<span data-ttu-id="64f98-678">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="64f98-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="64f98-679">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="64f98-679">The exception will not be handled.</span></span>
* <span data-ttu-id="64f98-680">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="64f98-681">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="64f98-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="64f98-682">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="64f98-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="64f98-683">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="64f98-683">Resource filters</span></span>

<span data-ttu-id="64f98-684">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="64f98-684">Resource filters:</span></span>

* <span data-ttu-id="64f98-685">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="64f98-686">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="64f98-687">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="64f98-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="64f98-688">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="64f98-689">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="64f98-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="64f98-690">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="64f98-690">Resource filter examples:</span></span>

* <span data-ttu-id="64f98-691">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="64f98-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="64f98-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="64f98-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="64f98-693">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="64f98-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="64f98-694">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="64f98-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="64f98-695">Filtri azioni</span><span class="sxs-lookup"><span data-stu-id="64f98-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64f98-696">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="64f98-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="64f98-697">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="64f98-698">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="64f98-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="64f98-699">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-699">Action filters:</span></span>

* <span data-ttu-id="64f98-700">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="64f98-701">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="64f98-702">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="64f98-703">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="64f98-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: consente di leggere gli input per un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="64f98-705"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="64f98-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="64f98-706"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="64f98-707">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="64f98-708">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="64f98-709">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="64f98-710">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="64f98-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="64f98-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="64f98-713">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="64f98-713">Setting this property to null:</span></span>

  * <span data-ttu-id="64f98-714">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="64f98-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="64f98-715">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="64f98-716">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="64f98-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="64f98-717">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="64f98-718">Restituisce un valore `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="64f98-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="64f98-719">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="64f98-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="64f98-720">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="64f98-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="64f98-721">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="64f98-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="64f98-722">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="64f98-722">Validate model state.</span></span>
* <span data-ttu-id="64f98-723">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="64f98-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="64f98-724">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="64f98-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="64f98-725">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="64f98-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="64f98-726">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="64f98-727">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="64f98-728">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="64f98-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="64f98-729">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="64f98-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="64f98-730">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="64f98-731">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="64f98-731">Exception filters</span></span>

<span data-ttu-id="64f98-732">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-732">Exception filters:</span></span>

* <span data-ttu-id="64f98-733">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="64f98-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="64f98-734">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="64f98-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="64f98-735">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="64f98-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="64f98-736">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-736">Exception filters:</span></span>

* <span data-ttu-id="64f98-737">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-737">Don't have before and after events.</span></span>
* <span data-ttu-id="64f98-738">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64f98-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="64f98-739">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="64f98-740">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="64f98-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="64f98-741">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="64f98-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="64f98-742">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-742">This stops propagation of the exception.</span></span> <span data-ttu-id="64f98-743">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="64f98-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="64f98-744">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-744">Only an action filter can do that.</span></span>

<span data-ttu-id="64f98-745">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="64f98-745">Exception filters:</span></span>

* <span data-ttu-id="64f98-746">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="64f98-747">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="64f98-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="64f98-748">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="64f98-749">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="64f98-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="64f98-750">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="64f98-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="64f98-751">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="64f98-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="64f98-752">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="64f98-752">Result filters</span></span>

<span data-ttu-id="64f98-753">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="64f98-753">Result filters:</span></span>

* <span data-ttu-id="64f98-754">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="64f98-754">Implement an interface:</span></span>
  * <span data-ttu-id="64f98-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="64f98-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="64f98-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="64f98-757">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="64f98-758">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="64f98-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="64f98-759">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="64f98-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="64f98-760">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="64f98-761">Un'azione che restituisce una visualizzazione include tutte le elaborazioni Razor come parte dell'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> eseguito.</span><span class="sxs-lookup"><span data-stu-id="64f98-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="64f98-762">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="64f98-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="64f98-763">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="64f98-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="64f98-764">I filtri dei risultati vengono eseguiti solo quando un filtro azione o azione produce un risultato di azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="64f98-765">I filtri dei risultati non vengono eseguiti nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f98-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="64f98-766">Un filtro di autorizzazione o un filtro risorse cortocircui la pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="64f98-767">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="64f98-768">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="64f98-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="64f98-769">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="64f98-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="64f98-770">La generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="64f98-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="64f98-771">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="64f98-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="64f98-772">È considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="64f98-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="64f98-773">Quando viene eseguito il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, è probabile che la risposta sia già stata inviata al client.</span><span class="sxs-lookup"><span data-stu-id="64f98-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="64f98-774">Se la risposta è già stata inviata al client, non è possibile modificarla ulteriormente.</span><span class="sxs-lookup"><span data-stu-id="64f98-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="64f98-775">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="64f98-776">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="64f98-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="64f98-777">Se si imposta `Exception` su Null l'eccezione viene gestita in modo efficace e si evita che venga generata nuovamente da ASP.NET Core in un punto successivo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="64f98-778">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="64f98-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="64f98-779">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="64f98-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="64f98-780">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="64f98-781">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="64f98-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="64f98-782">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="64f98-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="64f98-783">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="64f98-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="64f98-784">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="64f98-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="64f98-785">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="64f98-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="64f98-786">Sono inclusi i risultati dell'azione prodotti da:</span><span class="sxs-lookup"><span data-stu-id="64f98-786">This includes action results produced by:</span></span>

* <span data-ttu-id="64f98-787">Filtri di autorizzazione e filtri delle risorse che interessano il cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="64f98-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="64f98-788">Filtri eccezioni.</span><span class="sxs-lookup"><span data-stu-id="64f98-788">Exception filters.</span></span>

<span data-ttu-id="64f98-789">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="64f98-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="64f98-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="64f98-790">IFilterFactory</span></span>

<span data-ttu-id="64f98-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="64f98-792">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="64f98-793">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="64f98-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="64f98-794">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="64f98-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="64f98-795">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="64f98-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="64f98-796">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="64f98-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="64f98-797">Il codice precedente può essere testato eseguendo l'[esempio scaricato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="64f98-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="64f98-798">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="64f98-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="64f98-799">Passare a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="64f98-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="64f98-800">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="64f98-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="64f98-801">**Autore:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="64f98-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="64f98-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="64f98-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="64f98-803">**interno:** `My header`</span><span class="sxs-lookup"><span data-stu-id="64f98-803">**internal:** `My header`</span></span>

<span data-ttu-id="64f98-804">Il codice precedente crea l'intestazione di risposta **Internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="64f98-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="64f98-805">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="64f98-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="64f98-806">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="64f98-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="64f98-807">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="64f98-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="64f98-808">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="64f98-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="64f98-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="64f98-810">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="64f98-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="64f98-811">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="64f98-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="64f98-812">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="64f98-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="64f98-813">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="64f98-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="64f98-814">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="64f98-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="64f98-815">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="64f98-816">I filtri sono diversi dal middleware in quanto fanno parte del runtime di ASP.NET Core, quindi hanno accesso al contesto e ai costrutti di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64f98-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="64f98-817">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="64f98-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="64f98-818">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="64f98-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="64f98-819">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="64f98-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="64f98-820">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="64f98-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="64f98-821">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="64f98-821">Next actions</span></span>

* <span data-ttu-id="64f98-822">Vedere [metodi di filtro per Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="64f98-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="64f98-823">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="64f98-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
