---
title: Tipi restituiti per le azioni del controller nell'API Web di ASP.NET Core
author: scottaddie
description: Informazioni sull'uso dei vari tipi restituiti dal metodo per le azioni del controller nell'API Web di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 180d76c2c2e53dbf64b8fcc5cdc6d2b6f4dab6eb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887116"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="75b0b-103">Tipi restituiti per le azioni del controller nell'API Web di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75b0b-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="75b0b-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="75b0b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="75b0b-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75b0b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="75b0b-106">ASP.NET Core offre le seguenti opzioni per i tipi restituiti per le azioni del controller nell'API Web:</span><span class="sxs-lookup"><span data-stu-id="75b0b-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="75b0b-107">Tipo specifico</span><span class="sxs-lookup"><span data-stu-id="75b0b-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="75b0b-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="75b0b-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="75b0b-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="75b0b-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="75b0b-110">Tipo specifico</span><span class="sxs-lookup"><span data-stu-id="75b0b-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="75b0b-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="75b0b-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="75b0b-112">Questo documento spiega quando è più appropriato utilizzare ogni tipo restituito.</span><span class="sxs-lookup"><span data-stu-id="75b0b-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="75b0b-113">Tipo specifico</span><span class="sxs-lookup"><span data-stu-id="75b0b-113">Specific type</span></span>

<span data-ttu-id="75b0b-114">L'azione più semplice restituisce un tipo di dati primitivo o complesso, ad esempio, `string` o un tipo di oggetto personalizzato.</span><span class="sxs-lookup"><span data-stu-id="75b0b-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="75b0b-115">Si consideri l'azione seguente, che restituisce una raccolta di oggetti `Product` personalizzati:</span><span class="sxs-lookup"><span data-stu-id="75b0b-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="75b0b-116">In assenza di condizioni note da cui proteggersi durante l'esecuzione dell'azione, la restituzione di un tipo specifico potrebbe essere sufficiente.</span><span class="sxs-lookup"><span data-stu-id="75b0b-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="75b0b-117">L'azione precedente non accetta parametri, quindi non è necessario eseguire la convalida dei vincoli dei parametri.</span><span class="sxs-lookup"><span data-stu-id="75b0b-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="75b0b-118">Quando per un'azione devono essere prese in considerazione condizioni note, vengono introdotti più percorsi di ritorno.</span><span class="sxs-lookup"><span data-stu-id="75b0b-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="75b0b-119">In tal caso, è comune combinare un tipo restituito [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) al tipo restituito primitivo o complesso.</span><span class="sxs-lookup"><span data-stu-id="75b0b-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="75b0b-120">Ai fini di questo tipo di azione, è necessario usare [IActionResult](#iactionresult-type) oppure [ActionResult\<T >](#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="75b0b-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="75b0b-121">Tipo IActionResult</span><span class="sxs-lookup"><span data-stu-id="75b0b-121">IActionResult type</span></span>

<span data-ttu-id="75b0b-122">Il tipo restituito [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) è appropriato quando un'azione supporta più tipi restituiti [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="75b0b-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="75b0b-123">I tipi `ActionResult` rappresentano vari codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="75b0b-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="75b0b-124">Alcuni tipi restituiti comuni che rientrano in questa categoria sono [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) e [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="75b0b-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="75b0b-125">Dal momento che nell'azione sono presenti più percorsi e tipi restituiti, è necessario usare spesso l'attributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor).</span><span class="sxs-lookup"><span data-stu-id="75b0b-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="75b0b-126">Questo attributo genera dettagli più descrittivi sulla risposta per le pagine della Guida di API generate da strumenti quali [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="75b0b-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="75b0b-127">`[ProducesResponseType]` indica i tipi noti e i codici di stato HTTP che l'azione deve restituire.</span><span class="sxs-lookup"><span data-stu-id="75b0b-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="75b0b-128">Azione sincrona</span><span class="sxs-lookup"><span data-stu-id="75b0b-128">Synchronous action</span></span>

<span data-ttu-id="75b0b-129">Si consideri la seguente azione sincrona che prevede due possibili tipi restituiti:</span><span class="sxs-lookup"><span data-stu-id="75b0b-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="75b0b-130">Nell'azione precedente, viene restituito un codice di stato 404 quando il prodotto rappresentato da `id` non esiste nell'archivio dati sottostante.</span><span class="sxs-lookup"><span data-stu-id="75b0b-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="75b0b-131">Il metodo helper [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) viene richiamato come collegamento a `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="75b0b-132">Se il prodotto esiste, viene restituito un oggetto `Product` che rappresenta il payload con il codice di stato 200.</span><span class="sxs-lookup"><span data-stu-id="75b0b-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="75b0b-133">Il metodo helper [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) viene richiamato come forma abbreviata di `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="75b0b-134">Azione asincrona</span><span class="sxs-lookup"><span data-stu-id="75b0b-134">Asynchronous action</span></span>

<span data-ttu-id="75b0b-135">Si consideri la seguente azione asincrona che prevede due possibili tipi restituiti:</span><span class="sxs-lookup"><span data-stu-id="75b0b-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="75b0b-136">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="75b0b-136">In the preceding code:</span></span>

* <span data-ttu-id="75b0b-137">Un codice di stato 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) viene restituito dal runtime di ASP.NET Core quando la descrizione del prodotto contiene "XYZ Widget".</span><span class="sxs-lookup"><span data-stu-id="75b0b-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="75b0b-138">Un codice di stato 201 viene generato dal metodo [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) quando viene creato un prodotto.</span><span class="sxs-lookup"><span data-stu-id="75b0b-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="75b0b-139">In questo percorso del codice viene restituito l'oggetto `Product`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="75b0b-140">Ad esempio, il modello seguente indica che le richieste devono includere le proprietà `Name` e `Description`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="75b0b-141">Pertanto, se nella richiesta non vengono forniti `Name` e `Description`, la convalida del modello avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="75b0b-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="75b0b-142">Se viene applicato l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) in ASP.NET Core 2.1 o versioni successive, gli errori di convalida del modello generano un codice di stato 400.</span><span class="sxs-lookup"><span data-stu-id="75b0b-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="75b0b-143">Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="75b0b-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="75b0b-144">Tipo ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="75b0b-144">ActionResult\<T> type</span></span>

<span data-ttu-id="75b0b-145">ASP.NET Core 2.1 introduce il tipo restituito [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) per le azioni del controller API Web.</span><span class="sxs-lookup"><span data-stu-id="75b0b-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="75b0b-146">Consente di restituire un tipo che deriva da [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) o di restituire un [tipo specifico](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="75b0b-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="75b0b-147">`ActionResult<T>` offre i vantaggi seguenti rispetto al [tipo IActionResult](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="75b0b-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="75b0b-148">La proprietà `Type` dell'attributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) può essere esclusa.</span><span class="sxs-lookup"><span data-stu-id="75b0b-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="75b0b-149">Ad esempio, `[ProducesResponseType(200, Type = typeof(Product))]` viene semplificato in `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="75b0b-150">Il tipo restituito previsto dell'azione viene invece dedotto da `T` in `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="75b0b-151">[Operatori di cast impliciti](/dotnet/csharp/language-reference/keywords/implicit) supportano la conversione di `T` e `ActionResult` in `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="75b0b-152">`T` viene convertito in [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), il che significa che `return new ObjectResult(T);` viene semplificato in `return T;`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="75b0b-153">C# non supporta operatori di cast impliciti sulle interfacce.</span><span class="sxs-lookup"><span data-stu-id="75b0b-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="75b0b-154">Di conseguenza, per la conversione dell'interfaccia in un tipo concreto è necessario usare `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="75b0b-155">Ad esempio, usare `IEnumerable` nell'esempio seguente non funziona:</span><span class="sxs-lookup"><span data-stu-id="75b0b-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="75b0b-156">Una delle opzioni disponibili per correggere il codice precedente consiste nel restituire `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="75b0b-157">La maggior parte delle azioni presenta un tipo restituito specifico.</span><span class="sxs-lookup"><span data-stu-id="75b0b-157">Most actions have a specific return type.</span></span> <span data-ttu-id="75b0b-158">Durante l'esecuzione di un'azione possono verificarsi condizioni impreviste, nel qual caso il tipo specifico non viene restituito.</span><span class="sxs-lookup"><span data-stu-id="75b0b-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="75b0b-159">Ad esempio, il parametro di input di un'azione potrebbe non superare la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="75b0b-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="75b0b-160">In tal caso, è consueto che venga restituito il tipo `ActionResult` appropriato anziché il tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="75b0b-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="75b0b-161">Azione sincrona</span><span class="sxs-lookup"><span data-stu-id="75b0b-161">Synchronous action</span></span>

<span data-ttu-id="75b0b-162">Si consideri un'azione sincrona che prevede due possibili tipi restituiti:</span><span class="sxs-lookup"><span data-stu-id="75b0b-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="75b0b-163">Nel codice precedente, viene restituito un codice di stato 404 quando il prodotto non esiste nel database.</span><span class="sxs-lookup"><span data-stu-id="75b0b-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="75b0b-164">Se il prodotto esiste, viene restituito l'oggetto `Product` corrispondente.</span><span class="sxs-lookup"><span data-stu-id="75b0b-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="75b0b-165">Prima di ASP.NET Core 2.1, la riga `return product;` sarebbe stata `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="75b0b-166">A partire da ASP.NET Core 2.1, è attiva l'inferenza per l'origine di associazione del parametro di azione quando una classe controller è decorata con l'attributo`[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="75b0b-167">Un nome di parametro corrispondente a un nome del modello di route viene associato automaticamente usando i dati della route della richiesta.</span><span class="sxs-lookup"><span data-stu-id="75b0b-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="75b0b-168">Di conseguenza, il parametro `id` dell'azione precedente non è annotato in modo esplicito con l'attributo [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).</span><span class="sxs-lookup"><span data-stu-id="75b0b-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="75b0b-169">Azione asincrona</span><span class="sxs-lookup"><span data-stu-id="75b0b-169">Asynchronous action</span></span>

<span data-ttu-id="75b0b-170">Si consideri un'azione asincrona che prevede due possibili tipi restituiti:</span><span class="sxs-lookup"><span data-stu-id="75b0b-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="75b0b-171">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="75b0b-171">In the preceding code:</span></span>

* <span data-ttu-id="75b0b-172">Un codice di stato 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) viene restituito dal runtime di ASP.NET Core quando:</span><span class="sxs-lookup"><span data-stu-id="75b0b-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="75b0b-173">È stato applicato l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) e la convalida del modello ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="75b0b-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="75b0b-174">La descrizione del prodotto contiene "XYZ Widget".</span><span class="sxs-lookup"><span data-stu-id="75b0b-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="75b0b-175">Un codice di stato 201 viene generato dal metodo [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) quando viene creato un prodotto.</span><span class="sxs-lookup"><span data-stu-id="75b0b-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="75b0b-176">In questo percorso del codice viene restituito l'oggetto `Product`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="75b0b-177">A partire da ASP.NET Core 2.1, è attiva l'inferenza per l'origine di associazione del parametro di azione quando una classe controller è decorata con l'attributo`[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="75b0b-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="75b0b-178">I parametri di tipo complesso vengono associati automaticamente tramite il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="75b0b-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="75b0b-179">Di conseguenza, il parametro `product` dell'azione precedente non è annotato in modo esplicito con l'attributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).</span><span class="sxs-lookup"><span data-stu-id="75b0b-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="75b0b-180">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="75b0b-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
