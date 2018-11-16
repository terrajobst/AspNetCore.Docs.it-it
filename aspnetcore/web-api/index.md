---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni sulle funzionalità disponibili per la creazione di un'API Web in ASP.NET Core e su quando è appropriato usare ciascuna funzionalità.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/06/2018
uid: web-api/index
ms.openlocfilehash: 7541c4c308deaecda0bda9a9c77d9372b65a5100
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635302"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="e43b1-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e43b1-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="e43b1-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e43b1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e43b1-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e43b1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e43b1-106">Questo documento illustra come creare un'API Web in ASP.NET Core e indica quando è più appropriato usare ciascuna funzionalità.</span><span class="sxs-lookup"><span data-stu-id="e43b1-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="e43b1-107">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="e43b1-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="e43b1-108">Ereditare dalla classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> in un controller progettato per svolgere la funzione di API Web.</span><span class="sxs-lookup"><span data-stu-id="e43b1-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="e43b1-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e43b1-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="e43b1-110">La classe `ControllerBase` consente l'accesso a diverse proprietà e a svariati metodi.</span><span class="sxs-lookup"><span data-stu-id="e43b1-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="e43b1-111">Nel codice precedente, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)> sono due esempi.</span><span class="sxs-lookup"><span data-stu-id="e43b1-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="e43b1-112">Questi metodi vengono chiamati all'interno di metodi di azioni per restituire rispettivamente i codici di stato HTTP 400 e HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="e43b1-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="e43b1-113">Alla proprietà <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, fornita anche questa da `ControllerBase`, si accede per gestire la convalida del modello di richiesta.</span><span class="sxs-lookup"><span data-stu-id="e43b1-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="e43b1-114">Annotazione con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="e43b1-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="e43b1-115">ASP.NET Core 2.1 introduce l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) per indicare una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="e43b1-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="e43b1-116">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e43b1-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="e43b1-117">Per usare questo attributo al livello del controller, è necessaria una versione di compatibilità 2.1 o versioni successive impostata tramite <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="e43b1-118">Ad esempio, il codice evidenziato in `Startup.ConfigureServices` imposta il flag di compatibilità 2.1:</span><span class="sxs-lookup"><span data-stu-id="e43b1-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="e43b1-119">Per ulteriori informazioni, vedere <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e43b1-120">In ASP.NET Core 2.2 o versioni successive, l'attributo `[ApiController]` può essere applicato a un assembly.</span><span class="sxs-lookup"><span data-stu-id="e43b1-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="e43b1-121">L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="e43b1-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="e43b1-122">Tenere presente che non esiste alcun modo per rifiutare esplicitamente singoli controller.</span><span class="sxs-lookup"><span data-stu-id="e43b1-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="e43b1-123">È raccomandata l'applicazione degli attributi a livello di assembly alla classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="e43b1-124">Per usare questo attributo al livello dell'assembly, è necessaria una versione di compatibilità 2.2 o versioni successive impostata tramite <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e43b1-125">L'attributo `[ApiController]` è in genere associato a `ControllerBase` per abilitare il comportamento specifico di REST per i controller.</span><span class="sxs-lookup"><span data-stu-id="e43b1-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="e43b1-126">`ControllerBase` consente l'accesso a metodi quali <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="e43b1-127">Un altro approccio consiste nel creare una classe controller di base personalizzata annotata con l'attributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="e43b1-128">Le sezioni seguenti descrivono le funzionalità aggiunte dall'attributo che favoriscono una maggiore praticità.</span><span class="sxs-lookup"><span data-stu-id="e43b1-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="e43b1-129">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="e43b1-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="e43b1-130">Gli errori di convalida del modello attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="e43b1-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="e43b1-131">Di conseguenza, il codice seguente non è più necessario nelle azioni:</span><span class="sxs-lookup"><span data-stu-id="e43b1-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="e43b1-132">Usare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per personalizzare l'output della risposta prodotta.</span><span class="sxs-lookup"><span data-stu-id="e43b1-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="e43b1-133">Disabilitare il comportamento predefinito è utile quando l'azione può consentire il ripristino da un errore di convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="e43b1-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="e43b1-134">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="e43b1-135">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e43b1-136">Con un flag di compatibilità 2.2 o successivo, il tipo di risposta predefinito per le risposte HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="e43b1-137">Il tipo `ValidationProblemDetails` è conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="e43b1-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="e43b1-138">Impostare la proprietà `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` su `true` invece restituisce il formato di errore ASP.NET Core 2.1 <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="e43b1-139">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="e43b1-140">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="e43b1-140">Binding source parameter inference</span></span>

<span data-ttu-id="e43b1-141">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="e43b1-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="e43b1-142">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="e43b1-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="e43b1-143">Attributo</span><span class="sxs-lookup"><span data-stu-id="e43b1-143">Attribute</span></span>|<span data-ttu-id="e43b1-144">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="e43b1-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="e43b1-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="e43b1-146">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="e43b1-146">Request body</span></span> |
|<span data-ttu-id="e43b1-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="e43b1-148">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="e43b1-148">Form data in the request body</span></span> |
|<span data-ttu-id="e43b1-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="e43b1-150">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="e43b1-150">Request header</span></span> |
|<span data-ttu-id="e43b1-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="e43b1-152">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="e43b1-152">Request query string parameter</span></span> |
|<span data-ttu-id="e43b1-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="e43b1-154">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="e43b1-154">Route data from the current request</span></span> |
|<span data-ttu-id="e43b1-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="e43b1-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="e43b1-156">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="e43b1-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="e43b1-157">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="e43b1-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="e43b1-158">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="e43b1-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="e43b1-159">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="e43b1-160">Senza l'attributo `[ApiController]`, gli attributi di origine di associazione vengono definiti in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="e43b1-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="e43b1-161">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="e43b1-161">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="e43b1-162">Le regole di inferenza vengono applicate per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e43b1-162">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="e43b1-163">Queste regole configurano le origini di associazione altrimenti applicate manualmente con ogni probabilità ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e43b1-163">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="e43b1-164">Gli attributi di origine di associazione si comportano nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e43b1-164">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="e43b1-165">**[FromBody]**  viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="e43b1-165">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="e43b1-166">Un'eccezione a questa regola è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-166">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="e43b1-167">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="e43b1-167">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="e43b1-168">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-168">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="e43b1-169">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="e43b1-169">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="e43b1-170">Quando per un'azione esistono più parametri, specificati in modo esplicito (tramite `[FromBody]`) o dedotti in quanto associati dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="e43b1-170">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="e43b1-171">Le firme di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="e43b1-171">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="e43b1-172">**[FromForm]** viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-172">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="e43b1-173">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="e43b1-173">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="e43b1-174">**[FromRoute]**  viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="e43b1-174">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="e43b1-175">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-175">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="e43b1-176">**[FromQuery]**  viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e43b1-176">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="e43b1-177">Le regole di inferenza predefinite vengono disabilitate quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-177">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="e43b1-178">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-178">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="e43b1-179">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="e43b1-179">Multipart/form-data request inference</span></span>

<span data-ttu-id="e43b1-180">Quando un parametro di azione è annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), viene dedotto il tipo di contenuto `multipart/form-data` per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="e43b1-180">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="e43b1-181">Il comportamento predefinito viene disabilitato quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-181">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e43b1-182">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-182">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e43b1-183">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-183">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="e43b1-184">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="e43b1-184">Attribute routing requirement</span></span>

<span data-ttu-id="e43b1-185">Il routing degli attributi diventa un requisito.</span><span class="sxs-lookup"><span data-stu-id="e43b1-185">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="e43b1-186">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e43b1-186">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="e43b1-187">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-187">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="e43b1-188">Risposte con i dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="e43b1-188">Problem details responses for error status codes</span></span>

<span data-ttu-id="e43b1-189">In ASP.NET Core 2.2 o versioni successive, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="e43b1-189">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="e43b1-190">`ProblemDetails` è:</span><span class="sxs-lookup"><span data-stu-id="e43b1-190">`ProblemDetails` is:</span></span>

* <span data-ttu-id="e43b1-191">Un tipo basato sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="e43b1-191">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="e43b1-192">Un formato standardizzato per specificare i dettagli degli errori in formato leggibile dal computer in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e43b1-192">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="e43b1-193">Si consideri il codice seguente in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="e43b1-193">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="e43b1-194">La risposta HTTP per `NotFound` ha un codice di stato 404 con corpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-194">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="e43b1-195">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e43b1-195">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="e43b1-196">La funzionalità dei dettagli del problema richiede un flag di compatibilità pari a 2.2 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e43b1-196">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="e43b1-197">Il comportamento predefinito viene disabilitato quando la proprietà `SuppressMapClientErrors` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-197">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="e43b1-198">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e43b1-198">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="e43b1-199">Usare la proprietà `ClientErrorMapping` per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="e43b1-199">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="e43b1-200">Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="e43b1-200">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e43b1-201">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e43b1-201">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
