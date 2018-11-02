---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni sulle funzionalità disponibili per la creazione di un'API Web in ASP.NET Core e su quando è appropriato usare ciascuna funzionalità.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/30/2018
uid: web-api/index
ms.openlocfilehash: b3e26bee5e4dc8937e810bc5db300a486437f568
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244762"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="7ec3a-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec3a-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="7ec3a-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="7ec3a-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="7ec3a-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7ec3a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7ec3a-106">Questo documento illustra come creare un'API Web in ASP.NET Core e indica quando è più appropriato usare ciascuna funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="7ec3a-107">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="7ec3a-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="7ec3a-108">Ereditare dalla classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> in un controller progettato per svolgere la funzione di API Web.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="7ec3a-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="7ec3a-110">La classe `ControllerBase` consente l'accesso a diverse proprietà e a svariati metodi.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="7ec3a-111">Nel codice precedente, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> sono due esempi.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="7ec3a-112">Questi metodi vengono chiamati all'interno di metodi di azioni per restituire rispettivamente i codici di stato HTTP 400 e HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="7ec3a-113">Alla proprietà <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, fornita anche questa da `ControllerBase`, si accede per gestire la convalida del modello di richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontrollerattribute"></a><span data-ttu-id="7ec3a-114">Annotazione con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="7ec3a-114">Annotation with ApiControllerAttribute</span></span>

<span data-ttu-id="7ec3a-115">ASP.NET Core 2.1 introduce l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) per indicare una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="7ec3a-116">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="7ec3a-117">Per usare questo attributo al livello del controller, è necessaria una versione di compatibilità 2.1 o versioni successive impostata tramite <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="7ec3a-118">Ad esempio, il codice evidenziato in `Startup.ConfigureServices` imposta il flag di compatibilità 2.1:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="7ec3a-119">Per ulteriori informazioni, vedere <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7ec3a-120">In ASP.NET Core 2.2 o versioni successive, l'attributo `[ApiController]` può essere applicato a un assembly.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="7ec3a-121">L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="7ec3a-122">Tenere presente che non esiste alcun modo per rifiutare esplicitamente singoli controller.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="7ec3a-123">È raccomandata l'applicazione degli attributi a livello di assembly alla classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="7ec3a-124">Per usare questo attributo al livello dell'assembly, è necessaria una versione di compatibilità 2.2 o versioni successive impostata tramite <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7ec3a-125">L'attributo `[ApiController]` è in genere associato a `ControllerBase` per abilitare il comportamento specifico di REST per i controller.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="7ec3a-126">`ControllerBase` consente l'accesso a metodi quali <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="7ec3a-127">Un altro approccio consiste nel creare una classe controller di base personalizzata annotata con l'attributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="7ec3a-128">Le sezioni seguenti descrivono le funzionalità aggiunte dall'attributo che favoriscono una maggiore praticità.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="7ec3a-129">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="7ec3a-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="7ec3a-130">Gli errori di convalida attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-130">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="7ec3a-131">Il codice seguente non è più necessario nelle azioni:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-131">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="7ec3a-132">Usare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per personalizzare l'output della risposta prodotta.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="7ec3a-133">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-133">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="7ec3a-134">Aggiungere il codice seguente a `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-134">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7ec3a-135">Con un flag di compatibilità 2.2 o successivo, il tipo di risposta predefinito per le risposte HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-135">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="7ec3a-136">Il tipo `ValidationProblemDetails` è conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="7ec3a-136">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="7ec3a-137">Impostare la proprietà `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` su `true` invece restituisce il formato di errore ASP.NET Core 2.1 <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-137">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="7ec3a-138">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-138">Add the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="7ec3a-139">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="7ec3a-139">Binding source parameter inference</span></span>

<span data-ttu-id="7ec3a-140">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-140">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="7ec3a-141">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-141">The following binding source attributes exist:</span></span>

|<span data-ttu-id="7ec3a-142">Attributo</span><span class="sxs-lookup"><span data-stu-id="7ec3a-142">Attribute</span></span>|<span data-ttu-id="7ec3a-143">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="7ec3a-143">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="7ec3a-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="7ec3a-145">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="7ec3a-145">Request body</span></span> |
|<span data-ttu-id="7ec3a-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="7ec3a-147">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="7ec3a-147">Form data in the request body</span></span> |
|<span data-ttu-id="7ec3a-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="7ec3a-149">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="7ec3a-149">Request header</span></span> |
|<span data-ttu-id="7ec3a-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="7ec3a-151">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="7ec3a-151">Request query string parameter</span></span> |
|<span data-ttu-id="7ec3a-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="7ec3a-153">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="7ec3a-153">Route data from the current request</span></span> |
|<span data-ttu-id="7ec3a-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="7ec3a-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="7ec3a-155">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="7ec3a-155">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="7ec3a-156">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="7ec3a-156">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="7ec3a-157">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-157">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="7ec3a-158">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-158">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="7ec3a-159">Senza l'attributo `[ApiController]`, gli attributi di origine di associazione vengono definiti in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-159">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="7ec3a-160">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-160">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="7ec3a-161">Le regole di inferenza vengono applicate per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-161">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="7ec3a-162">Queste regole configurano le origini di associazione altrimenti applicate manualmente con ogni probabilità ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-162">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="7ec3a-163">Gli attributi di origine di associazione si comportano nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-163">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="7ec3a-164">**[FromBody]**  viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-164">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="7ec3a-165">Un'eccezione a questa regola è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-165">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="7ec3a-166">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-166">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="7ec3a-167">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-167">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="7ec3a-168">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-168">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="7ec3a-169">Quando per un'azione esistono più parametri, specificati in modo esplicito (tramite `[FromBody]`) o dedotti in quanto associati dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-169">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="7ec3a-170">Le firme di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-170">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="7ec3a-171">**[FromForm]** viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-171">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="7ec3a-172">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-172">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="7ec3a-173">**[FromRoute]**  viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-173">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="7ec3a-174">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-174">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="7ec3a-175">**[FromQuery]**  viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-175">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="7ec3a-176">Le regole di inferenza predefinite vengono disabilitate quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-176">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="7ec3a-177">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-177">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="7ec3a-178">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="7ec3a-178">Multipart/form-data request inference</span></span>

<span data-ttu-id="7ec3a-179">Quando un parametro di azione è annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), viene dedotto il tipo di contenuto `multipart/form-data` per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-179">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="7ec3a-180">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-180">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7ec3a-181">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-181">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="7ec3a-182">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-182">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="7ec3a-183">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="7ec3a-183">Attribute routing requirement</span></span>

<span data-ttu-id="7ec3a-184">Il routing degli attributi diventa un requisito.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-184">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="7ec3a-185">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-185">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="7ec3a-186">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-186">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="7ec3a-187">Risposte con i dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="7ec3a-187">Problem details responses for error status codes</span></span>

<span data-ttu-id="7ec3a-188">In ASP.NET Core 2.2 o versioni successive, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-188">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="7ec3a-189">`ProblemDetails` è:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-189">`ProblemDetails` is:</span></span>

* <span data-ttu-id="7ec3a-190">Un tipo basato sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="7ec3a-190">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="7ec3a-191">Un formato standardizzato per specificare i dettagli degli errori in formato leggibile dal computer in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-191">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="7ec3a-192">Si consideri il codice seguente in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-192">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="7ec3a-193">La risposta HTTP per `NotFound` ha un codice di stato 404 con corpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-193">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="7ec3a-194">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-194">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="7ec3a-195">La funzionalità dei dettagli del problema richiede un flag di compatibilità pari a 2.2 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-195">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="7ec3a-196">Il comportamento predefinito viene disabilitato quando la proprietà `SuppressMapClientErrors` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-196">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="7ec3a-197">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-197">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="7ec3a-198">Usare la proprietà `ClientErrorMapping` per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="7ec3a-198">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="7ec3a-199">Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="7ec3a-199">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7ec3a-200">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7ec3a-200">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
