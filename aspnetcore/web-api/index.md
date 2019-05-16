---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni di base sulla creazione di un'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/07/2019
uid: web-api/index
ms.openlocfilehash: 593fd33babc81cddfc4db2150a37e5ec3bc1a0be
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450844"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="3c344-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c344-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="3c344-104">Di [Scott Addie](https://github.com/scottaddie) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3c344-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3c344-105">ASP.NET Core supporta la creazione di servizi RESTful, noti anche come API Web, con C#.</span><span class="sxs-lookup"><span data-stu-id="3c344-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="3c344-106">Per gestire le richieste, un'API Web usa i controller.</span><span class="sxs-lookup"><span data-stu-id="3c344-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="3c344-107">I *controller* in un'API Web sono classi che derivano da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="3c344-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="3c344-108">Questo articolo illustra come usare i controller per gestire le richieste API.</span><span class="sxs-lookup"><span data-stu-id="3c344-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="3c344-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="3c344-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="3c344-110">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3c344-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="3c344-111">Classe ControllerBase</span><span class="sxs-lookup"><span data-stu-id="3c344-111">ControllerBase class</span></span>

<span data-ttu-id="3c344-112">Un'API Web include una o più classi controller che derivano da <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="3c344-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="3c344-113">Ad esempio, il modello di progetto API Web crea un controller Values:</span><span class="sxs-lookup"><span data-stu-id="3c344-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="3c344-114">Non creare un controller API Web tramite derivazione dalla classe di base <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="3c344-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="3c344-115">La classe `Controller` deriva da `ControllerBase` e aggiunge il supporto per le visualizzazioni, pertanto è progettata per la gestione delle pagine Web e non per le richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="3c344-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="3c344-116">Esiste un'eccezione a questa regola: quando si prevede di usare lo stesso controller sia per le API che per le visualizzazioni, derivarlo da `Controller`.</span><span class="sxs-lookup"><span data-stu-id="3c344-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="3c344-117">La classe `ControllerBase` offre molti metodi e proprietà utili per la gestione delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c344-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="3c344-118">Ad esempio, `ControllerBase.CreatedAtAction` restituisce un codice di stato 201:</span><span class="sxs-lookup"><span data-stu-id="3c344-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="3c344-119">Di seguito sono elencati alcuni esempi di metodi forniti da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="3c344-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="3c344-120">Metodo</span><span class="sxs-lookup"><span data-stu-id="3c344-120">Method</span></span>  |<span data-ttu-id="3c344-121">Note</span><span class="sxs-lookup"><span data-stu-id="3c344-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="3c344-122">Restituisce il codice di stato 400.</span><span class="sxs-lookup"><span data-stu-id="3c344-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="3c344-123">Restituisce il codice di stato 404.</span><span class="sxs-lookup"><span data-stu-id="3c344-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="3c344-124">Restituisce un file.</span><span class="sxs-lookup"><span data-stu-id="3c344-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="3c344-125">Richiama l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="3c344-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="3c344-126">Richiama la [convalida dei modelli](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="3c344-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="3c344-127">Per un elenco di tutti i metodi e proprietà disponibili, vedere <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="3c344-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="3c344-128">Attributi</span><span class="sxs-lookup"><span data-stu-id="3c344-128">Attributes</span></span>

<span data-ttu-id="3c344-129">Lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc> include attributi che possono essere usati per configurare il comportamento del controller di API Web e i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="3c344-130">L'esempio seguente usa gli attributi per specificare il metodo HTTP accettato e i codici di stato restituiti:</span><span class="sxs-lookup"><span data-stu-id="3c344-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="3c344-131">Di seguito sono riportati altri esempi degli attributi disponibili.</span><span class="sxs-lookup"><span data-stu-id="3c344-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="3c344-132">Attributo</span><span class="sxs-lookup"><span data-stu-id="3c344-132">Attribute</span></span>|<span data-ttu-id="3c344-133">Note</span><span class="sxs-lookup"><span data-stu-id="3c344-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="3c344-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="3c344-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="3c344-135">Specifica il modello di URL per un controller o un'azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="3c344-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="3c344-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="3c344-137">Specifica il prefisso e le proprietà da includere per l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="3c344-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="3c344-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="3c344-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="3c344-139">Identifica un'azione che supporta il metodo HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3c344-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="3c344-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="3c344-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="3c344-141">Specifica i tipi di dati accettati da un'azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="3c344-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="3c344-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="3c344-143">Specifica i tipi di dati restituiti da un'azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="3c344-144">Per un elenco che include gli attributi disponibili, vedere lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="3c344-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="3c344-145">Attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="3c344-145">ApiController attribute</span></span>

<span data-ttu-id="3c344-146">L'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) può essere applicato a una classe di controller per abilitare i comportamenti specifici dell'API:</span><span class="sxs-lookup"><span data-stu-id="3c344-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="3c344-147">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="3c344-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="3c344-148">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="3c344-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="3c344-149">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="3c344-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="3c344-150">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="3c344-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="3c344-151">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="3c344-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="3c344-152">Queste funzionalità richiedono la [versione di compatibilità](<xref:mvc/compatibility-version>) 2.1 o successiva.</span><span class="sxs-lookup"><span data-stu-id="3c344-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="3c344-153">ApiController per controller specifici</span><span class="sxs-lookup"><span data-stu-id="3c344-153">ApiController on specific controllers</span></span>

<span data-ttu-id="3c344-154">L'attributo `[ApiController]` può essere applicato a controller specifici, come illustrato nell'esempio seguente dal modello di progetto:</span><span class="sxs-lookup"><span data-stu-id="3c344-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="3c344-155">ApiController per più controller</span><span class="sxs-lookup"><span data-stu-id="3c344-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="3c344-156">Un approccio per usare l'attributo su più di un controller consiste nel creare una classe di controller di base personalizzata annotata con l'attributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="3c344-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="3c344-157">Ecco un esempio che illustra una classe di base personalizzata e un controller che deriva da tale classe:</span><span class="sxs-lookup"><span data-stu-id="3c344-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="3c344-158">ApiController per un assembly</span><span class="sxs-lookup"><span data-stu-id="3c344-158">ApiController on an assembly</span></span>

<span data-ttu-id="3c344-159">Se la [versione di compatibilità](<xref:mvc/compatibility-version>) è impostata su 2.2 o una versione successiva, l'attributo `[ApiController]` può essere applicato a un assembly.</span><span class="sxs-lookup"><span data-stu-id="3c344-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="3c344-160">L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="3c344-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="3c344-161">Non esiste alcun modo per rifiutare esplicitamente singoli controller.</span><span class="sxs-lookup"><span data-stu-id="3c344-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="3c344-162">Applicare l'attributo a livello di assembly alla classe `Startup`, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3c344-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a><span data-ttu-id="3c344-163">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="3c344-163">Attribute routing requirement</span></span>

<span data-ttu-id="3c344-164">Con l'attributo `ApiController` il routing degli attributi è un requisito.</span><span class="sxs-lookup"><span data-stu-id="3c344-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="3c344-165">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c344-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="3c344-166">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="3c344-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="3c344-167">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="3c344-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="3c344-168">Con l'attributo `ApiController` gli errori di convalida del modello attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="3c344-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="3c344-169">Di conseguenza è necessario il codice seguente in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="3c344-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="3c344-170">Risposta BadRequest predefinita</span><span class="sxs-lookup"><span data-stu-id="3c344-170">Default BadRequest response</span></span> 

<span data-ttu-id="3c344-171">Con la versione di compatibilità 2.2 o successiva, il tipo di risposta predefinito per le risposte HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="3c344-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="3c344-172">Il tipo `ValidationProblemDetails` è conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="3c344-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="3c344-173">Per modificare la risposta predefinita per <xref:Microsoft.AspNetCore.Mvc.SerializableError>, impostare la proprietà `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` su `true` in `Startup.ConfigureServices`, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3c344-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="3c344-174">Personalizzare la risposta BadRequest</span><span class="sxs-lookup"><span data-stu-id="3c344-174">Customize BadRequest response</span></span>

<span data-ttu-id="3c344-175">Per personalizzare la risposta risultante da un errore di convalida, usare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="3c344-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="3c344-176">Aggiungere il codice evidenziato seguente dopo `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="3c344-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a><span data-ttu-id="3c344-177">Registrare le risposte 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="3c344-177">Log automatic 400 responses</span></span>

<span data-ttu-id="3c344-178">Vedere [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) (Come registrare le risposte 400 automatiche negli errori di convalida del modello - aspnet/AspNetCore.Docs 12157).</span><span class="sxs-lookup"><span data-stu-id="3c344-178">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400"></a><span data-ttu-id="3c344-179">Disabilitare il comportamento automatico per gli errori 400</span><span class="sxs-lookup"><span data-stu-id="3c344-179">Disable automatic 400</span></span>

<span data-ttu-id="3c344-180">Per disabilitare il comportamento automatico per gli errori 400, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> su `true`.</span><span class="sxs-lookup"><span data-stu-id="3c344-180">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="3c344-181">Aggiungere il codice evidenziato seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="3c344-181">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="3c344-182">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="3c344-182">Binding source parameter inference</span></span>

<span data-ttu-id="3c344-183">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-183">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="3c344-184">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="3c344-184">The following binding source attributes exist:</span></span>

|<span data-ttu-id="3c344-185">Attributo</span><span class="sxs-lookup"><span data-stu-id="3c344-185">Attribute</span></span>|<span data-ttu-id="3c344-186">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="3c344-186">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="3c344-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="3c344-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="3c344-188">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="3c344-188">Request body</span></span> |
|<span data-ttu-id="3c344-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="3c344-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="3c344-190">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="3c344-190">Form data in the request body</span></span> |
|<span data-ttu-id="3c344-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="3c344-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="3c344-192">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="3c344-192">Request header</span></span> |
|<span data-ttu-id="3c344-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="3c344-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="3c344-194">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="3c344-194">Request query string parameter</span></span> |
|<span data-ttu-id="3c344-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="3c344-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="3c344-196">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="3c344-196">Route data from the current request</span></span> |
|<span data-ttu-id="3c344-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="3c344-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="3c344-198">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="3c344-198">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="3c344-199">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="3c344-199">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="3c344-200">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="3c344-200">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="3c344-201">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="3c344-201">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="3c344-202">Senza l'attributo `[ApiController]` o altri attributi di origine di associazione come `[FromQuery]`, il runtime di ASP.NET Core tenta di usare lo strumento di associazione di modelli a oggetti complesso.</span><span class="sxs-lookup"><span data-stu-id="3c344-202">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="3c344-203">Lo strumento di associazione di modelli a oggetti complesso estrae i dati dal provider di valori in un ordine definito.</span><span class="sxs-lookup"><span data-stu-id="3c344-203">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="3c344-204">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="3c344-204">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="3c344-205">L'attributo `[ApiController]` applica le regole di inferenza per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-205">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="3c344-206">Queste regole consentono di evitare di dover identificare le origini di associazione manualmente applicando attributi ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-206">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="3c344-207">Il comportamento delle regole di inferenza per le origini di associazione è il seguente:</span><span class="sxs-lookup"><span data-stu-id="3c344-207">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="3c344-208">`[FromBody]` viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="3c344-208">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="3c344-209">Un'eccezione alla regola di inferenza `[FromBody]` è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="3c344-209">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="3c344-210">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="3c344-210">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="3c344-211">`[FromForm]` viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="3c344-211">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="3c344-212">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3c344-212">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="3c344-213">`[FromRoute]` viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="3c344-213">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="3c344-214">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="3c344-214">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="3c344-215">`[FromQuery]` viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="3c344-215">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="3c344-216">Note sulla regola di inferenza FromBody</span><span class="sxs-lookup"><span data-stu-id="3c344-216">FromBody inference notes</span></span>

<span data-ttu-id="3c344-217">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="3c344-217">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="3c344-218">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="3c344-218">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="3c344-219">Quando a un'azione sono associati più parametri dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3c344-219">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="3c344-220">Tutte le firme di metodo di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="3c344-220">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="3c344-221">`[FromBody]` dedotto per entrambi, perché si tratta di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="3c344-221">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="3c344-222">`[FromBody]` attributo per uno, dedotto per l'altro perché è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="3c344-222">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="3c344-223">`[FromBody]` attributo per entrambi.</span><span class="sxs-lookup"><span data-stu-id="3c344-223">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="3c344-224">In ASP.NET Core 2.1, i parametri di tipo raccolta, come elenchi e matrici, vengono dedotti in modo errato come `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="3c344-224">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="3c344-225">È necessario usare l'attributo `[FromBody]` per questi parametri, se devono essere associati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3c344-225">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="3c344-226">Questo comportamento è stato corretto in ASP.NET Core 2.2 o versioni successive, in cui i parametri di tipo raccolta vengono dedotti come associati dal corpo per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3c344-226">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="3c344-227">Disabilitare le regole di inferenza</span><span class="sxs-lookup"><span data-stu-id="3c344-227">Disable inference rules</span></span>

<span data-ttu-id="3c344-228">Per disabilitare l'inferenza delle origini di associazione, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> su `true`.</span><span class="sxs-lookup"><span data-stu-id="3c344-228">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="3c344-229">Aggiungere il codice seguente in `Startup.ConfigureServices` dopo `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="3c344-229">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="3c344-230">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="3c344-230">Multipart/form-data request inference</span></span>

<span data-ttu-id="3c344-231">L'attributo `[ApiController]` applica una regola di inferenza quando un parametro di azione è annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): viene dedotto il tipo di contenuto della richiesta `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3c344-231">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="3c344-232">Per disabilitare il comportamento predefinito, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> su `true` in `Startup.ConfigureServices`, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3c344-232">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="3c344-233">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="3c344-233">Problem details for error status codes</span></span>

<span data-ttu-id="3c344-234">Con la versione di compatibilità 2.2 o successiva, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="3c344-234">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="3c344-235">Il tipo `ProblemDetails` si basa sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807) per fornire dettagli sull'errore leggibili dal computer in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c344-235">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="3c344-236">Si consideri il codice seguente in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="3c344-236">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="3c344-237">La risposta HTTP per `NotFound` ha un codice di stato 404 con corpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="3c344-237">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="3c344-238">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c344-238">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="3c344-239">Personalizzare la risposta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="3c344-239">Customize ProblemDetails response</span></span>

<span data-ttu-id="3c344-240">Usare la proprietà `ClientErrorMapping` per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="3c344-240">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="3c344-241">Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="3c344-241">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="3c344-242">Disabilitare la risposta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="3c344-242">Disable ProblemDetails response</span></span>

<span data-ttu-id="3c344-243">La creazione automatica di `ProblemDetails` è disabilitata quando la proprietà `SuppressMapClientErrors` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="3c344-243">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="3c344-244">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3c344-244">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="3c344-245">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3c344-245">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
