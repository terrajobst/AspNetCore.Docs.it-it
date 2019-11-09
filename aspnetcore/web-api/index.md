---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni di base sulla creazione di un'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: 122de0a225668a7523eec900e2ad8fdac56d7886
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2019
ms.locfileid: "73897016"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="12811-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12811-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="12811-104">Di [Scott Addie](https://github.com/scottaddie) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="12811-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="12811-105">ASP.NET Core supporta la creazione di servizi RESTful, noti anche come API Web, con C#.</span><span class="sxs-lookup"><span data-stu-id="12811-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="12811-106">Per gestire le richieste, un'API Web usa i controller.</span><span class="sxs-lookup"><span data-stu-id="12811-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="12811-107">I *controller* in un'API Web sono classi che derivano da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="12811-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="12811-108">Questo articolo illustra come usare i controller per gestire le richieste API Web.</span><span class="sxs-lookup"><span data-stu-id="12811-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="12811-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="12811-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="12811-110">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="12811-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="12811-111">Classe ControllerBase</span><span class="sxs-lookup"><span data-stu-id="12811-111">ControllerBase class</span></span>

<span data-ttu-id="12811-112">Un'API Web è costituita da una o più classi controller che derivano da <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="12811-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="12811-113">Il modello di progetto API Web fornisce un controller Starter:</span><span class="sxs-lookup"><span data-stu-id="12811-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="12811-114">Non creare un controller API Web tramite derivazione dalla classe <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="12811-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="12811-115">La classe `Controller` deriva da `ControllerBase` e aggiunge il supporto per le visualizzazioni, pertanto è progettata per la gestione delle pagine Web e non per le richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="12811-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="12811-116">Si è verificata un'eccezione a questa regola: se si prevede di usare lo stesso controller per le visualizzazioni e le API Web, derivarlo dal `Controller`.</span><span class="sxs-lookup"><span data-stu-id="12811-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="12811-117">La classe `ControllerBase` offre molti metodi e proprietà utili per la gestione delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="12811-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="12811-118">Ad esempio, `ControllerBase.CreatedAtAction` restituisce un codice di stato 201:</span><span class="sxs-lookup"><span data-stu-id="12811-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="12811-119">Di seguito sono elencati alcuni esempi di metodi forniti da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="12811-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="12811-120">Metodo</span><span class="sxs-lookup"><span data-stu-id="12811-120">Method</span></span>   |<span data-ttu-id="12811-121">Note</span><span class="sxs-lookup"><span data-stu-id="12811-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="12811-122">Restituisce il codice di stato 400.</span><span class="sxs-lookup"><span data-stu-id="12811-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|<span data-ttu-id="12811-123">Restituisce il codice di stato 404.</span><span class="sxs-lookup"><span data-stu-id="12811-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="12811-124">Restituisce un file.</span><span class="sxs-lookup"><span data-stu-id="12811-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="12811-125">Richiama l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="12811-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="12811-126">Richiama la [convalida dei modelli](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="12811-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="12811-127">Per un elenco di tutti i metodi e proprietà disponibili, vedere <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="12811-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="12811-128">Attributi</span><span class="sxs-lookup"><span data-stu-id="12811-128">Attributes</span></span>

<span data-ttu-id="12811-129">Lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc> include attributi che possono essere usati per configurare il comportamento del controller di API Web e i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="12811-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="12811-130">Nell'esempio seguente vengono utilizzati gli attributi per specificare il verbo di azione HTTP supportato e i codici di stato HTTP noti che possono essere restituiti:</span><span class="sxs-lookup"><span data-stu-id="12811-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="12811-131">Di seguito sono riportati altri esempi degli attributi disponibili.</span><span class="sxs-lookup"><span data-stu-id="12811-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="12811-132">Attributo</span><span class="sxs-lookup"><span data-stu-id="12811-132">Attribute</span></span>|<span data-ttu-id="12811-133">Note</span><span class="sxs-lookup"><span data-stu-id="12811-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="12811-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="12811-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="12811-135">Specifica il modello di URL per un controller o un'azione.</span><span class="sxs-lookup"><span data-stu-id="12811-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="12811-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="12811-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="12811-137">Specifica il prefisso e le proprietà da includere per l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="12811-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="12811-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="12811-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="12811-139">Identifica un'azione che supporta il verbo di azione HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="12811-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="12811-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="12811-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="12811-141">Specifica i tipi di dati accettati da un'azione.</span><span class="sxs-lookup"><span data-stu-id="12811-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="12811-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="12811-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="12811-143">Specifica i tipi di dati restituiti da un'azione.</span><span class="sxs-lookup"><span data-stu-id="12811-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="12811-144">Per un elenco che include gli attributi disponibili, vedere lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="12811-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="12811-145">Attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="12811-145">ApiController attribute</span></span>

<span data-ttu-id="12811-146">L'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) può essere applicato a una classe controller per abilitare i seguenti comportamenti supponenti specifici dell'API:</span><span class="sxs-lookup"><span data-stu-id="12811-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="12811-147">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="12811-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="12811-148">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="12811-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="12811-149">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="12811-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="12811-150">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="12811-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="12811-151">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="12811-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="12811-152">Queste funzionalità richiedono la [versione di compatibilità](xref:mvc/compatibility-version) 2.1 o successiva.</span><span class="sxs-lookup"><span data-stu-id="12811-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="12811-153">Attributo su controller specifici</span><span class="sxs-lookup"><span data-stu-id="12811-153">Attribute on specific controllers</span></span>

<span data-ttu-id="12811-154">L'attributo `[ApiController]` può essere applicato a controller specifici, come illustrato nell'esempio seguente dal modello di progetto:</span><span class="sxs-lookup"><span data-stu-id="12811-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="12811-155">Attributo su più controller</span><span class="sxs-lookup"><span data-stu-id="12811-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="12811-156">Un approccio per usare l'attributo su più di un controller consiste nel creare una classe di controller di base personalizzata annotata con l'attributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="12811-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="12811-157">Nell'esempio seguente viene illustrata una classe base personalizzata e un controller che deriva da esso:</span><span class="sxs-lookup"><span data-stu-id="12811-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="12811-158">Attributo in un assembly</span><span class="sxs-lookup"><span data-stu-id="12811-158">Attribute on an assembly</span></span>

<span data-ttu-id="12811-159">Se la [versione di compatibilità](xref:mvc/compatibility-version) è impostata su 2.2 o una versione successiva, l'attributo `[ApiController]` può essere applicato a un assembly.</span><span class="sxs-lookup"><span data-stu-id="12811-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="12811-160">L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="12811-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="12811-161">Non esiste alcun modo per rifiutare esplicitamente singoli controller.</span><span class="sxs-lookup"><span data-stu-id="12811-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="12811-162">Applicare l'attributo a livello di assembly alla dichiarazione dello spazio dei nomi che circonda la classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="12811-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="12811-163">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="12811-163">Attribute routing requirement</span></span>

<span data-ttu-id="12811-164">Con l'attributo `[ApiController]` il routing degli attributi è un requisito.</span><span class="sxs-lookup"><span data-stu-id="12811-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="12811-165">Esempio:</span><span class="sxs-lookup"><span data-stu-id="12811-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="12811-166">Le azioni sono inaccessibili tramite [Route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="12811-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="12811-167">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="12811-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="12811-168">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="12811-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="12811-169">Con l'attributo `[ApiController]` gli errori di convalida del modello attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="12811-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="12811-170">Di conseguenza è necessario il codice seguente in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="12811-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="12811-171">ASP.NET Core MVC usa il filtro azione <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> per eseguire il controllo precedente.</span><span class="sxs-lookup"><span data-stu-id="12811-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="12811-172">Risposta BadRequest predefinita</span><span class="sxs-lookup"><span data-stu-id="12811-172">Default BadRequest response</span></span>

<span data-ttu-id="12811-173">Con una versione di compatibilità 2,1, il tipo di risposta predefinito per una risposta HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="12811-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="12811-174">Il corpo della richiesta seguente è un esempio del tipo serializzato:</span><span class="sxs-lookup"><span data-stu-id="12811-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="12811-175">Con una versione di compatibilità 2,2 o successiva, il tipo di risposta predefinito per una risposta HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="12811-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="12811-176">Il corpo della richiesta seguente è un esempio del tipo serializzato:</span><span class="sxs-lookup"><span data-stu-id="12811-176">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="12811-177">Tipo di `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="12811-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="12811-178">Fornisce un formato leggibile dal computer per specificare gli errori nelle risposte all'API Web.</span><span class="sxs-lookup"><span data-stu-id="12811-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="12811-179">È conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="12811-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="12811-180">Registrare le risposte 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="12811-180">Log automatic 400 responses</span></span>

<span data-ttu-id="12811-181">Vedere [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) (Come registrare le risposte 400 automatiche negli errori di convalida del modello - aspnet/AspNetCore.Docs 12157).</span><span class="sxs-lookup"><span data-stu-id="12811-181">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="12811-182">Disabilitare la risposta automatica 400</span><span class="sxs-lookup"><span data-stu-id="12811-182">Disable automatic 400 response</span></span>

<span data-ttu-id="12811-183">Per disabilitare il comportamento automatico per gli errori 400, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> su `true`.</span><span class="sxs-lookup"><span data-stu-id="12811-183">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="12811-184">Aggiungere il codice evidenziato seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12811-184">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="12811-185">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="12811-185">Binding source parameter inference</span></span>

<span data-ttu-id="12811-186">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="12811-186">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="12811-187">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="12811-187">The following binding source attributes exist:</span></span>

|<span data-ttu-id="12811-188">Attributo</span><span class="sxs-lookup"><span data-stu-id="12811-188">Attribute</span></span>|<span data-ttu-id="12811-189">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="12811-189">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="12811-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="12811-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="12811-191">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="12811-191">Request body</span></span> |
|<span data-ttu-id="12811-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="12811-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="12811-193">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="12811-193">Form data in the request body</span></span> |
|<span data-ttu-id="12811-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="12811-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="12811-195">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="12811-195">Request header</span></span> |
|<span data-ttu-id="12811-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="12811-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="12811-197">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="12811-197">Request query string parameter</span></span> |
|<span data-ttu-id="12811-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="12811-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="12811-199">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="12811-199">Route data from the current request</span></span> |
|<span data-ttu-id="12811-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="12811-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="12811-201">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="12811-201">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="12811-202">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="12811-202">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="12811-203">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="12811-203">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="12811-204">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="12811-204">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="12811-205">Senza l'attributo `[ApiController]` o altri attributi di origine di associazione come `[FromQuery]`, il runtime di ASP.NET Core tenta di usare lo strumento di associazione di modelli a oggetti complesso.</span><span class="sxs-lookup"><span data-stu-id="12811-205">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="12811-206">Lo strumento di associazione di modelli a oggetti complesso estrae i dati dal provider di valori in un ordine definito.</span><span class="sxs-lookup"><span data-stu-id="12811-206">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="12811-207">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="12811-207">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="12811-208">L'attributo `[ApiController]` applica le regole di inferenza per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="12811-208">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="12811-209">Queste regole consentono di evitare di dover identificare le origini di associazione manualmente applicando attributi ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="12811-209">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="12811-210">Il comportamento delle regole di inferenza per le origini di associazione è il seguente:</span><span class="sxs-lookup"><span data-stu-id="12811-210">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="12811-211">`[FromBody]` viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12811-211">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="12811-212">Un'eccezione alla regola di inferenza `[FromBody]` è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="12811-212">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="12811-213">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="12811-213">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="12811-214">`[FromForm]` viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="12811-214">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="12811-215">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="12811-215">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="12811-216">`[FromRoute]` viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="12811-216">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="12811-217">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="12811-217">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="12811-218">`[FromQuery]` viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="12811-218">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="12811-219">Note sulla regola di inferenza FromBody</span><span class="sxs-lookup"><span data-stu-id="12811-219">FromBody inference notes</span></span>

<span data-ttu-id="12811-220">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="12811-220">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="12811-221">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="12811-221">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="12811-222">Quando a un'azione sono associati più parametri dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="12811-222">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="12811-223">Tutte le firme di metodo di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="12811-223">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="12811-224">`[FromBody]` dedotto per entrambi, perché si tratta di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="12811-224">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="12811-225">`[FromBody]` attributo per uno, dedotto per l'altro perché è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12811-225">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="12811-226">`[FromBody]` attributo per entrambi.</span><span class="sxs-lookup"><span data-stu-id="12811-226">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="12811-227">In ASP.NET Core 2.1, i parametri di tipo raccolta, come elenchi e matrici, vengono dedotti in modo errato come `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="12811-227">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="12811-228">È necessario usare l'attributo `[FromBody]` per questi parametri, se devono essere associati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="12811-228">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="12811-229">Questo comportamento è stato corretto in ASP.NET Core 2.2 o versioni successive, in cui i parametri di tipo raccolta vengono dedotti come associati dal corpo per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="12811-229">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="12811-230">Disabilitare le regole di inferenza</span><span class="sxs-lookup"><span data-stu-id="12811-230">Disable inference rules</span></span>

<span data-ttu-id="12811-231">Per disabilitare l'inferenza delle origini di associazione, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> su `true`.</span><span class="sxs-lookup"><span data-stu-id="12811-231">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="12811-232">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12811-232">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="12811-233">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="12811-233">Multipart/form-data request inference</span></span>

<span data-ttu-id="12811-234">L'attributo `[ApiController]` applica una regola di inferenza quando un parametro di azione viene annotato con l'attributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) .</span><span class="sxs-lookup"><span data-stu-id="12811-234">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="12811-235">Il tipo di contenuto della richiesta `multipart/form-data` viene dedotto.</span><span class="sxs-lookup"><span data-stu-id="12811-235">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="12811-236">Per disabilitare il comportamento predefinito, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> su `true` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12811-236">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="12811-237">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="12811-237">Problem details for error status codes</span></span>

<span data-ttu-id="12811-238">Con la versione di compatibilità 2.2 o successiva, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="12811-238">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="12811-239">Il tipo `ProblemDetails` si basa sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807) per fornire dettagli sull'errore leggibili dal computer in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="12811-239">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="12811-240">Si consideri il codice seguente in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="12811-240">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="12811-241">Il metodo `NotFound` genera un codice di stato HTTP 404 con un corpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="12811-241">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="12811-242">Esempio:</span><span class="sxs-lookup"><span data-stu-id="12811-242">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="12811-243">Disabilitare la risposta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="12811-243">Disable ProblemDetails response</span></span>

<span data-ttu-id="12811-244">La creazione automatica di un'istanza di `ProblemDetails` viene disabilitata quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="12811-244">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> property is set to `true`.</span></span> <span data-ttu-id="12811-245">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12811-245">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="12811-246">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="12811-246">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
