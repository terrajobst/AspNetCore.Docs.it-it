---
title: Filtri in ASP.NET Core
author: ardalis
description: Informazioni sul funzionamento dei filtri e su come usarli in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 50b199744f32ad19335080da406db69665ec1ae9
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856152"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="7ae82-103">Filtri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ae82-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="7ae82-104">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7ae82-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7ae82-105">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="7ae82-106">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7ae82-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="7ae82-107">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="7ae82-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="7ae82-108">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="7ae82-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="7ae82-109">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="7ae82-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="7ae82-110">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="7ae82-111">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="7ae82-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="7ae82-112">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="7ae82-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="7ae82-113">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="7ae82-114">[Visualizzare o scaricare un esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7ae82-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="7ae82-115">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="7ae82-115">How filters work</span></span>

<span data-ttu-id="7ae82-116">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="7ae82-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="7ae82-117">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="7ae82-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altro middleware, il middleware di routing, la selezione dell'azione e la pipeline di chiamata di azioni ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="7ae82-120">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-120">Filter types</span></span>

<span data-ttu-id="7ae82-121">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="7ae82-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="7ae82-122">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="7ae82-123">I filtri di autorizzazione causano il corto circuito della pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="7ae82-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="7ae82-124">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="7ae82-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="7ae82-125">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-125">Run after authorization.</span></span>  
  * <span data-ttu-id="7ae82-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> può eseguire codice prima del resto della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="7ae82-127">Ad esempio, `OnResourceExecuting` può eseguire codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="7ae82-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="7ae82-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> può eseguire codice dopo che il resto della pipeline è stato completato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="7ae82-129">I [filtri azione](#action-filters) possono eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singolo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="7ae82-130">Possono essere usati per modificare gli argomenti passati a un'azione e il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="7ae82-131">I filtri di azione **non** sono supportati in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7ae82-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="7ae82-132">I [filtri eccezioni](#exception-filters) vengono usati per applicare i criteri globali a eccezioni non gestite che si verificano prima che qualsiasi elemento sia stato scritto nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="7ae82-133">I [filtri risultato](#result-filters) possono eseguire codice immediatamente prima e dopo l'esecuzione di risultati di azioni singole.</span><span class="sxs-lookup"><span data-stu-id="7ae82-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="7ae82-134">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="7ae82-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="7ae82-135">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="7ae82-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="7ae82-136">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="7ae82-139">Implementazione</span><span class="sxs-lookup"><span data-stu-id="7ae82-139">Implementation</span></span>

<span data-ttu-id="7ae82-140">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="7ae82-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="7ae82-141">I filtri sincroni possono eseguire codice prima (`On-Stage-Executing`) e dopo (`On-Stage-Executed`) la relativa fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="7ae82-142">La chiamata di `OnActionExecuting`, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="7ae82-143">La chiamata di `OnActionExecuted` avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="7ae82-144">I filtri asincroni definiscono un metodo `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="7ae82-145">Nel codice precedente `SampleAsyncActionFilter` ha un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="7ae82-146">Ognuno dei metodi `On-Stage-ExecutionAsync` accetta un oggetto `FilterType-ExecutionDelegate` che esegue la fase della pipeline del filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="7ae82-147">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="7ae82-147">Multiple filter stages</span></span>

<span data-ttu-id="7ae82-148">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="7ae82-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="7ae82-149">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` e i relativi equivalenti asincroni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="7ae82-150">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="7ae82-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="7ae82-151">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="7ae82-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="7ae82-152">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="7ae82-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="7ae82-153">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7ae82-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="7ae82-154">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="7ae82-155">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="7ae82-155">Built-in filter attributes</span></span>

<span data-ttu-id="7ae82-156">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="7ae82-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="7ae82-157">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="7ae82-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="7ae82-158">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="7ae82-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="7ae82-159">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="7ae82-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="7ae82-160">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="7ae82-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="7ae82-161">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="7ae82-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="7ae82-162">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="7ae82-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="7ae82-163">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="7ae82-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="7ae82-164">Usando un attributo di un'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="7ae82-165">Usando un attributo di un controller.</span><span class="sxs-lookup"><span data-stu-id="7ae82-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="7ae82-166">A livello globale per tutti i controller e le azioni come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7ae82-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="7ae82-167">Il codice precedente aggiunge tre filtri a livello globale tramite la raccolta [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="7ae82-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="7ae82-168">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="7ae82-168">Default order of execution</span></span>

<span data-ttu-id="7ae82-169">Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione predefinito dei filtri.</span><span class="sxs-lookup"><span data-stu-id="7ae82-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="7ae82-170">I filtri globali racchiudono i filtri di classe, che a loro volta racchiudono i filtri dei metodi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="7ae82-171">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="7ae82-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="7ae82-172">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="7ae82-172">The filter sequence:</span></span>

* <span data-ttu-id="7ae82-173">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="7ae82-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="7ae82-174">Codice *before* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="7ae82-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="7ae82-175">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="7ae82-176">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="7ae82-177">Codice *after* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="7ae82-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="7ae82-178">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="7ae82-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="7ae82-179">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="7ae82-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="7ae82-180">Sequence</span></span> | <span data-ttu-id="7ae82-181">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-181">Filter scope</span></span> | <span data-ttu-id="7ae82-182">Metodo del filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="7ae82-183">1</span><span class="sxs-lookup"><span data-stu-id="7ae82-183">1</span></span> | <span data-ttu-id="7ae82-184">Global</span><span class="sxs-lookup"><span data-stu-id="7ae82-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="7ae82-185">2</span><span class="sxs-lookup"><span data-stu-id="7ae82-185">2</span></span> | <span data-ttu-id="7ae82-186">Controller</span><span class="sxs-lookup"><span data-stu-id="7ae82-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="7ae82-187">3</span><span class="sxs-lookup"><span data-stu-id="7ae82-187">3</span></span> | <span data-ttu-id="7ae82-188">Metodo</span><span class="sxs-lookup"><span data-stu-id="7ae82-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="7ae82-189">4</span><span class="sxs-lookup"><span data-stu-id="7ae82-189">4</span></span> | <span data-ttu-id="7ae82-190">Metodo</span><span class="sxs-lookup"><span data-stu-id="7ae82-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="7ae82-191">5</span><span class="sxs-lookup"><span data-stu-id="7ae82-191">5</span></span> | <span data-ttu-id="7ae82-192">Controller</span><span class="sxs-lookup"><span data-stu-id="7ae82-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="7ae82-193">6</span><span class="sxs-lookup"><span data-stu-id="7ae82-193">6</span></span> | <span data-ttu-id="7ae82-194">Global</span><span class="sxs-lookup"><span data-stu-id="7ae82-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="7ae82-195">Questa sequenza mostra che:</span><span class="sxs-lookup"><span data-stu-id="7ae82-195">This sequence shows:</span></span>

* <span data-ttu-id="7ae82-196">Il filtro del metodo è annidato all'interno del filtro del controller.</span><span class="sxs-lookup"><span data-stu-id="7ae82-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="7ae82-197">Il filtro del controller è annidato all'interno del filtro globale.</span><span class="sxs-lookup"><span data-stu-id="7ae82-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="7ae82-198">Filtri a livello di controller e pagina Razor</span><span class="sxs-lookup"><span data-stu-id="7ae82-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="7ae82-199">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="7ae82-200">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="7ae82-200">These methods:</span></span>

* <span data-ttu-id="7ae82-201">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="7ae82-202">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="7ae82-203">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="7ae82-204">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="7ae82-205">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="7ae82-206">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="7ae82-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="7ae82-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-207">The `TestController`:</span></span>

* <span data-ttu-id="7ae82-208">Applica `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione `FilterTest2`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="7ae82-209">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="7ae82-210">Passando a `https://localhost:5001/Test/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7ae82-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="7ae82-211">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="7ae82-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="7ae82-212">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="7ae82-212">Overriding the default order</span></span>

<span data-ttu-id="7ae82-213">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="7ae82-214">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="7ae82-215">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="7ae82-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="7ae82-216">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="7ae82-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="7ae82-217">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="7ae82-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="7ae82-218">La proprietà `Order` può essere impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="7ae82-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="7ae82-219">Prendere in considerazione gli stessi tre filtri di azione illustrati nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="7ae82-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="7ae82-220">Se la proprietà `Order` del controller e dei filtri globali è impostata, rispettivamente, su 1 e su 2, l'ordine di esecuzione viene invertito.</span><span class="sxs-lookup"><span data-stu-id="7ae82-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="7ae82-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="7ae82-221">Sequence</span></span> | <span data-ttu-id="7ae82-222">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-222">Filter scope</span></span> | <span data-ttu-id="7ae82-223">Proprietà`Order`</span><span class="sxs-lookup"><span data-stu-id="7ae82-223">`Order` property</span></span> | <span data-ttu-id="7ae82-224">Metodo del filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="7ae82-225">1</span><span class="sxs-lookup"><span data-stu-id="7ae82-225">1</span></span> | <span data-ttu-id="7ae82-226">Metodo</span><span class="sxs-lookup"><span data-stu-id="7ae82-226">Method</span></span> | <span data-ttu-id="7ae82-227">0</span><span class="sxs-lookup"><span data-stu-id="7ae82-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="7ae82-228">2</span><span class="sxs-lookup"><span data-stu-id="7ae82-228">2</span></span> | <span data-ttu-id="7ae82-229">Controller</span><span class="sxs-lookup"><span data-stu-id="7ae82-229">Controller</span></span> | <span data-ttu-id="7ae82-230">1</span><span class="sxs-lookup"><span data-stu-id="7ae82-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="7ae82-231">3</span><span class="sxs-lookup"><span data-stu-id="7ae82-231">3</span></span> | <span data-ttu-id="7ae82-232">Global</span><span class="sxs-lookup"><span data-stu-id="7ae82-232">Global</span></span> | <span data-ttu-id="7ae82-233">2</span><span class="sxs-lookup"><span data-stu-id="7ae82-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="7ae82-234">4</span><span class="sxs-lookup"><span data-stu-id="7ae82-234">4</span></span> | <span data-ttu-id="7ae82-235">Global</span><span class="sxs-lookup"><span data-stu-id="7ae82-235">Global</span></span> | <span data-ttu-id="7ae82-236">2</span><span class="sxs-lookup"><span data-stu-id="7ae82-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="7ae82-237">5</span><span class="sxs-lookup"><span data-stu-id="7ae82-237">5</span></span> | <span data-ttu-id="7ae82-238">Controller</span><span class="sxs-lookup"><span data-stu-id="7ae82-238">Controller</span></span> | <span data-ttu-id="7ae82-239">1</span><span class="sxs-lookup"><span data-stu-id="7ae82-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="7ae82-240">6</span><span class="sxs-lookup"><span data-stu-id="7ae82-240">6</span></span> | <span data-ttu-id="7ae82-241">Metodo</span><span class="sxs-lookup"><span data-stu-id="7ae82-241">Method</span></span> | <span data-ttu-id="7ae82-242">0</span><span class="sxs-lookup"><span data-stu-id="7ae82-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="7ae82-243">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="7ae82-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="7ae82-244">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="7ae82-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="7ae82-245">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="7ae82-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="7ae82-246">Per i filtri predefiniti, l'ambito determina l'ordine, a meno che non si imposti `Order` su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="7ae82-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="7ae82-247">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="7ae82-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="7ae82-248">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="7ae82-249">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="7ae82-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="7ae82-250">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="7ae82-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="7ae82-252">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="7ae82-253">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="7ae82-254">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="7ae82-255">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="7ae82-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="7ae82-256">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="7ae82-257">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="7ae82-257">Dependency injection</span></span>

<span data-ttu-id="7ae82-258">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="7ae82-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="7ae82-259">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="7ae82-260">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="7ae82-261">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="7ae82-261">A type-activated filter means:</span></span>

* <span data-ttu-id="7ae82-262">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-262">An instance is created for each request.</span></span>
* <span data-ttu-id="7ae82-263">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7ae82-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="7ae82-264">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7ae82-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="7ae82-265">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="7ae82-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="7ae82-266">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="7ae82-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="7ae82-267">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="7ae82-268">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="7ae82-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="7ae82-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="7ae82-270">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="7ae82-271">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-271">Loggers are available from DI.</span></span> <span data-ttu-id="7ae82-272">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="7ae82-273">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="7ae82-274">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="7ae82-274">Logging added to filters:</span></span>

* <span data-ttu-id="7ae82-275">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="7ae82-276">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="7ae82-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="7ae82-277">I filtri predefiniti registrano azioni ed eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="7ae82-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="7ae82-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="7ae82-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="7ae82-279">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="7ae82-280"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="7ae82-281">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="7ae82-282">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="7ae82-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="7ae82-283">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="7ae82-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="7ae82-284">Quando si usa l'impostazione `ServiceFilterAttribute`, [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="7ae82-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="7ae82-285">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="7ae82-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="7ae82-286">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="7ae82-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="7ae82-287">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="7ae82-288">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="7ae82-289">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="7ae82-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="7ae82-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="7ae82-291">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="7ae82-292">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="7ae82-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="7ae82-293">TypeFilterAttribute</span></span>

<span data-ttu-id="7ae82-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="7ae82-295">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="7ae82-296">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="7ae82-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="7ae82-297">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="7ae82-298">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="7ae82-299">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="7ae82-300">Quando si usa l'impostazione `TypeFilterAttribute`, [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="7ae82-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="7ae82-301">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="7ae82-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="7ae82-302">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="7ae82-303">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="7ae82-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="7ae82-304">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="7ae82-305">Filtri autorizzazione</span><span class="sxs-lookup"><span data-stu-id="7ae82-305">Authorization filters</span></span>

<span data-ttu-id="7ae82-306">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-306">Authorization filters:</span></span>

* <span data-ttu-id="7ae82-307">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="7ae82-308">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-308">Control access to action methods.</span></span>
* <span data-ttu-id="7ae82-309">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="7ae82-310">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="7ae82-311">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="7ae82-312">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="7ae82-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="7ae82-313">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-313">Calls the authorization system.</span></span>
* <span data-ttu-id="7ae82-314">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="7ae82-314">Does not authorize requests.</span></span>

<span data-ttu-id="7ae82-315">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="7ae82-316">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="7ae82-316">The exception will not be handled.</span></span>
* <span data-ttu-id="7ae82-317">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="7ae82-318">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="7ae82-319">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="7ae82-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="7ae82-320">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="7ae82-320">Resource filters</span></span>

<span data-ttu-id="7ae82-321">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="7ae82-321">Resource filters:</span></span>

* <span data-ttu-id="7ae82-322">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="7ae82-323">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="7ae82-324">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="7ae82-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="7ae82-325">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="7ae82-326">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="7ae82-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="7ae82-327">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="7ae82-327">Resource filter examples:</span></span>

* <span data-ttu-id="7ae82-328">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7ae82-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="7ae82-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="7ae82-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="7ae82-330">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="7ae82-331">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="7ae82-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="7ae82-332">Filtri azione</span><span class="sxs-lookup"><span data-stu-id="7ae82-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ae82-333">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7ae82-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="7ae82-334">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="7ae82-335">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="7ae82-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="7ae82-336">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-336">Action filters:</span></span>

* <span data-ttu-id="7ae82-337">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="7ae82-338">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="7ae82-339">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="7ae82-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="7ae82-340">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="7ae82-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="7ae82-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: consente di leggere gli input per un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="7ae82-342"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="7ae82-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="7ae82-343"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="7ae82-344">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="7ae82-345">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="7ae82-346">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="7ae82-347">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="7ae82-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="7ae82-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="7ae82-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="7ae82-350">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="7ae82-350">Setting this property to null:</span></span>

  * <span data-ttu-id="7ae82-351">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="7ae82-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="7ae82-352">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="7ae82-353">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="7ae82-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="7ae82-354">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="7ae82-355">Restituisce `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="7ae82-356">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="7ae82-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="7ae82-357">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="7ae82-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="7ae82-358">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="7ae82-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="7ae82-359">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="7ae82-359">Validate model state.</span></span>
* <span data-ttu-id="7ae82-360">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="7ae82-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="7ae82-361">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="7ae82-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="7ae82-362">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="7ae82-363">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="7ae82-364">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="7ae82-365">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="7ae82-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="7ae82-366">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="7ae82-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="7ae82-367">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="7ae82-368">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="7ae82-368">Exception filters</span></span>

<span data-ttu-id="7ae82-369">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="7ae82-369">Exception filters:</span></span>

* <span data-ttu-id="7ae82-370">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="7ae82-371">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="7ae82-372">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="7ae82-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="7ae82-373">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="7ae82-373">Exception filters:</span></span>

* <span data-ttu-id="7ae82-374">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-374">Don't have before and after events.</span></span>
* <span data-ttu-id="7ae82-375">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="7ae82-376">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="7ae82-377">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="7ae82-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="7ae82-378">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="7ae82-379">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-379">This stops propagation of the exception.</span></span> <span data-ttu-id="7ae82-380">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="7ae82-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="7ae82-381">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-381">Only an action filter can do that.</span></span>

<span data-ttu-id="7ae82-382">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="7ae82-382">Exception filters:</span></span>

* <span data-ttu-id="7ae82-383">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="7ae82-384">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="7ae82-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="7ae82-385">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="7ae82-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="7ae82-386">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="7ae82-387">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="7ae82-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="7ae82-388">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="7ae82-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="7ae82-389">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="7ae82-389">Result filters</span></span>

<span data-ttu-id="7ae82-390">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="7ae82-390">Result filters:</span></span>

* <span data-ttu-id="7ae82-391">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="7ae82-391">Implement an interface:</span></span>
  * <span data-ttu-id="7ae82-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="7ae82-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="7ae82-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="7ae82-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="7ae82-394">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="7ae82-395">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="7ae82-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="7ae82-396">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="7ae82-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="7ae82-397">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="7ae82-398">Un'azione che restituisce una visualizzazione include tutte le elaborazioni Razor come parte dell'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> eseguito.</span><span class="sxs-lookup"><span data-stu-id="7ae82-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="7ae82-399">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="7ae82-400">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="7ae82-400">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="7ae82-401">I filtri risultato vengono eseguiti solo per i risultati corretti, ovvero quando l'azione o i filtri azione producono un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-401">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="7ae82-402">I filtri risultato non vengono eseguiti quando i filtri eccezioni gestiscono un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-402">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="7ae82-403">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-403">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="7ae82-404">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="7ae82-404">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="7ae82-405">La generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-405">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="7ae82-406">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="7ae82-406">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="7ae82-407">È considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="7ae82-407">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="7ae82-408">Quando il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="7ae82-408">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="7ae82-409">È probabile che la risposta sia stata inviata al client e non possa essere più modificata.</span><span class="sxs-lookup"><span data-stu-id="7ae82-409">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="7ae82-410">Se è stata generata un'eccezione, il corpo della risposta non viene inviato.</span><span class="sxs-lookup"><span data-stu-id="7ae82-410">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="7ae82-411">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-411">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="7ae82-412">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-412">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="7ae82-413">Se si imposta `Exception` su Null l'eccezione viene gestita in modo efficace e si evita che venga generata nuovamente da ASP.NET Core in un punto successivo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-413">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="7ae82-414">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="7ae82-414">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="7ae82-415">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="7ae82-415">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="7ae82-416">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-416">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="7ae82-417">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-417">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="7ae82-418">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="7ae82-418">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="7ae82-419">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="7ae82-419">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="7ae82-420">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="7ae82-420">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="7ae82-421">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-421">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="7ae82-422">Il filtro viene applicato a tutti i risultati dell'azione, a meno che:</span><span class="sxs-lookup"><span data-stu-id="7ae82-422">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="7ae82-423">Non venga applicato un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> che causa il corto circuito della risposta.</span><span class="sxs-lookup"><span data-stu-id="7ae82-423">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="7ae82-424">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="7ae82-424">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="7ae82-425">I filtri diversi da `IExceptionFilter` e `IAuthorizationFilter` non causano il corto circuito di `IAlwaysRunResultFilter` e `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-425">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="7ae82-426">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="7ae82-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="7ae82-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="7ae82-427">IFilterFactory</span></span>

<span data-ttu-id="7ae82-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="7ae82-429">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="7ae82-430">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="7ae82-431">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="7ae82-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="7ae82-432">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="7ae82-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="7ae82-433">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="7ae82-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="7ae82-434">Il codice precedente può essere testato eseguendo l'[esempio scaricato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="7ae82-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="7ae82-435">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="7ae82-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="7ae82-436">Passare a `https://localhost:5001/Sample/HeaderWithFactory`</span><span class="sxs-lookup"><span data-stu-id="7ae82-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="7ae82-437">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="7ae82-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="7ae82-438">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="7ae82-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="7ae82-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="7ae82-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="7ae82-440">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="7ae82-440">**internal:** `My header`</span></span>

<span data-ttu-id="7ae82-441">Il codice precedente crea l'intestazione di risposta **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="7ae82-442">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="7ae82-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="7ae82-443">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="7ae82-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="7ae82-444">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="7ae82-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="7ae82-445">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="7ae82-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="7ae82-447">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="7ae82-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="7ae82-448">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7ae82-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="7ae82-449">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="7ae82-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="7ae82-450">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="7ae82-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="7ae82-451">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="7ae82-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="7ae82-452">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="7ae82-453">I filtri sono diversi dal middleware in quanto fanno parte del runtime di ASP.NET Core, quindi hanno accesso al contesto e ai costrutti di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ae82-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="7ae82-454">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="7ae82-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="7ae82-455">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="7ae82-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="7ae82-456">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="7ae82-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="7ae82-457">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="7ae82-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="7ae82-458">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="7ae82-458">Next actions</span></span>

* <span data-ttu-id="7ae82-459">Vedere [Modalità di filtro per Razor Pages](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="7ae82-459">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="7ae82-460">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="7ae82-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
