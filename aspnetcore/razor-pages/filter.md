---
title: Modalità di filtro per Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Informazioni su come creare metodi di filtro per Razor Pages in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 1da46c61617a01698e3c4b1fe6bf9825db6643fd
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589942"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Modalità di filtro per Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

I filtri di Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) consentono a Razor Pages di eseguire il codice prima e dopo l'esecuzione di un gestore Razor Pages. I filtri di Razor Pages sono simili ai [filtri azione di ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), con la differenza che non possono essere applicati a metodi del gestore pagina singola. 

I filtri di Razor Pages:

* Eseguono il codice dopo aver selezionato un metodo del gestore, ma prima che si verifichi l'associazione di modelli.
* Eseguono il codice prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.
* Eseguono il codice dopo l'esecuzione del metodo del gestore.
* Possono essere implementati in una pagina o a livello globale.
* Non possono essere applicati a metodi del gestore pagina specifici.

È possibile eseguire il codice prima che venga eseguito il metodo del gestore usando il costruttore pagina o il middleware, ma solo i filtri di Razor Pages hanno accesso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). I filtri contengono un parametro derivato da [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0), che fornisce accesso a `HttpContext`. Ad esempio, nell'esempio [Implementare un attributo di filtro](#ifa) viene aggiunta un'intestazione alla risposta, operazione impossibile con costruttori o middleware.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([procedura per il download](xref:index#how-to-download-a-sample))

I filtri di Razor Pages mettono a disposizione i metodi seguenti, che possono essere applicati globalmente o a livello di pagina:

* Metodi sincroni:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): metodo chiamato dopo la selezione di un metodo del gestore, ma prima che si verifichi l'associazione di modelli.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): metodo chiamato prima che il metodo del gestore venga eseguito, al termine dell'associazione di modelli.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): metodo chiamato dopo che il metodo del gestore è stato eseguito e prima del risultato dell'azione.

* Metodi asincroni:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono dopo la selezione del metodo del gestore, ma prima che si verifichi l'associazione di modelli.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): metodo chiamato in modo asincrono prima che venga richiamato il metodo del gestore, al termine dell'associazione di modelli.

> [!NOTE]
> Implementare la versione sincrona **o** la versione asincrona di un'interfaccia di filtro, non entrambe. Il framework controlla per prima cosa se il filtro implementa l'interfaccia asincrona e, in tal caso, la chiama. In caso contrario, chiama i metodi dell'interfaccia sincrona. Se entrambe le interfacce sono implementate, verranno chiamati solo i metodi asincroni. La stessa regola vale per gli override nelle pagine. Implementare la versione sincrona o la versione asincrona dell'override, non entrambe.

## <a name="implement-razor-page-filters-globally"></a>Implementare i filtri di Razor Pages a livello globale

Il codice seguente implementa `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Nel codice precedente, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) non è obbligatorio. Nell'esempio viene utilizzato per fornire informazioni di traccia per l'applicazione.

Il codice seguente abilita `SampleAsyncPageFilter` nella classe `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Il codice seguente mostra la classe `Startup` completa:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Il codice seguente chiama `AddFolderApplicationModelConvention` per applicare `SampleAsyncPageFilter` solo alle pagine in */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Il codice seguente implementa il filtro `IPageFilter` sincrono:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Il codice seguente abilita `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementare i filtri di Razor Pages eseguendo l'override dei metodi di filtro

Il codice seguente esegue l'override dei filtri sincroni di Razor Pages:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Implementare un attributo di filtro

Il filtro basato su attributi predefinito [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) può essere sottoclassato. Il filtro seguente aggiunge un'intestazione alla risposta:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Il codice seguente applica l'attributo `AddHeader`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Vedere [Override dell'ordine predefinito](xref:mvc/controllers/filters#overriding-the-default-order) per istruzioni su come eseguire l'override dell'ordine.

Vedere [Annullamento e blocco](xref:mvc/controllers/filters#cancellation-and-short-circuiting) per istruzioni su come bloccare la pipeline filtro da un filtro. 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Autorizzare l'attributo di filtro

L'attributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) può essere applicato a `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
