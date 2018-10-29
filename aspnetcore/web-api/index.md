---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni sulle funzionalità disponibili per la creazione di un'API Web in ASP.NET Core e su quando è appropriato usare ciascuna funzionalità.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090420"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="47d56-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47d56-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="47d56-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="47d56-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="47d56-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47d56-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="47d56-106">Questo documento illustra come creare un'API Web in ASP.NET Core e indica quando è più appropriato usare ciascuna funzionalità.</span><span class="sxs-lookup"><span data-stu-id="47d56-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="47d56-107">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="47d56-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="47d56-108">Ereditare dalla classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> in un controller progettato per svolgere la funzione di API Web.</span><span class="sxs-lookup"><span data-stu-id="47d56-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="47d56-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="47d56-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="47d56-110">La classe `ControllerBase` consente l'accesso a diverse proprietà e a svariati metodi.</span><span class="sxs-lookup"><span data-stu-id="47d56-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="47d56-111">Nel codice precedente, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> sono due esempi.</span><span class="sxs-lookup"><span data-stu-id="47d56-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="47d56-112">Questi metodi vengono chiamati all'interno di metodi di azioni per restituire rispettivamente i codici di stato HTTP 400 e HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="47d56-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="47d56-113">Alla proprietà <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, fornita anche questa da `ControllerBase`, si accede per gestire la convalida del modello di richiesta.</span><span class="sxs-lookup"><span data-stu-id="47d56-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="47d56-114">Annotare una classe con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="47d56-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="47d56-115">ASP.NET Core 2.1 introduce l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) per indicare una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="47d56-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="47d56-116">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="47d56-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="47d56-117">Per usare questo attributo, è necessaria una versione di compatibilità di 2.1 o versioni successive impostata tramite <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="47d56-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="47d56-118">Ad esempio, il codice evidenziato in *Startup.ConfigureServices* imposta il flag di compatibilità 2.2:</span><span class="sxs-lookup"><span data-stu-id="47d56-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="47d56-119">Per ulteriori informazioni, vedere <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="47d56-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="47d56-120">L'attributo `[ApiController]` è in genere associato a `ControllerBase` per abilitare il comportamento specifico di REST per i controller.</span><span class="sxs-lookup"><span data-stu-id="47d56-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="47d56-121">`ControllerBase` consente l'accesso a metodi quali <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="47d56-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="47d56-122">Un altro approccio consiste nel creare una classe controller di base personalizzata annotata con l'attributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="47d56-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="47d56-123">Le sezioni seguenti descrivono le funzionalità aggiunte dall'attributo che favoriscono una maggiore praticità.</span><span class="sxs-lookup"><span data-stu-id="47d56-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="47d56-124">Risposte con i dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="47d56-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="47d56-125">ASP.NET Core 2.1 e versioni successive include [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), un tipo basato sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="47d56-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="47d56-126">Il tipo `ProblemDetails` fornisce un formato standardizzato per trasmettere i dettagli leggibili dal computer degli errori in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="47d56-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="47d56-127">In ASP.NET Core 2.2 e versioni successive, MVC trasforma i risultati del codice di stato di errore (codice di stato 400 e versioni successive) in un risultato con `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="47d56-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="47d56-128">Esaminare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="47d56-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="47d56-129">La risposta HTTP per il risultato `NotFound` ha il codice di stato 404 con un corpo `ProblemDetails` analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="47d56-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="47d56-130">La funzionalità dei dettagli del problema richiede un flag di compatibilità pari a 2.2 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="47d56-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="47d56-131">Il comportamento predefinito viene disabilitato quando la proprietà [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="47d56-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="47d56-132">Il codice seguente evidenziato da `Startup.ConfigureServices` disabilita i dettagli del problema:</span><span class="sxs-lookup"><span data-stu-id="47d56-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="47d56-133">Usare la proprietà [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="47d56-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="47d56-134">Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="47d56-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="47d56-135">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="47d56-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="47d56-136">Gli errori di convalida attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="47d56-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="47d56-137">Il codice seguente non è più necessario nelle azioni:</span><span class="sxs-lookup"><span data-stu-id="47d56-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="47d56-138">Usare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per personalizzare l'output della risposta prodotta.</span><span class="sxs-lookup"><span data-stu-id="47d56-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="47d56-139">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="47d56-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="47d56-140">Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="47d56-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="47d56-141">Con un flag di compatibilità pari a 2.2 o versioni successive, il tipo di risposta predefinito restituito per le risposte 400 è un <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="47d56-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="47d56-142">Usare la proprietà [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> per usare il formato di errore ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="47d56-142">Use the Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="47d56-143">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="47d56-143">Binding source parameter inference</span></span>

<span data-ttu-id="47d56-144">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="47d56-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="47d56-145">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="47d56-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="47d56-146">Attributo</span><span class="sxs-lookup"><span data-stu-id="47d56-146">Attribute</span></span>|<span data-ttu-id="47d56-147">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="47d56-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="47d56-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="47d56-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="47d56-149">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="47d56-149">Request body</span></span> |
|<span data-ttu-id="47d56-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="47d56-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="47d56-151">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="47d56-151">Form data in the request body</span></span> |
|<span data-ttu-id="47d56-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="47d56-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="47d56-153">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="47d56-153">Request header</span></span> |
|<span data-ttu-id="47d56-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="47d56-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="47d56-155">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="47d56-155">Request query string parameter</span></span> |
|<span data-ttu-id="47d56-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="47d56-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="47d56-157">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="47d56-157">Route data from the current request</span></span> |
|<span data-ttu-id="47d56-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="47d56-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="47d56-159">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="47d56-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="47d56-160">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="47d56-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="47d56-161">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="47d56-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="47d56-162">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="47d56-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="47d56-163">Senza l'attributo `[ApiController]`, gli attributi di origine di associazione vengono definiti in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="47d56-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="47d56-164">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="47d56-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="47d56-165">Le regole di inferenza vengono applicate per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="47d56-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="47d56-166">Queste regole configurano le origini di associazione altrimenti applicate manualmente con ogni probabilità ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="47d56-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="47d56-167">Gli attributi di origine di associazione si comportano nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="47d56-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="47d56-168">**[FromBody]**  viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="47d56-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="47d56-169">Un'eccezione a questa regola è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="47d56-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="47d56-170">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="47d56-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="47d56-171">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="47d56-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="47d56-172">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se si desidera tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="47d56-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="47d56-173">Quando per un'azione esistono più parametri, specificati in modo esplicito (tramite `[FromBody]`) o dedotti in quanto associati dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="47d56-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="47d56-174">Le firme di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="47d56-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="47d56-175">**[FromForm]** viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="47d56-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="47d56-176">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="47d56-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="47d56-177">**[FromRoute]**  viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="47d56-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="47d56-178">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="47d56-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="47d56-179">**[FromQuery]**  viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="47d56-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="47d56-180">Le regole di inferenza predefinite vengono disabilitate quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="47d56-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="47d56-181">Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="47d56-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="47d56-182">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="47d56-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="47d56-183">Quando un parametro di azione è annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), viene dedotto il tipo di contenuto `multipart/form-data` per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="47d56-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="47d56-184">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="47d56-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="47d56-185">Aggiungere il codice seguente in *Startup.ConfigureServices* dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="47d56-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="47d56-186">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="47d56-186">Attribute routing requirement</span></span>

<span data-ttu-id="47d56-187">Il routing degli attributi diventa un requisito.</span><span class="sxs-lookup"><span data-stu-id="47d56-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="47d56-188">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="47d56-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="47d56-189">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="47d56-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="47d56-190">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="47d56-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>