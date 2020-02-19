---
title: Modalità di filtro per Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Informazioni su come creare metodi di filtro per Razor Pages in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: a60b17685c6f836de7c0afcc5b89a9894fb8b28f
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447231"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="2ce3d-103">Modalità di filtro per Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ce3d-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2ce3d-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ce3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ce3d-105">I filtri di Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) consentono a Razor Pages di eseguire il codice prima e dopo l'esecuzione di un gestore Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="2ce3d-106">I filtri di Razor Pages sono simili ai [filtri azione di ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), con la differenza che non possono essere applicati a metodi del gestore pagina singola.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="2ce3d-107">I filtri di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-107">Razor Page filters:</span></span>

* <span data-ttu-id="2ce3d-108">Eseguono il codice dopo aver selezionato un metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="2ce3d-109">Eseguono il codice prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="2ce3d-110">Eseguono il codice dopo l'esecuzione del metodo del gestore.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="2ce3d-111">Possono essere implementati in una pagina o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="2ce3d-112">Non possono essere applicati a metodi del gestore pagina specifici.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-112">Cannot be applied to specific page handler methods.</span></span>
* <span data-ttu-id="2ce3d-113">Le dipendenze del costruttore possono essere popolate dall' [inserimento di dipendenze](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="2ce3d-113">Can have constructor dependencies populated by [Dependency Injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="2ce3d-114">Per ulteriori informazioni, vedere [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) e [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span><span class="sxs-lookup"><span data-stu-id="2ce3d-114">For more information, see [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) and [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span></span>

<span data-ttu-id="2ce3d-115">Mentre i costruttori di pagina e il middleware abilitano l'esecuzione di codice personalizzato prima dell'esecuzione di un metodo del gestore, solo i filtri della pagina Razor consentono l'accesso a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> e alla pagina.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-115">While page constructors and middleware enable executing custom code before a handler method executes, only Razor Page filters enable access to <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> and the page.</span></span> <span data-ttu-id="2ce3d-116">Il middleware ha accesso alla `HttpContext`, ma non al "contesto di pagina".</span><span class="sxs-lookup"><span data-stu-id="2ce3d-116">Middleware has access to the `HttpContext`, but not to the "page context".</span></span> <span data-ttu-id="2ce3d-117">I filtri hanno un <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> parametro derivato, che consente l'accesso a `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-117">Filters have a <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="2ce3d-118">Ad esempio, nell'esempio [Implementare un attributo di filtro](#ifa) viene aggiunta un'intestazione alla risposta, operazione impossibile con costruttori o middleware.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-118">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="2ce3d-119">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ce3d-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2ce3d-120">I filtri di Razor Pages mettono a disposizione i metodi seguenti, che possono essere applicati globalmente o a livello di pagina:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-120">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="2ce3d-121">Metodi sincroni:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-121">Synchronous methods:</span></span>

  * <span data-ttu-id="2ce3d-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): metodo chiamato dopo la selezione di un metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="2ce3d-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): metodo chiamato prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="2ce3d-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): metodo chiamato dopo che il metodo del gestore è stato eseguito e prima del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="2ce3d-125">Metodi asincroni:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-125">Asynchronous methods:</span></span>

  * <span data-ttu-id="2ce3d-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono dopo la selezione del metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="2ce3d-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono prima che venga richiamato il metodo del gestore, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

<span data-ttu-id="2ce3d-128">Implementare la versione sincrona **oppure** la versione asincrona di un'interfaccia di filtro, **non** entrambe.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-128">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="2ce3d-129">Il framework controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-129">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="2ce3d-130">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-130">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="2ce3d-131">Se entrambe le interfacce sono implementate, verranno chiamati solo i metodi asincroni.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-131">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="2ce3d-132">La stessa regola vale per gli override nelle pagine. Implementare la versione sincrona o la versione asincrona dell'override, non entrambe.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-132">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="2ce3d-133">Implementare i filtri di Razor Pages a livello globale</span><span class="sxs-lookup"><span data-stu-id="2ce3d-133">Implement Razor Page filters globally</span></span>

<span data-ttu-id="2ce3d-134">Il codice seguente implementa `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-134">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="2ce3d-135">Nel codice precedente `ProcessUserAgent.Write` è il codice fornito dall'utente che funziona con la stringa agente utente.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-135">In the preceding code, `ProcessUserAgent.Write` is user supplied code that works with the user agent string.</span></span>

<span data-ttu-id="2ce3d-136">Il codice seguente abilita `SampleAsyncPageFilter` nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-136">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

<span data-ttu-id="2ce3d-137">Il codice seguente chiama <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> per applicare il `SampleAsyncPageFilter` solo alle pagine in */Movies*:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-137">The following code calls <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to apply the `SampleAsyncPageFilter` to only pages in */Movies*:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="2ce3d-138">Il codice seguente implementa il filtro `IPageFilter` sincrono:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-138">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="2ce3d-139">Il codice seguente abilita `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-139">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="2ce3d-140">Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-140">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="2ce3d-141">Il codice seguente esegue l'override dei filtri di pagine Razor asincrone:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-141">The following code overrides the asynchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="2ce3d-142">Implementare un attributo di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-142">Implement a filter attribute</span></span>

<span data-ttu-id="2ce3d-143">Il filtro predefinito basato su attributi <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filtro può essere sottoclassato.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-143">The built-in attribute-based filter <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filter can be subclassed.</span></span> <span data-ttu-id="2ce3d-144">Il filtro seguente aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="2ce3d-145">Il codice seguente applica l'attributo `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

<span data-ttu-id="2ce3d-146">Usare uno strumento come gli strumenti di sviluppo del browser per esaminare le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-146">Use a tool such as the browser developer tools to examine the headers.</span></span> <span data-ttu-id="2ce3d-147">In **intestazioni risposta**viene visualizzato `author: Rick`.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-147">Under **Response Headers**, `author: Rick` is displayed.</span></span>

<span data-ttu-id="2ce3d-148">Vedere [Override dell'ordine predefinito](xref:mvc/controllers/filters#overriding-the-default-order) per istruzioni su come eseguire l'override dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-148">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="2ce3d-149">Vedere [Annullamento e blocco](xref:mvc/controllers/filters#cancellation-and-short-circuiting) per istruzioni su come bloccare la pipeline filtro da un filtro.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-149">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span>

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="2ce3d-150">Autorizzare l'attributo di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-150">Authorize filter attribute</span></span>

<span data-ttu-id="2ce3d-151">L'attributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) può essere applicato a `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-151">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2ce3d-152">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ce3d-152">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ce3d-153">I filtri di Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) consentono a Razor Pages di eseguire il codice prima e dopo l'esecuzione di un gestore Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-153">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="2ce3d-154">I filtri di Razor Pages sono simili ai [filtri azione di ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), con la differenza che non possono essere applicati a metodi del gestore pagina singola.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-154">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="2ce3d-155">I filtri di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-155">Razor Page filters:</span></span>

* <span data-ttu-id="2ce3d-156">Eseguono il codice dopo aver selezionato un metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-156">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="2ce3d-157">Eseguono il codice prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-157">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="2ce3d-158">Eseguono il codice dopo l'esecuzione del metodo del gestore.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-158">Run code after the handler method executes.</span></span>
* <span data-ttu-id="2ce3d-159">Possono essere implementati in una pagina o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-159">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="2ce3d-160">Non possono essere applicati a metodi del gestore pagina specifici.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-160">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="2ce3d-161">È possibile eseguire il codice prima che venga eseguito il metodo del gestore usando il costruttore pagina o il middleware, ma solo i filtri di Razor Pages hanno accesso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="2ce3d-161">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="2ce3d-162">I filtri contengono un parametro derivato da [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0), che fornisce accesso a `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-162">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="2ce3d-163">Ad esempio, nell'esempio [Implementare un attributo di filtro](#ifa) viene aggiunta un'intestazione alla risposta, operazione impossibile con costruttori o middleware.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-163">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="2ce3d-164">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ce3d-164">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2ce3d-165">I filtri di Razor Pages mettono a disposizione i metodi seguenti, che possono essere applicati globalmente o a livello di pagina:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-165">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="2ce3d-166">Metodi sincroni:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-166">Synchronous methods:</span></span>

  * <span data-ttu-id="2ce3d-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): metodo chiamato dopo la selezione di un metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="2ce3d-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): metodo chiamato prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="2ce3d-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): metodo chiamato dopo che il metodo del gestore è stato eseguito e prima del risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="2ce3d-170">Metodi asincroni:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-170">Asynchronous methods:</span></span>

  * <span data-ttu-id="2ce3d-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono dopo la selezione del metodo del gestore, ma prima che si verifichi l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="2ce3d-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono prima che venga richiamato il metodo del gestore, al termine dell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="2ce3d-173">Implementare la versione sincrona **o** la versione asincrona di un'interfaccia di filtro, non entrambe.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-173">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="2ce3d-174">Il framework controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-174">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="2ce3d-175">In caso contrario, chiama i metodi dell'interfaccia sincrona.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-175">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="2ce3d-176">Se entrambe le interfacce sono implementate, verranno chiamati solo i metodi asincroni.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-176">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="2ce3d-177">La stessa regola vale per gli override nelle pagine. Implementare la versione sincrona o la versione asincrona dell'override, non entrambe.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-177">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="2ce3d-178">Implementare i filtri di Razor Pages a livello globale</span><span class="sxs-lookup"><span data-stu-id="2ce3d-178">Implement Razor Page filters globally</span></span>

<span data-ttu-id="2ce3d-179">Il codice seguente implementa `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-179">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="2ce3d-180">Nel codice precedente, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-180">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="2ce3d-181">Nell'esempio viene utilizzato per fornire informazioni di traccia per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-181">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="2ce3d-182">Il codice seguente abilita `SampleAsyncPageFilter` nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-182">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="2ce3d-183">Il codice seguente mostra la classe `Startup` completa:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-183">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="2ce3d-184">Il codice seguente chiama `AddFolderApplicationModelConvention` per applicare `SampleAsyncPageFilter` solo alle pagine in */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-184">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="2ce3d-185">Il codice seguente implementa il filtro `IPageFilter` sincrono:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-185">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="2ce3d-186">Il codice seguente abilita `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-186">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="2ce3d-187">Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-187">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="2ce3d-188">Il codice seguente esegue l'override dei filtri sincroni di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-188">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="2ce3d-189">Implementare un attributo di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-189">Implement a filter attribute</span></span>

<span data-ttu-id="2ce3d-190">Il filtro basato su attributi predefinito [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) può essere sottoclassato.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-190">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="2ce3d-191">Il filtro seguente aggiunge un'intestazione alla risposta:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-191">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="2ce3d-192">Il codice seguente applica l'attributo `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-192">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="2ce3d-193">Vedere [Override dell'ordine predefinito](xref:mvc/controllers/filters#overriding-the-default-order) per istruzioni su come eseguire l'override dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-193">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="2ce3d-194">Vedere [Annullamento e blocco](xref:mvc/controllers/filters#cancellation-and-short-circuiting) per istruzioni su come bloccare la pipeline filtro da un filtro.</span><span class="sxs-lookup"><span data-stu-id="2ce3d-194">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="2ce3d-195">Autorizzare l'attributo di filtro</span><span class="sxs-lookup"><span data-stu-id="2ce3d-195">Authorize filter attribute</span></span>

<span data-ttu-id="2ce3d-196">L'attributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) può essere applicato a `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="2ce3d-196">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
