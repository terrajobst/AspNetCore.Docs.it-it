---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni di base sulla creazione di un'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 01/27/2020
uid: web-api/index
ms.openlocfilehash: 8609e2095c202643cdc905cc610298195b654215
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870017"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="91a87-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91a87-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="91a87-104">Di [Scott Addie](https://github.com/scottaddie) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="91a87-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="91a87-105">ASP.NET Core supporta la creazione di servizi RESTful, noti anche come API Web, con C#.</span><span class="sxs-lookup"><span data-stu-id="91a87-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="91a87-106">Per gestire le richieste, un'API Web usa i controller.</span><span class="sxs-lookup"><span data-stu-id="91a87-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="91a87-107">I *controller* in un'API Web sono classi che derivano da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="91a87-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="91a87-108">Questo articolo illustra come usare i controller per gestire le richieste API Web.</span><span class="sxs-lookup"><span data-stu-id="91a87-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="91a87-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="91a87-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="91a87-110">([Come scaricare un esempio](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="91a87-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="91a87-111">Classe ControllerBase</span><span class="sxs-lookup"><span data-stu-id="91a87-111">ControllerBase class</span></span>

<span data-ttu-id="91a87-112">Un'API Web è costituita da una o più classi controller che derivano da <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="91a87-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="91a87-113">Il modello di progetto API Web fornisce un controller Starter:</span><span class="sxs-lookup"><span data-stu-id="91a87-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="91a87-114">Non creare un controller API Web tramite derivazione dalla classe <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="91a87-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="91a87-115">La classe `Controller` deriva da `ControllerBase` e aggiunge il supporto per le visualizzazioni, pertanto è progettata per la gestione delle pagine Web e non per le richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="91a87-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="91a87-116">Si è verificata un'eccezione a questa regola: se si prevede di usare lo stesso controller per le visualizzazioni e le API Web, derivarlo dal `Controller`.</span><span class="sxs-lookup"><span data-stu-id="91a87-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="91a87-117">La classe `ControllerBase` offre molti metodi e proprietà utili per la gestione delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="91a87-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="91a87-118">Ad esempio, `ControllerBase.CreatedAtAction` restituisce un codice di stato 201:</span><span class="sxs-lookup"><span data-stu-id="91a87-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="91a87-119">Di seguito sono elencati alcuni esempi di metodi forniti da `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="91a87-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="91a87-120">Metodo</span><span class="sxs-lookup"><span data-stu-id="91a87-120">Method</span></span>   |<span data-ttu-id="91a87-121">Note</span><span class="sxs-lookup"><span data-stu-id="91a87-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="91a87-122">Restituisce il codice di stato 400.</span><span class="sxs-lookup"><span data-stu-id="91a87-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="91a87-123">Restituisce il codice di stato 404.</span><span class="sxs-lookup"><span data-stu-id="91a87-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="91a87-124">Restituisce un file.</span><span class="sxs-lookup"><span data-stu-id="91a87-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="91a87-125">Richiama l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="91a87-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="91a87-126">Richiama la [convalida dei modelli](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="91a87-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="91a87-127">Per un elenco di tutti i metodi e proprietà disponibili, vedere <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="91a87-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="91a87-128">Attributi</span><span class="sxs-lookup"><span data-stu-id="91a87-128">Attributes</span></span>

<span data-ttu-id="91a87-129">Lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc> include attributi che possono essere usati per configurare il comportamento del controller di API Web e i metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="91a87-130">Nell'esempio seguente vengono utilizzati gli attributi per specificare il verbo di azione HTTP supportato e i codici di stato HTTP noti che possono essere restituiti:</span><span class="sxs-lookup"><span data-stu-id="91a87-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="91a87-131">Di seguito sono riportati altri esempi degli attributi disponibili.</span><span class="sxs-lookup"><span data-stu-id="91a87-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="91a87-132">Attributo</span><span class="sxs-lookup"><span data-stu-id="91a87-132">Attribute</span></span>|<span data-ttu-id="91a87-133">Note</span><span class="sxs-lookup"><span data-stu-id="91a87-133">Notes</span></span>|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |<span data-ttu-id="91a87-134">Specifica il modello di URL per un controller o un'azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-134">Specifies URL pattern for a controller or action.</span></span>|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |<span data-ttu-id="91a87-135">Specifica il prefisso e le proprietà da includere per l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="91a87-135">Specifies prefix and properties to include for model binding.</span></span>|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |<span data-ttu-id="91a87-136">Identifica un'azione che supporta il verbo di azione HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="91a87-136">Identifies an action that supports the HTTP GET action verb.</span></span>|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|<span data-ttu-id="91a87-137">Specifica i tipi di dati accettati da un'azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-137">Specifies data types that an action accepts.</span></span>|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|<span data-ttu-id="91a87-138">Specifica i tipi di dati restituiti da un'azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-138">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="91a87-139">Per un elenco che include gli attributi disponibili, vedere lo spazio dei nomi <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="91a87-139">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="91a87-140">Attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="91a87-140">ApiController attribute</span></span>

<span data-ttu-id="91a87-141">L'attributo [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) può essere applicato a una classe controller per abilitare i seguenti comportamenti supponenti specifici dell'API:</span><span class="sxs-lookup"><span data-stu-id="91a87-141">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="91a87-142">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="91a87-142">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="91a87-143">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="91a87-143">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="91a87-144">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="91a87-144">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="91a87-145">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="91a87-145">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="91a87-146">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="91a87-146">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="91a87-147">La funzionalità *Dettagli problema per i codici di stato di errore* richiede una [versione di compatibilità](xref:mvc/compatibility-version) 2,2 o successiva.</span><span class="sxs-lookup"><span data-stu-id="91a87-147">The *Problem details for error status codes* feature requires a [compatibility version](xref:mvc/compatibility-version) of 2.2 or later.</span></span> <span data-ttu-id="91a87-148">Le altre funzionalità richiedono una versione di compatibilità 2,1 o successiva.</span><span class="sxs-lookup"><span data-stu-id="91a87-148">The other features require a compatibility version of 2.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

* [<span data-ttu-id="91a87-149">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="91a87-149">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="91a87-150">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="91a87-150">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="91a87-151">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="91a87-151">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="91a87-152">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="91a87-152">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)

<span data-ttu-id="91a87-153">Queste funzionalità richiedono la [versione di compatibilità](xref:mvc/compatibility-version) 2.1 o successiva.</span><span class="sxs-lookup"><span data-stu-id="91a87-153">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

::: moniker-end

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="91a87-154">Attributo su controller specifici</span><span class="sxs-lookup"><span data-stu-id="91a87-154">Attribute on specific controllers</span></span>

<span data-ttu-id="91a87-155">L'attributo `[ApiController]` può essere applicato a controller specifici, come illustrato nell'esempio seguente dal modello di progetto:</span><span class="sxs-lookup"><span data-stu-id="91a87-155">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="91a87-156">Attributo su più controller</span><span class="sxs-lookup"><span data-stu-id="91a87-156">Attribute on multiple controllers</span></span>

<span data-ttu-id="91a87-157">Un approccio per usare l'attributo su più di un controller consiste nel creare una classe di controller di base personalizzata annotata con l'attributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="91a87-157">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="91a87-158">Nell'esempio seguente viene illustrata una classe base personalizzata e un controller che deriva da esso:</span><span class="sxs-lookup"><span data-stu-id="91a87-158">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="91a87-159">Attributo in un assembly</span><span class="sxs-lookup"><span data-stu-id="91a87-159">Attribute on an assembly</span></span>

<span data-ttu-id="91a87-160">Se la [versione di compatibilità](xref:mvc/compatibility-version) è impostata su 2.2 o una versione successiva, l'attributo `[ApiController]` può essere applicato a un assembly.</span><span class="sxs-lookup"><span data-stu-id="91a87-160">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="91a87-161">L'uso delle annotazioni in questo modo applica il comportamento delle API Web a tutti i controller nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="91a87-161">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="91a87-162">Non esiste alcun modo per rifiutare esplicitamente singoli controller.</span><span class="sxs-lookup"><span data-stu-id="91a87-162">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="91a87-163">Applicare l'attributo a livello di assembly alla dichiarazione dello spazio dei nomi che circonda la classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="91a87-163">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="91a87-164">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="91a87-164">Attribute routing requirement</span></span>

<span data-ttu-id="91a87-165">Con l'attributo `[ApiController]` il routing degli attributi è un requisito.</span><span class="sxs-lookup"><span data-stu-id="91a87-165">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="91a87-166">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="91a87-166">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="91a87-167">Le azioni sono inaccessibili tramite [Route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="91a87-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="91a87-168">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="91a87-168">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="91a87-169">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="91a87-169">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="91a87-170">Con l'attributo `[ApiController]` gli errori di convalida del modello attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="91a87-170">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="91a87-171">Di conseguenza è necessario il codice seguente in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="91a87-171">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="91a87-172">ASP.NET Core MVC usa il filtro azione <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> per eseguire il controllo precedente.</span><span class="sxs-lookup"><span data-stu-id="91a87-172">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="91a87-173">Risposta BadRequest predefinita</span><span class="sxs-lookup"><span data-stu-id="91a87-173">Default BadRequest response</span></span>

<span data-ttu-id="91a87-174">Con una versione di compatibilità 2,1, il tipo di risposta predefinito per una risposta HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="91a87-174">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="91a87-175">Il corpo della richiesta seguente è un esempio del tipo serializzato:</span><span class="sxs-lookup"><span data-stu-id="91a87-175">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="91a87-176">Con una versione di compatibilità 2,2 o successiva, il tipo di risposta predefinito per una risposta HTTP 400 è <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="91a87-176">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="91a87-177">Il corpo della richiesta seguente è un esempio del tipo serializzato:</span><span class="sxs-lookup"><span data-stu-id="91a87-177">The following request body is an example of the serialized type:</span></span>

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

<span data-ttu-id="91a87-178">Tipo di `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="91a87-178">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="91a87-179">Fornisce un formato leggibile dal computer per specificare gli errori nelle risposte all'API Web.</span><span class="sxs-lookup"><span data-stu-id="91a87-179">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="91a87-180">È conforme alla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="91a87-180">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="91a87-181">Registrare le risposte 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="91a87-181">Log automatic 400 responses</span></span>

<span data-ttu-id="91a87-182">Vedere [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) (Come registrare le risposte 400 automatiche negli errori di convalida del modello - aspnet/AspNetCore.Docs 12157).</span><span class="sxs-lookup"><span data-stu-id="91a87-182">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="91a87-183">Disabilitare la risposta automatica 400</span><span class="sxs-lookup"><span data-stu-id="91a87-183">Disable automatic 400 response</span></span>

<span data-ttu-id="91a87-184">Per disabilitare il comportamento automatico per gli errori 400, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> su `true`.</span><span class="sxs-lookup"><span data-stu-id="91a87-184">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="91a87-185">Aggiungere il codice evidenziato seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="91a87-185">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="91a87-186">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="91a87-186">Binding source parameter inference</span></span>

<span data-ttu-id="91a87-187">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-187">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="91a87-188">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="91a87-188">The following binding source attributes exist:</span></span>

|<span data-ttu-id="91a87-189">Attributo</span><span class="sxs-lookup"><span data-stu-id="91a87-189">Attribute</span></span>|<span data-ttu-id="91a87-190">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="91a87-190">Binding source</span></span> |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | <span data-ttu-id="91a87-191">Testo della richiesta</span><span class="sxs-lookup"><span data-stu-id="91a87-191">Request body</span></span> |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | <span data-ttu-id="91a87-192">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="91a87-192">Form data in the request body</span></span> |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | <span data-ttu-id="91a87-193">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="91a87-193">Request header</span></span> |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | <span data-ttu-id="91a87-194">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="91a87-194">Request query string parameter</span></span> |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | <span data-ttu-id="91a87-195">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="91a87-195">Route data from the current request</span></span> |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | <span data-ttu-id="91a87-196">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="91a87-196">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="91a87-197">Non usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`).</span><span class="sxs-lookup"><span data-stu-id="91a87-197">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="91a87-198">`%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="91a87-198">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="91a87-199">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="91a87-199">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="91a87-200">Senza l'attributo `[ApiController]` o altri attributi di origine di associazione come `[FromQuery]`, il runtime di ASP.NET Core tenta di usare lo strumento di associazione di modelli a oggetti complesso.</span><span class="sxs-lookup"><span data-stu-id="91a87-200">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="91a87-201">Lo strumento di associazione di modelli a oggetti complesso estrae i dati dal provider di valori in un ordine definito.</span><span class="sxs-lookup"><span data-stu-id="91a87-201">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="91a87-202">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="91a87-202">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="91a87-203">L'attributo `[ApiController]` applica le regole di inferenza per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-203">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="91a87-204">Queste regole consentono di evitare di dover identificare le origini di associazione manualmente applicando attributi ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-204">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="91a87-205">Il comportamento delle regole di inferenza per le origini di associazione è il seguente:</span><span class="sxs-lookup"><span data-stu-id="91a87-205">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="91a87-206">`[FromBody]` viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="91a87-206">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="91a87-207">Un'eccezione alla regola di inferenza `[FromBody]` è costituita dai tipi predefiniti complessi con un significato speciale, ad esempio <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="91a87-207">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="91a87-208">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="91a87-208">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="91a87-209">`[FromForm]` viene dedotto per i parametri di azione di tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="91a87-209">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="91a87-210">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="91a87-210">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="91a87-211">`[FromRoute]` viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="91a87-211">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="91a87-212">Quando più di una route corrisponde a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="91a87-212">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="91a87-213">`[FromQuery]` viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="91a87-213">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="91a87-214">Note sulla regola di inferenza FromBody</span><span class="sxs-lookup"><span data-stu-id="91a87-214">FromBody inference notes</span></span>

<span data-ttu-id="91a87-215">`[FromBody]` non viene dedotto per i tipi semplici, ad esempio `string` o `int`.</span><span class="sxs-lookup"><span data-stu-id="91a87-215">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="91a87-216">Pertanto, l'attributo `[FromBody]` deve essere usato per i tipi semplici se è necessaria tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="91a87-216">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="91a87-217">Quando a un'azione sono associati più parametri dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="91a87-217">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="91a87-218">Tutte le firme di metodo di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="91a87-218">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="91a87-219">`[FromBody]` dedotto per entrambi, perché si tratta di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="91a87-219">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="91a87-220">`[FromBody]` attributo per uno, dedotto per l'altro perché è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="91a87-220">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="91a87-221">`[FromBody]` attributo per entrambi.</span><span class="sxs-lookup"><span data-stu-id="91a87-221">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="91a87-222">In ASP.NET Core 2.1, i parametri di tipo raccolta, come elenchi e matrici, vengono dedotti in modo errato come `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="91a87-222">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="91a87-223">È necessario usare l'attributo `[FromBody]` per questi parametri, se devono essere associati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="91a87-223">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="91a87-224">Questo comportamento è stato corretto in ASP.NET Core 2.2 o versioni successive, in cui i parametri di tipo raccolta vengono dedotti come associati dal corpo per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="91a87-224">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="91a87-225">Disabilitare le regole di inferenza</span><span class="sxs-lookup"><span data-stu-id="91a87-225">Disable inference rules</span></span>

<span data-ttu-id="91a87-226">Per disabilitare l'inferenza delle origini di associazione, impostare <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> su `true`.</span><span class="sxs-lookup"><span data-stu-id="91a87-226">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="91a87-227">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="91a87-227">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="91a87-228">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="91a87-228">Multipart/form-data request inference</span></span>

<span data-ttu-id="91a87-229">L'attributo `[ApiController]` applica una regola di inferenza quando un parametro di azione viene annotato con l'attributo [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) .</span><span class="sxs-lookup"><span data-stu-id="91a87-229">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="91a87-230">Il tipo di contenuto della richiesta `multipart/form-data` viene dedotto.</span><span class="sxs-lookup"><span data-stu-id="91a87-230">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="91a87-231">Per disabilitare il comportamento predefinito, impostare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> su `true` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="91a87-231">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="91a87-232">Dettagli del problema per i codici di stato di errore</span><span class="sxs-lookup"><span data-stu-id="91a87-232">Problem details for error status codes</span></span>

<span data-ttu-id="91a87-233">Con la versione di compatibilità 2.2 o successiva, MVC consente di trasformare un risultato di errore (un risultato con codice di stato 400 o superiore) in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="91a87-233">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="91a87-234">Il tipo `ProblemDetails` si basa sulla [specifica RFC 7807](https://tools.ietf.org/html/rfc7807) per fornire dettagli sull'errore leggibili dal computer in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="91a87-234">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="91a87-235">Si consideri il codice seguente in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="91a87-235">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="91a87-236">Il metodo `NotFound` genera un codice di stato HTTP 404 con un corpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="91a87-236">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="91a87-237">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="91a87-237">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="91a87-238">Disabilitare la risposta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="91a87-238">Disable ProblemDetails response</span></span>

<span data-ttu-id="91a87-239">La creazione automatica di un'istanza di `ProblemDetails` viene disabilitata quando la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="91a87-239">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="91a87-240">Aggiungere il codice seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="91a87-240">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="91a87-241">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="91a87-241">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
