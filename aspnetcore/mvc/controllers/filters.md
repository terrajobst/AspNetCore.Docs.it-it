---
title: Filtri in ASP.NET Core
author: ardalis
description: Informazioni sul funzionamento dei filtri e su come usarli in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: cdf121b97396cb23103d49cd141b9ef19b8c0cc6
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223028"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="f7d88-103">Filtri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7d88-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="f7d88-104">Di [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f7d88-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f7d88-105">I *filtri* in ASP.NET Core consentono l'esecuzione del codice prima o dopo fasi specifiche della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="f7d88-106">I filtri predefiniti gestiscono attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f7d88-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="f7d88-107">Autorizzazione (impedire l'accesso alle risorse per cui un utente non è autorizzato).</span><span class="sxs-lookup"><span data-stu-id="f7d88-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="f7d88-108">Memorizzazione nella cache delle risposte (blocco della pipeline delle richieste per restituire una risposta memorizzata nella cache).</span><span class="sxs-lookup"><span data-stu-id="f7d88-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="f7d88-109">I filtri personalizzati possono essere creati per gestire problemi relativi a più settori.</span><span class="sxs-lookup"><span data-stu-id="f7d88-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="f7d88-110">Esempi di problematiche trasversali includono la gestione degli errori, la memorizzazione nella cache, la configurazione, l'autorizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="f7d88-111">I filtri evitano la duplicazione del codice.</span><span class="sxs-lookup"><span data-stu-id="f7d88-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="f7d88-112">Ad esempio, un filtro eccezioni per la gestione degli errori potrebbe consolidare la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="f7d88-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="f7d88-113">Questo documento si applica a Razor Pages, controller API e controller con visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="f7d88-114">[Visualizzare o scaricare un esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f7d88-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="f7d88-115">Funzionamento dei filtri</span><span class="sxs-lookup"><span data-stu-id="f7d88-115">How filters work</span></span>

<span data-ttu-id="f7d88-116">I filtri vengono eseguiti all'interno della *pipeline di chiamata di azioni ASP.NET Core*, talvolta definita *pipeline di filtro*.</span><span class="sxs-lookup"><span data-stu-id="f7d88-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="f7d88-117">La pipeline di filtro viene eseguita dopo che ASP.NET Core ha selezionato l'azione da eseguire.</span><span class="sxs-lookup"><span data-stu-id="f7d88-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La richiesta viene elaborata tramite altro middleware, il middleware di routing, la selezione dell'azione e la pipeline di chiamata di azioni ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="f7d88-120">Tipi di filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-120">Filter types</span></span>

<span data-ttu-id="f7d88-121">Ogni tipo di filtro viene eseguito in una fase diversa della pipeline di filtro:</span><span class="sxs-lookup"><span data-stu-id="f7d88-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="f7d88-122">I [filtri di autorizzazione](#authorization-filters) vengono eseguiti per primi e consentono di determinare se l'utente è autorizzato per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="f7d88-123">I filtri di autorizzazione causano il corto circuito della pipeline se la richiesta non è autorizzata.</span><span class="sxs-lookup"><span data-stu-id="f7d88-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="f7d88-124">[Filtri di risorse](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="f7d88-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="f7d88-125">Vengono eseguiti dopo l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-125">Run after authorization.</span></span>  
  * <span data-ttu-id="f7d88-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> può eseguire codice prima del resto della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="f7d88-127">Ad esempio, `OnResourceExecuting` può eseguire codice prima dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="f7d88-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="f7d88-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> può eseguire codice dopo che il resto della pipeline è stato completato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="f7d88-129">I [filtri azione](#action-filters) possono eseguire codice immediatamente prima e dopo la chiamata di un metodo di azione singolo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="f7d88-130">Possono essere usati per modificare gli argomenti passati a un'azione e il risultato restituito dall'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="f7d88-131">I filtri di azione **non** sono supportati in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f7d88-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="f7d88-132">I [filtri eccezioni](#exception-filters) vengono usati per applicare i criteri globali a eccezioni non gestite che si verificano prima che qualsiasi elemento sia stato scritto nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="f7d88-133">I [filtri risultato](#result-filters) possono eseguire codice immediatamente prima e dopo l'esecuzione di risultati di azioni singole.</span><span class="sxs-lookup"><span data-stu-id="f7d88-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="f7d88-134">Vengono eseguiti solo quando il metodo di azione è stato eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="f7d88-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="f7d88-135">Sono utili per la logica che deve racchiudere l'esecuzione del formattatore o di una vista.</span><span class="sxs-lookup"><span data-stu-id="f7d88-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="f7d88-136">Il diagramma seguente illustra come interagiscono i tipi di filtro nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La richiesta passa attraverso i filtri autorizzazione, i filtri risorse, l'associazione di modelli, i filtri azione, l'esecuzione dell'azione e la conversione del risultato dell'azione, i filtri eccezioni, i filtri risultato e l'esecuzione del risultato.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="f7d88-139">Implementazione</span><span class="sxs-lookup"><span data-stu-id="f7d88-139">Implementation</span></span>

<span data-ttu-id="f7d88-140">I filtri supportano sia implementazioni sincrone che asincrone tramite definizioni di interfaccia diverse.</span><span class="sxs-lookup"><span data-stu-id="f7d88-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="f7d88-141">I filtri sincroni possono eseguire codice prima (`On-Stage-Executing`) e dopo (`On-Stage-Executed`) la relativa fase della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="f7d88-142">La chiamata di `OnActionExecuting`, ad esempio, avviene prima della chiamata del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="f7d88-143">La chiamata di `OnActionExecuted` avviene dopo l'esecuzione del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f7d88-144">I filtri asincroni definiscono un metodo `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="f7d88-145">Nel codice precedente `SampleAsyncActionFilter` ha un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) che esegue il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="f7d88-146">Ognuno dei metodi `On-Stage-ExecutionAsync` accetta un oggetto `FilterType-ExecutionDelegate` che esegue la fase della pipeline del filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="f7d88-147">Fasi di filtro multiple</span><span class="sxs-lookup"><span data-stu-id="f7d88-147">Multiple filter stages</span></span>

<span data-ttu-id="f7d88-148">È possibile implementare interfacce per più fasi di filtro in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="f7d88-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="f7d88-149">Ad esempio, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` e i relativi equivalenti asincroni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="f7d88-150">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="f7d88-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="f7d88-151">Il runtime controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="f7d88-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="f7d88-152">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="f7d88-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="f7d88-153">Se in una classe sono implementate entrambe le interfacce, sincrona e asincrona, viene chiamato solo il metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="f7d88-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="f7d88-154">Quando si usano classi astratte come <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> eseguire l'override solo dei metodi sincroni o del metodo asincrono per ogni tipo di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="f7d88-155">Attributi filtro predefiniti</span><span class="sxs-lookup"><span data-stu-id="f7d88-155">Built-in filter attributes</span></span>

<span data-ttu-id="f7d88-156">ASP.NET Core include filtri predefiniti basati su attributi che è possibile impostare come sottoclasse e personalizzare.</span><span class="sxs-lookup"><span data-stu-id="f7d88-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="f7d88-157">Il filtro dei risultati seguente, ad esempio, aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="f7d88-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="f7d88-158">Gli attributi consentono ai filtri di accettare argomenti, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="f7d88-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="f7d88-159">Applicare `AddHeaderAttribute` a un controller o a un metodo di azione e specificare il nome e il valore dell'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="f7d88-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="f7d88-160">Molte delle interfacce del filtro presentano attributi corrispondenti che possono essere usati come classi di base per implementazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="f7d88-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="f7d88-161">Attributi dei filtri:</span><span class="sxs-lookup"><span data-stu-id="f7d88-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="f7d88-162">Ambiti dei filtri e ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="f7d88-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="f7d88-163">È possibile aggiungere un filtro alla pipeline in uno dei tre *ambiti*:</span><span class="sxs-lookup"><span data-stu-id="f7d88-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="f7d88-164">Usando un attributo di un'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="f7d88-165">Usando un attributo di un controller.</span><span class="sxs-lookup"><span data-stu-id="f7d88-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="f7d88-166">A livello globale per tutti i controller e le azioni come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f7d88-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="f7d88-167">Il codice precedente aggiunge tre filtri a livello globale tramite la raccolta [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="f7d88-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="f7d88-168">Ordine di esecuzione predefinito</span><span class="sxs-lookup"><span data-stu-id="f7d88-168">Default order of execution</span></span>

<span data-ttu-id="f7d88-169">Quando sono presenti più filtri per una particolare fase della pipeline, l'ambito determina l'ordine di esecuzione predefinito dei filtri.</span><span class="sxs-lookup"><span data-stu-id="f7d88-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="f7d88-170">I filtri globali racchiudono i filtri di classe, che a loro volta racchiudono i filtri dei metodi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="f7d88-171">Come risultato dell'annidamento dei filtri, il codice *after* dei filtri viene eseguito in ordine inverso rispetto al codice *before*.</span><span class="sxs-lookup"><span data-stu-id="f7d88-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="f7d88-172">Sequenza di filtro:</span><span class="sxs-lookup"><span data-stu-id="f7d88-172">The filter sequence:</span></span>

* <span data-ttu-id="f7d88-173">Codice *before* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="f7d88-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="f7d88-174">Codice *before* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="f7d88-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="f7d88-175">Codice *before* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="f7d88-176">Codice *after* dei filtri del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="f7d88-177">Codice *after* dei filtri del controller.</span><span class="sxs-lookup"><span data-stu-id="f7d88-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="f7d88-178">Codice *after* dei filtri globali.</span><span class="sxs-lookup"><span data-stu-id="f7d88-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="f7d88-179">L'esempio seguente illustra l'ordine in cui i metodi dei filtri vengono chiamati per i filtri di azione sincroni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="f7d88-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="f7d88-180">Sequence</span></span> | <span data-ttu-id="f7d88-181">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-181">Filter scope</span></span> | <span data-ttu-id="f7d88-182">Metodo del filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="f7d88-183">1</span><span class="sxs-lookup"><span data-stu-id="f7d88-183">1</span></span> | <span data-ttu-id="f7d88-184">Global</span><span class="sxs-lookup"><span data-stu-id="f7d88-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f7d88-185">2</span><span class="sxs-lookup"><span data-stu-id="f7d88-185">2</span></span> | <span data-ttu-id="f7d88-186">Controller</span><span class="sxs-lookup"><span data-stu-id="f7d88-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f7d88-187">3</span><span class="sxs-lookup"><span data-stu-id="f7d88-187">3</span></span> | <span data-ttu-id="f7d88-188">Metodo</span><span class="sxs-lookup"><span data-stu-id="f7d88-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f7d88-189">4</span><span class="sxs-lookup"><span data-stu-id="f7d88-189">4</span></span> | <span data-ttu-id="f7d88-190">Metodo</span><span class="sxs-lookup"><span data-stu-id="f7d88-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f7d88-191">5</span><span class="sxs-lookup"><span data-stu-id="f7d88-191">5</span></span> | <span data-ttu-id="f7d88-192">Controller</span><span class="sxs-lookup"><span data-stu-id="f7d88-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f7d88-193">6</span><span class="sxs-lookup"><span data-stu-id="f7d88-193">6</span></span> | <span data-ttu-id="f7d88-194">Global</span><span class="sxs-lookup"><span data-stu-id="f7d88-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="f7d88-195">Questa sequenza mostra che:</span><span class="sxs-lookup"><span data-stu-id="f7d88-195">This sequence shows:</span></span>

* <span data-ttu-id="f7d88-196">Il filtro del metodo è annidato all'interno del filtro del controller.</span><span class="sxs-lookup"><span data-stu-id="f7d88-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="f7d88-197">Il filtro del controller è annidato all'interno del filtro globale.</span><span class="sxs-lookup"><span data-stu-id="f7d88-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="f7d88-198">Filtri a livello di controller e pagina Razor</span><span class="sxs-lookup"><span data-stu-id="f7d88-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="f7d88-199">Ogni controller che eredita dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller> include i metodi [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) e [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="f7d88-200">Questi metodi:</span><span class="sxs-lookup"><span data-stu-id="f7d88-200">These methods:</span></span>

* <span data-ttu-id="f7d88-201">Eseguono il wrapping dei filtri che vengono eseguiti per una determinata azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="f7d88-202">La chiamata di `OnActionExecuting` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="f7d88-203">La chiamata di `OnActionExecuted` avviene dopo tutti i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="f7d88-204">La chiamata di `OnActionExecutionAsync` avviene prima di qualsiasi filtro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="f7d88-205">Il codice del filtro dopo `next` viene eseguito dopo il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="f7d88-206">Ad esempio, l'applicazione di `MySampleActionFilter` nell'esempio scaricato avviene a livello globale all'avvio.</span><span class="sxs-lookup"><span data-stu-id="f7d88-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="f7d88-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-207">The `TestController`:</span></span>

* <span data-ttu-id="f7d88-208">Applica `SampleActionFilterAttribute` (`[SampleActionFilter]`) all'azione `FilterTest2`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="f7d88-209">Esegue l'override di `OnActionExecuting` e `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="f7d88-210">Passando a `https://localhost:5001/Test/FilterTest2`, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f7d88-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="f7d88-211">Per Razor Pages, vedere [Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="f7d88-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="f7d88-212">Override dell'ordine predefinito</span><span class="sxs-lookup"><span data-stu-id="f7d88-212">Overriding the default order</span></span>

<span data-ttu-id="f7d88-213">È possibile eseguire l'override della sequenza di esecuzione predefinita implementando <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="f7d88-214">`IOrderedFilter` espone la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, che ha la precedenza sull'ambito per determinare l'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="f7d88-215">Un filtro con un valore di `Order` inferiore:</span><span class="sxs-lookup"><span data-stu-id="f7d88-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="f7d88-216">Esegue il codice *before* prima di quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="f7d88-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="f7d88-217">Esegue il codice *after* dopo quello di un filtro con un valore di `Order` più alto.</span><span class="sxs-lookup"><span data-stu-id="f7d88-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="f7d88-218">La proprietà `Order` può essere impostata con un parametro del costruttore:</span><span class="sxs-lookup"><span data-stu-id="f7d88-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="f7d88-219">Prendere in considerazione gli stessi tre filtri di azione illustrati nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="f7d88-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="f7d88-220">Se la proprietà `Order` del controller e dei filtri globali è impostata, rispettivamente, su 1 e su 2, l'ordine di esecuzione viene invertito.</span><span class="sxs-lookup"><span data-stu-id="f7d88-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="f7d88-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="f7d88-221">Sequence</span></span> | <span data-ttu-id="f7d88-222">Ambito del filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-222">Filter scope</span></span> | <span data-ttu-id="f7d88-223">Proprietà`Order` </span><span class="sxs-lookup"><span data-stu-id="f7d88-223">`Order` property</span></span> | <span data-ttu-id="f7d88-224">Metodo del filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="f7d88-225">1</span><span class="sxs-lookup"><span data-stu-id="f7d88-225">1</span></span> | <span data-ttu-id="f7d88-226">Metodo</span><span class="sxs-lookup"><span data-stu-id="f7d88-226">Method</span></span> | <span data-ttu-id="f7d88-227">0</span><span class="sxs-lookup"><span data-stu-id="f7d88-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f7d88-228">2</span><span class="sxs-lookup"><span data-stu-id="f7d88-228">2</span></span> | <span data-ttu-id="f7d88-229">Controller</span><span class="sxs-lookup"><span data-stu-id="f7d88-229">Controller</span></span> | <span data-ttu-id="f7d88-230">1</span><span class="sxs-lookup"><span data-stu-id="f7d88-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="f7d88-231">3</span><span class="sxs-lookup"><span data-stu-id="f7d88-231">3</span></span> | <span data-ttu-id="f7d88-232">Global</span><span class="sxs-lookup"><span data-stu-id="f7d88-232">Global</span></span> | <span data-ttu-id="f7d88-233">2</span><span class="sxs-lookup"><span data-stu-id="f7d88-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="f7d88-234">4</span><span class="sxs-lookup"><span data-stu-id="f7d88-234">4</span></span> | <span data-ttu-id="f7d88-235">Global</span><span class="sxs-lookup"><span data-stu-id="f7d88-235">Global</span></span> | <span data-ttu-id="f7d88-236">2</span><span class="sxs-lookup"><span data-stu-id="f7d88-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="f7d88-237">5</span><span class="sxs-lookup"><span data-stu-id="f7d88-237">5</span></span> | <span data-ttu-id="f7d88-238">Controller</span><span class="sxs-lookup"><span data-stu-id="f7d88-238">Controller</span></span> | <span data-ttu-id="f7d88-239">1</span><span class="sxs-lookup"><span data-stu-id="f7d88-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="f7d88-240">6</span><span class="sxs-lookup"><span data-stu-id="f7d88-240">6</span></span> | <span data-ttu-id="f7d88-241">Metodo</span><span class="sxs-lookup"><span data-stu-id="f7d88-241">Method</span></span> | <span data-ttu-id="f7d88-242">0</span><span class="sxs-lookup"><span data-stu-id="f7d88-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="f7d88-243">La proprietà `Order` ha la precedenza sull'ambito nel determinare l'ordine di esecuzione dei filtri.</span><span class="sxs-lookup"><span data-stu-id="f7d88-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="f7d88-244">I filtri vengono ordinati prima in base all'ordine, poi viene usato l'ambito per interrompere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="f7d88-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="f7d88-245">Tutti i filtri predefiniti implementano `IOrderedFilter` e impostano il valore predefinito di `Order` su 0.</span><span class="sxs-lookup"><span data-stu-id="f7d88-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="f7d88-246">Per i filtri predefiniti, l'ambito determina l'ordine, a meno che non si imposti `Order` su un valore diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="f7d88-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="f7d88-247">Annullamento e corto circuito</span><span class="sxs-lookup"><span data-stu-id="f7d88-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="f7d88-248">È possibile causare il corto circuito della pipeline di filtro impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sul parametro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fornito al metodo di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="f7d88-249">Ad esempio, il filtro di risorse seguente impedisce l'esecuzione del resto della pipeline:</span><span class="sxs-lookup"><span data-stu-id="f7d88-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="f7d88-250">Nel codice seguente sia il filtro `ShortCircuitingResourceFilter` che il filtro `AddHeader` hanno come destinazione il metodo di azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="f7d88-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="f7d88-252">Viene eseguito per primo perché è un filtro risorsa e `AddHeader` è un filtro azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="f7d88-253">Blocca il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="f7d88-254">Pertanto il filtro `AddHeader` non viene mai eseguito per l'azione `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="f7d88-255">Questo comportamento sarebbe lo stesso se entrambi i filtri venissero applicati a livello di metodo di azione, a condizione che il filtro `ShortCircuitingResourceFilter` venga eseguito prima.</span><span class="sxs-lookup"><span data-stu-id="f7d88-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="f7d88-256">`ShortCircuitingResourceFilter` viene eseguito per primo per il tipo di filtro o per l'uso esplicito della proprietà `Order`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="f7d88-257">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="f7d88-257">Dependency injection</span></span>

<span data-ttu-id="f7d88-258">È possibile aggiungere filtri per tipo o per istanza.</span><span class="sxs-lookup"><span data-stu-id="f7d88-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="f7d88-259">Se viene aggiunta un'istanza, tale istanza viene usata per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="f7d88-260">Se viene aggiunto un tipo, il filtro è attivato dal tipo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="f7d88-261">Un filtro attivato dal tipo comporta:</span><span class="sxs-lookup"><span data-stu-id="f7d88-261">A type-activated filter means:</span></span>

* <span data-ttu-id="f7d88-262">La creazione di un'istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-262">An instance is created for each request.</span></span>
* <span data-ttu-id="f7d88-263">Il popolamento di tutte le dipendenze del costruttore tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7d88-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="f7d88-264">I filtri implementati come attributi e aggiunti direttamente alle classi controller o ai metodi di azione non possono avere dipendenze costruttore specificate dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7d88-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="f7d88-265">Le dipendenze del costruttore non possono essere fornite tramite inserimento delle dipendenze in quanto:</span><span class="sxs-lookup"><span data-stu-id="f7d88-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="f7d88-266">I parametri del costruttore devono essere forniti agli attributi nella posizione in cui vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="f7d88-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="f7d88-267">Questa è una limitazione del funzionamento degli attributi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="f7d88-268">I filtri seguenti supportano le dipendenze del costruttore fornite dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f7d88-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="f7d88-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementato nell'attributo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="f7d88-270">I filtri precedenti possono essere applicati a un controller o a un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="f7d88-271">I logger sono disponibili tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-271">Loggers are available from DI.</span></span> <span data-ttu-id="f7d88-272">Evitare tuttavia di creare e usare filtri esclusivamente per scopi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="f7d88-273">La funzionalità di [registrazione del framework predefinita](xref:fundamentals/logging/index) in genere fornisce il necessario per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="f7d88-274">La registrazione aggiunta ai filtri:</span><span class="sxs-lookup"><span data-stu-id="f7d88-274">Logging added to filters:</span></span>

* <span data-ttu-id="f7d88-275">Deve concentrarsi su problemi del dominio di business o comportamenti specifici del filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="f7d88-276">**Non** deve registrare azioni o altri eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="f7d88-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="f7d88-277">I filtri predefiniti registrano azioni ed eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="f7d88-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="f7d88-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f7d88-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="f7d88-279">I tipi di implementazione del filtro di servizi sono registrati in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="f7d88-280"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera un'istanza del filtro dall'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="f7d88-281">Il codice seguente illustra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f7d88-282">Nel codice seguente l'oggetto `AddHeaderResultServiceFilter` viene aggiunto al contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f7d88-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="f7d88-283">Nel codice seguente l'attributo `ServiceFilter` recupera un'istanza del filtro `AddHeaderResultServiceFilter` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f7d88-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="f7d88-284">[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="f7d88-284">[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="f7d88-285">Indica che l'istanza del filtro *può* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="f7d88-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f7d88-286">Il runtime di ASP.NET Core non garantisce:</span><span class="sxs-lookup"><span data-stu-id="f7d88-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="f7d88-287">Che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="f7d88-288">Che il filtro non verrà richiesto di nuovo dal contenitore di inserimento delle dipendenze in un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="f7d88-289">Non si deve usare con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="f7d88-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="f7d88-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f7d88-291">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f7d88-292">`CreateInstance` carica il tipo specificato dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="f7d88-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f7d88-293">TypeFilterAttribute</span></span>

<span data-ttu-id="f7d88-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> è simile a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ma il relativo tipo non viene risolto direttamente dal contenitore dell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="f7d88-295">Viene creata un'istanza del tipo tramite <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="f7d88-296">Poiché i tipi `TypeFilterAttribute` non vengono risolti direttamente dal contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f7d88-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="f7d88-297">Non è necessario che i tipi a cui viene fatto riferimento tramite `TypeFilterAttribute` siano registrati nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="f7d88-298">Le loro dipendenze vengono soddisfatte dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="f7d88-299">In via facoltativa, `TypeFilterAttribute` può anche accettare gli argomenti del costruttore per il tipo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="f7d88-300">Quando si usa `TypeFilterAttribute`, l'impostazione di `IsReusable` indica che l'istanza del filtro *potrebbe* essere riutilizzata all'esterno dell'ambito della richiesta in cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="f7d88-300">When using `TypeFilterAttribute`, setting `IsReusable` is a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f7d88-301">Il runtime di ASP.NET Core non fornisce alcuna garanzia che venga creata una singola istanza del filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-301">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span> <span data-ttu-id="f7d88-302">Non si deve usare `IsReusable` con un filtro che dipende da servizi con una durata diversa da singleton.</span><span class="sxs-lookup"><span data-stu-id="f7d88-302">`IsReusable` should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="f7d88-303">L'esempio seguente illustra come passare argomenti a un tipo usando `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-303">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="f7d88-304">Filtri autorizzazione</span><span class="sxs-lookup"><span data-stu-id="f7d88-304">Authorization filters</span></span>

<span data-ttu-id="f7d88-305">I filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-305">Authorization filters:</span></span>

* <span data-ttu-id="f7d88-306">Sono i primi filtri a essere eseguiti nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-306">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="f7d88-307">Controllano l'accesso ai metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-307">Control access to action methods.</span></span>
* <span data-ttu-id="f7d88-308">Dispongono di un metodo precedente, ma non di un metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-308">Have a before method, but no after method.</span></span>

<span data-ttu-id="f7d88-309">I filtri di autorizzazione personalizzati richiedono un framework di autorizzazione personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-309">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="f7d88-310">È preferibile configurare criteri di autorizzazione o scrivere criteri di autorizzazione personalizzati piuttosto che scrivere un filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-310">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="f7d88-311">Il filtro di autorizzazione predefinito:</span><span class="sxs-lookup"><span data-stu-id="f7d88-311">The built-in authorization filter:</span></span>

* <span data-ttu-id="f7d88-312">Chiama il sistema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-312">Calls the authorization system.</span></span>
* <span data-ttu-id="f7d88-313">Non autorizza le richieste.</span><span class="sxs-lookup"><span data-stu-id="f7d88-313">Does not authorize requests.</span></span>

<span data-ttu-id="f7d88-314">**Non** generare eccezioni nei filtri di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-314">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="f7d88-315">L'eccezione non verrà gestita.</span><span class="sxs-lookup"><span data-stu-id="f7d88-315">The exception will not be handled.</span></span>
* <span data-ttu-id="f7d88-316">I filtri di eccezione non gestiranno l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-316">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="f7d88-317">È consigliabile emettere una richiesta di verifica in caso di eccezione in un filtro di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-317">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="f7d88-318">Altre informazioni sull'[autorizzazione](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="f7d88-318">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="f7d88-319">Filtri risorse</span><span class="sxs-lookup"><span data-stu-id="f7d88-319">Resource filters</span></span>

<span data-ttu-id="f7d88-320">I filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="f7d88-320">Resource filters:</span></span>

* <span data-ttu-id="f7d88-321">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-321">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="f7d88-322">L'esecuzione racchiude la maggior parte della pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-322">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="f7d88-323">Solo i [filtri di autorizzazione](#authorization-filters) vengono eseguiti prima dei filtri di risorse.</span><span class="sxs-lookup"><span data-stu-id="f7d88-323">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="f7d88-324">I filtri di risorse sono utili per causare il corto circuito della maggior parte della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-324">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="f7d88-325">Un filtro di memorizzazione nella cache può, ad esempio, evitare il resto della pipeline in caso di riscontro nella cache.</span><span class="sxs-lookup"><span data-stu-id="f7d88-325">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="f7d88-326">Esempi di filtri di risorse:</span><span class="sxs-lookup"><span data-stu-id="f7d88-326">Resource filter examples:</span></span>

* <span data-ttu-id="f7d88-327">Il [filtro di risorse di corto circuito](#short-circuiting-resource-filter) illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f7d88-327">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="f7d88-328">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="f7d88-328">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="f7d88-329">Impedisce all'associazione di modelli di accedere ai dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-329">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="f7d88-330">Viene usato quando si caricano file di grandi dimensioni per impedire la lettura dei dati del modulo in memoria.</span><span class="sxs-lookup"><span data-stu-id="f7d88-330">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="f7d88-331">Filtri azione</span><span class="sxs-lookup"><span data-stu-id="f7d88-331">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7d88-332">I filtri azione **non** si applicano a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f7d88-332">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="f7d88-333">Razor Pages supporta <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-333">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="f7d88-334">Per altre informazioni, vedere [Modalità di filtro per pagine Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="f7d88-334">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="f7d88-335">I filtri di azione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-335">Action filters:</span></span>

* <span data-ttu-id="f7d88-336">Implementano l'interfaccia <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-336">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="f7d88-337">La loro esecuzione circonda l'esecuzione dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-337">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="f7d88-338">Il codice seguente mostra un filtro di azione di esempio:</span><span class="sxs-lookup"><span data-stu-id="f7d88-338">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f7d88-339">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> specifica le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="f7d88-339">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="f7d88-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: consente di leggere gli input per un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="f7d88-341"><xref:Microsoft.AspNetCore.Mvc.Controller>: consente di modificare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="f7d88-341"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="f7d88-342"><xref:System.Web.Mvc.ActionExecutingContext.Result>: l'impostazione di `Result` causa il corto circuito del metodo di azione e dei filtri di azione successivi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-342"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="f7d88-343">La generazione di un'eccezione in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-343">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="f7d88-344">Impedisce l'esecuzione dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-344">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="f7d88-345">A differenza dell'impostazione di `Result`, è considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-345">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f7d88-346">La classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> specifica `Controller` e `Result` oltre alle proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="f7d88-346">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="f7d88-347"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: true se un altro file ha causato il corto circuito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-347"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f7d88-348"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: non Null se l'azione o un filtro di azione eseguito in precedenza ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-348"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="f7d88-349">Se si imposta questa proprietà su Null:</span><span class="sxs-lookup"><span data-stu-id="f7d88-349">Setting this property to null:</span></span>

  * <span data-ttu-id="f7d88-350">L'eccezione viene gestita in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="f7d88-350">Effectively handles the exception.</span></span>
  * <span data-ttu-id="f7d88-351">L'oggetto `Result` viene eseguito come se fosse stato restituito dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-351">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="f7d88-352">Per un oggetto `IAsyncActionFilter`, una chiamata a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="f7d88-352">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="f7d88-353">Esegue qualsiasi filtro azione successivo e il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-353">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="f7d88-354">Restituisce `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-354">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="f7d88-355">Per causare il corto circuito, assegnare <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a un'istanza del risultato e non chiamare `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="f7d88-355">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="f7d88-356">Il framework fornisce un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="f7d88-356">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="f7d88-357">Il filtro di azione `OnActionExecuting` può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="f7d88-357">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="f7d88-358">Convalidare lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="f7d88-358">Validate model state.</span></span>
* <span data-ttu-id="f7d88-359">Restituire un errore se lo stato non è valido.</span><span class="sxs-lookup"><span data-stu-id="f7d88-359">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="f7d88-360">Il metodo `OnActionExecuted` viene eseguito dopo il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="f7d88-360">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="f7d88-361">È possibile visualizzare e modificare i risultati dell'azione tramite la proprietà <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-361">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="f7d88-362">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> è true se un altro filtro ha causato il corto circuito dell'esecuzione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-362"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f7d88-363">L'impostazione di <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> è un valore non Null se l'azione o un filtro di azione successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="f7d88-364">Impostazione di `Exception` su null:</span><span class="sxs-lookup"><span data-stu-id="f7d88-364">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="f7d88-365">Gestisce un'eccezione in modo efficace.</span><span class="sxs-lookup"><span data-stu-id="f7d88-365">Effectively handles an exception.</span></span>
  * <span data-ttu-id="f7d88-366">L'oggetto `ActionExecutedContext.Result` viene eseguito come se fosse stato restituito normalmente dal metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-366">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="f7d88-367">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="f7d88-367">Exception filters</span></span>

<span data-ttu-id="f7d88-368">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="f7d88-368">Exception filters:</span></span>

* <span data-ttu-id="f7d88-369">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-369">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="f7d88-370">Possono essere usati per implementare criteri di gestione degli errori comuni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-370">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="f7d88-371">Il filtro di eccezione di esempio seguente usa una visualizzazione degli errori personalizzata per mostrare i dettagli sulle eccezioni che si verificano quando l'app è in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="f7d88-371">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="f7d88-372">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="f7d88-372">Exception filters:</span></span>

* <span data-ttu-id="f7d88-373">Non dispone di eventi precedenti o successivi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-373">Don't have before and after events.</span></span>
* <span data-ttu-id="f7d88-374">Implementano <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-374">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="f7d88-375">Gestiscono le eccezioni non gestite che si verificano nella creazione di un controller o una pagina Razor, nell'[associazione di modelli](xref:mvc/models/model-binding), nei filtri di azione o nei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-375">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="f7d88-376">**Non** rilevano le eccezioni che si verificano nei filtri di risorse, nei filtri dei risultati o nell'esecuzione dei risultati MVC.</span><span class="sxs-lookup"><span data-stu-id="f7d88-376">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="f7d88-377">Per gestire un'eccezione, impostare la proprietà <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> su `true` o scrivere una risposta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-377">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="f7d88-378">In questo modo si arresta la propagazione dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-378">This stops propagation of the exception.</span></span> <span data-ttu-id="f7d88-379">Un filtro di eccezione non può modificare un'eccezione in una "operazione riuscita".</span><span class="sxs-lookup"><span data-stu-id="f7d88-379">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="f7d88-380">Questa operazione può essere eseguita solo tramite un filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-380">Only an action filter can do that.</span></span>

<span data-ttu-id="f7d88-381">Filtri eccezioni:</span><span class="sxs-lookup"><span data-stu-id="f7d88-381">Exception filters:</span></span>

* <span data-ttu-id="f7d88-382">Sono ideali per intercettare le eccezioni che si verificano nelle azioni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-382">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="f7d88-383">Non sono flessibili quanto il middleware di gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="f7d88-383">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="f7d88-384">Scegliere il middleware per la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="f7d88-384">Prefer middleware for exception handling.</span></span> <span data-ttu-id="f7d88-385">Usare i filtri di eccezione solo quando la gestione degli errori *varia* in base al metodo di azione chiamato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-385">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="f7d88-386">Un'app può ad esempio avere metodi di azione sia per gli endpoint API che per le visualizzazioni/HTML.</span><span class="sxs-lookup"><span data-stu-id="f7d88-386">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="f7d88-387">Gli endpoint dell'API possono restituire informazioni sull'errore come JSON, mentre le azioni basate sulla visualizzazione possono restituire una pagina di errore in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="f7d88-387">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="f7d88-388">Filtri risultato</span><span class="sxs-lookup"><span data-stu-id="f7d88-388">Result filters</span></span>

<span data-ttu-id="f7d88-389">I filtri dei risultati:</span><span class="sxs-lookup"><span data-stu-id="f7d88-389">Result filters:</span></span>

* <span data-ttu-id="f7d88-390">Implementare un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="f7d88-390">Implement an interface:</span></span>
  * <span data-ttu-id="f7d88-391"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="f7d88-391"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="f7d88-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="f7d88-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="f7d88-393">La loro esecuzione circonda l'esecuzione dei risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-393">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="f7d88-394">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="f7d88-394">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="f7d88-395">Il codice seguente illustra un filtro dei risultati che aggiunge un'intestazione HTTP:</span><span class="sxs-lookup"><span data-stu-id="f7d88-395">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f7d88-396">Il tipo di risultato eseguito dipende dall'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-396">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="f7d88-397">Un'azione che restituisce una visualizzazione include tutte le elaborazioni Razor come parte dell'elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> eseguito.</span><span class="sxs-lookup"><span data-stu-id="f7d88-397">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="f7d88-398">Un metodo API può eseguire la serializzazione in quanto parte dell'esecuzione del risultato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-398">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="f7d88-399">Altre informazioni sui [risultati dell'azione](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="f7d88-399">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="f7d88-400">I filtri risultato vengono eseguiti solo per i risultati corretti, ovvero quando l'azione o i filtri azione producono un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-400">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="f7d88-401">I filtri risultato non vengono eseguiti quando i filtri eccezioni gestiscono un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-401">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="f7d88-402">Il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> può causare il corto circuito dell'esecuzione del risultato dell'azione e dei filtri dei risultati successivi se si imposta <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> su `true`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-402">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="f7d88-403">In caso di corto circuito, scrivere nell'oggetto risposta per evitare di generare una risposta vuota.</span><span class="sxs-lookup"><span data-stu-id="f7d88-403">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="f7d88-404">La generazione di un'eccezione in `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-404">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="f7d88-405">Impedisce l'esecuzione del risultato dell'azione e dei filtri successivi.</span><span class="sxs-lookup"><span data-stu-id="f7d88-405">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="f7d88-406">È considerata un errore anziché un risultato positivo.</span><span class="sxs-lookup"><span data-stu-id="f7d88-406">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f7d88-407">Quando il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="f7d88-407">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="f7d88-408">È probabile che la risposta sia stata inviata al client e non possa essere più modificata.</span><span class="sxs-lookup"><span data-stu-id="f7d88-408">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="f7d88-409">Se è stata generata un'eccezione, il corpo della risposta non viene inviato.</span><span class="sxs-lookup"><span data-stu-id="f7d88-409">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="f7d88-410">L'impostazione di `ResultExecutedContext.Canceled` è `true` se un altro filtro ha causato il corto circuito dell'esecuzione del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-410">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="f7d88-411">L'impostazione di `ResultExecutedContext.Exception` è un valore non Null se il risultato dell'azione o un filtro dei risultati successivo ha generato un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-411">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="f7d88-412">Se si imposta `Exception` su Null l'eccezione viene gestita in modo efficace e si evita che venga generata nuovamente da ASP.NET Core in un punto successivo della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-412">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="f7d88-413">Non c'è alcun modo affidabile per scrivere i dati in una risposta quando si gestisce un'eccezione in un filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="f7d88-413">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="f7d88-414">Se le intestazioni sono state scaricate nel client quando il risultato di un'azione genera un'eccezione, non c'è alcun meccanismo affidabile per inviare un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="f7d88-414">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="f7d88-415">Per un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una chiamata a `await next` in <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> esegue tutti i filtri dei risultati successivi e il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-415">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="f7d88-416">Per causare il corto circuito, impostare [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) su `true` e non chiamare `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-416">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="f7d88-417">Il framework fornisce un oggetto `ResultFilterAttribute` astratto che è possibile impostare come sottoclasse.</span><span class="sxs-lookup"><span data-stu-id="f7d88-417">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="f7d88-418">La classe [AddHeaderAttribute](#add-header-attribute) illustrata in precedenza è un esempio di un attributo di filtro dei risultati.</span><span class="sxs-lookup"><span data-stu-id="f7d88-418">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="f7d88-419">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="f7d88-419">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="f7d88-420">Le interfacce <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> dichiarano un'implementazione di <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> eseguita per tutti i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-420">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="f7d88-421">Il filtro viene applicato a tutti i risultati dell'azione, a meno che:</span><span class="sxs-lookup"><span data-stu-id="f7d88-421">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="f7d88-422">Non venga applicato un oggetto <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> che causa il corto circuito della risposta.</span><span class="sxs-lookup"><span data-stu-id="f7d88-422">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="f7d88-423">Un filtro di eccezione non gestisca un'eccezione producendo un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f7d88-423">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="f7d88-424">I filtri diversi da `IExceptionFilter` e `IAuthorizationFilter` non causano il corto circuito di `IAlwaysRunResultFilter` e `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-424">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="f7d88-425">Ad esempio, il filtro seguente viene eseguito sempre e imposta un risultato dell'azione (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un codice di stato *422 Entità non elaborabile* quando la negoziazione del contenuto ha esito negativo:</span><span class="sxs-lookup"><span data-stu-id="f7d88-425">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="f7d88-426">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="f7d88-426">IFilterFactory</span></span>

<span data-ttu-id="f7d88-427"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-427"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="f7d88-428">Pertanto, un'istanza di `IFilterFactory` può essere usata come un'istanza di `IFilterMetadata` in un punto qualsiasi della pipeline filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-428">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="f7d88-429">Quando il runtime si prepara per richiamare il filtro, cerca di eseguirne il cast a un oggetto `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-429">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="f7d88-430">Se l'esecuzione del cast ha esito positivo, viene chiamato il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare l'istanza di `IFilterMetadata` richiamata.</span><span class="sxs-lookup"><span data-stu-id="f7d88-430">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="f7d88-431">In questo modo viene specificata una struttura flessibile, poiché non è necessario impostare in modo esplicito la pipeline filtro all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="f7d88-431">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="f7d88-432">Un altro approccio alla creazione di filtri consiste nell'implementare `IFilterFactory` usando implementazioni dell'attributo personalizzate:</span><span class="sxs-lookup"><span data-stu-id="f7d88-432">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="f7d88-433">Il codice precedente può essere testato eseguendo l'[esempio scaricato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="f7d88-433">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="f7d88-434">Richiamare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="f7d88-434">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="f7d88-435">Passare a `https://localhost:5001/Sample/HeaderWithFactory`</span><span class="sxs-lookup"><span data-stu-id="f7d88-435">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="f7d88-436">Gli strumenti di sviluppo F12 visualizzano le intestazioni di risposta seguenti aggiunte dal codice di esempio:</span><span class="sxs-lookup"><span data-stu-id="f7d88-436">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="f7d88-437">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="f7d88-437">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="f7d88-438">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="f7d88-438">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="f7d88-439">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="f7d88-439">**internal:** `My header`</span></span>

<span data-ttu-id="f7d88-440">Il codice precedente crea l'intestazione di risposta **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-440">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="f7d88-441">Implementazione di IFilterFactory in un attributo</span><span class="sxs-lookup"><span data-stu-id="f7d88-441">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="f7d88-442">I filtri che implementano `IFilterFactory` sono utili per i filtri che:</span><span class="sxs-lookup"><span data-stu-id="f7d88-442">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="f7d88-443">Non richiedono il passaggio di parametri.</span><span class="sxs-lookup"><span data-stu-id="f7d88-443">Don't require passing parameters.</span></span>
* <span data-ttu-id="f7d88-444">Hanno dipendenze del costruttore che devono essere soddisfatte dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-444">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="f7d88-445"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-445"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f7d88-446">`IFilterFactory` espone il metodo <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> per creare un'istanza <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="f7d88-446">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f7d88-447">`CreateInstance` carica il tipo specificato dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f7d88-447">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="f7d88-448">Il codice seguente illustra tre approcci all'applicazione di `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="f7d88-448">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="f7d88-449">Nel codice precedente la decorazione del metodo con `[SampleActionFilter]` è l'approccio da preferire per applicare `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="f7d88-449">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="f7d88-450">Uso di middleware nella pipeline filtro</span><span class="sxs-lookup"><span data-stu-id="f7d88-450">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="f7d88-451">I filtri risorse funzionano come [middleware](xref:fundamentals/middleware/index) in quanto racchiudono l'esecuzione di tutto ciò che viene dopo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-451">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="f7d88-452">I filtri sono diversi dal middleware in quanto fanno parte del runtime di ASP.NET Core, quindi hanno accesso al contesto e ai costrutti di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7d88-452">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="f7d88-453">Per usare il middleware come filtro, creare un tipo con un metodo `Configure` che specifica il middleware da inserire nella pipeline di filtro.</span><span class="sxs-lookup"><span data-stu-id="f7d88-453">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="f7d88-454">L'esempio seguente usa il middleware di localizzazione per stabilire le impostazioni cultura correnti per una richiesta:</span><span class="sxs-lookup"><span data-stu-id="f7d88-454">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="f7d88-455">Usare <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> per eseguire il middleware:</span><span class="sxs-lookup"><span data-stu-id="f7d88-455">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="f7d88-456">I filtri middleware vengono eseguiti nella stessa fase della pipeline filtro come filtri risorse, prima dell'associazione di modelli e dopo il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="f7d88-456">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="f7d88-457">Azioni successive</span><span class="sxs-lookup"><span data-stu-id="f7d88-457">Next actions</span></span>

* <span data-ttu-id="f7d88-458">Vedere [Modalità di filtro per Razor Pages](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="f7d88-458">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="f7d88-459">Per sperimentare i filtri, [scaricare, testare e modificare l'esempio di GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="f7d88-459">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
