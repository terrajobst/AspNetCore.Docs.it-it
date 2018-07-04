---
title: Creare API Web con ASP.NET Core
author: scottaddie
description: Informazioni sulle funzionalità disponibili per la creazione di un'API Web in ASP.NET Core e su quando è appropriato usare ciascuna funzionalità.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274966"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="e2a07-103">Creare API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2a07-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="e2a07-104">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e2a07-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e2a07-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2a07-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e2a07-106">Questo documento illustra come creare un'API Web in ASP.NET Core e indica quando è più appropriato usare ciascuna funzionalità.</span><span class="sxs-lookup"><span data-stu-id="e2a07-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="e2a07-107">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="e2a07-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="e2a07-108">Ereditare dalla classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) in un controller progettato per svolgere la funzione di API Web.</span><span class="sxs-lookup"><span data-stu-id="e2a07-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="e2a07-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e2a07-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="e2a07-110">La classe `ControllerBase` consente l'accesso a numerose proprietà e a svariati metodi.</span><span class="sxs-lookup"><span data-stu-id="e2a07-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="e2a07-111">Nell'esempio precedente, alcuni metodi di questo tipo sono [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) e [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="e2a07-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="e2a07-112">Questi metodi vengono richiamati all'interno di metodi di azioni per restituire rispettivamente i codici di stato HTTP 400 e HTTP 201.</span><span class="sxs-lookup"><span data-stu-id="e2a07-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="e2a07-113">Alla proprietà [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), fornita anche questa da `ControllerBase`, si accede per eseguire la convalida del modello di richiesta.</span><span class="sxs-lookup"><span data-stu-id="e2a07-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="e2a07-114">Annotare una classe con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="e2a07-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="e2a07-115">ASP.NET Core 2.1 introduce l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) per indicare una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="e2a07-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="e2a07-116">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e2a07-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="e2a07-117">Questo attributo viene in genere associato a `ControllerBase` per accedere a proprietà e a metodi utili.</span><span class="sxs-lookup"><span data-stu-id="e2a07-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="e2a07-118">`ControllerBase` consente l'accesso a metodi quali [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) e [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="e2a07-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="e2a07-119">Un altro approccio consiste nel creare una classe controller di base personalizzata annotata con l'attributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="e2a07-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="e2a07-120">Le sezioni seguenti descrivono le funzionalità aggiunte dall'attributo che favoriscono una maggiore praticità.</span><span class="sxs-lookup"><span data-stu-id="e2a07-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="e2a07-121">Risposte HTTP 400 automatiche</span><span class="sxs-lookup"><span data-stu-id="e2a07-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="e2a07-122">Gli errori di convalida attivano automaticamente una risposta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="e2a07-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="e2a07-123">Il codice seguente non è più necessario nelle azioni:</span><span class="sxs-lookup"><span data-stu-id="e2a07-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="e2a07-124">Questo comportamento predefinito viene disabilitato con il codice seguente in *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="e2a07-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="e2a07-125">Inferenza del parametro di origine di associazione</span><span class="sxs-lookup"><span data-stu-id="e2a07-125">Binding source parameter inference</span></span>

<span data-ttu-id="e2a07-126">Un attributo di origine di associazione definisce la posizione in cui viene trovato il valore del parametro di un'azione.</span><span class="sxs-lookup"><span data-stu-id="e2a07-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="e2a07-127">Esistono gli attributi di origine di associazione seguente:</span><span class="sxs-lookup"><span data-stu-id="e2a07-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="e2a07-128">Attributo</span><span class="sxs-lookup"><span data-stu-id="e2a07-128">Attribute</span></span>|<span data-ttu-id="e2a07-129">Origine di associazione</span><span class="sxs-lookup"><span data-stu-id="e2a07-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="e2a07-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="e2a07-131">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="e2a07-131">Request body</span></span> |
|<span data-ttu-id="e2a07-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="e2a07-133">Dati di modulo nel corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="e2a07-133">Form data in the request body</span></span> |
|<span data-ttu-id="e2a07-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="e2a07-135">Intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="e2a07-135">Request header</span></span> |
|<span data-ttu-id="e2a07-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="e2a07-137">Parametri della stringa di query della richiesta</span><span class="sxs-lookup"><span data-stu-id="e2a07-137">Request query string parameter</span></span> |
|<span data-ttu-id="e2a07-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="e2a07-139">Dati della route della richiesta corrente</span><span class="sxs-lookup"><span data-stu-id="e2a07-139">Route data from the current request</span></span> |
|<span data-ttu-id="e2a07-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="e2a07-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="e2a07-141">Il servizio richiesta inserito come parametro di azione</span><span class="sxs-lookup"><span data-stu-id="e2a07-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="e2a07-142">**Non** usare `[FromRoute]` quando i valori potrebbero contenere `%2f` (vale a dire `/`) perché `%2f` non sarà convertito in `/` rimuovendo i caratteri di escape.</span><span class="sxs-lookup"><span data-stu-id="e2a07-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="e2a07-143">Usare `[FromQuery]` se il valore potrebbe contenere `%2f`.</span><span class="sxs-lookup"><span data-stu-id="e2a07-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="e2a07-144">Senza l'attributo `[ApiController]`, gli attributi di origine di associazione vengono definiti in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="e2a07-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="e2a07-145">Nell'esempio seguente, l'attributo `[FromQuery]` indica che il valore del parametro `discontinuedOnly` è specificato nella stringa di query dell'URL della richiesta:</span><span class="sxs-lookup"><span data-stu-id="e2a07-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="e2a07-146">Le regole di inferenza vengono applicate per le origini dati predefinite dei parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e2a07-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="e2a07-147">Queste regole configurano le origini di associazione altrimenti applicate manualmente con ogni probabilità ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e2a07-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="e2a07-148">Gli attributi di origine di associazione si comportano nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e2a07-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="e2a07-149">**[FromBody]**  viene dedotto per i parametri di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="e2a07-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="e2a07-150">Un'eccezione a questa regola è costituita dai tipi incorporati complessi con un significato speciale, ad esempio [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) e [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="e2a07-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="e2a07-151">Il codice di inferenza di origine di associazione ignora tali tipi speciali.</span><span class="sxs-lookup"><span data-stu-id="e2a07-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="e2a07-152">Quando per un'azione esistono più parametri, specificati in modo esplicito (tramite `[FromBody]`) o dedotti in quanto associati dal corpo della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="e2a07-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="e2a07-153">Le firme di azione seguenti, ad esempio, causano un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="e2a07-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="e2a07-154">**[FromForm]** viene dedotto per i parametri di azione di tipo [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="e2a07-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="e2a07-155">Non viene dedotto per i tipi semplici o definiti dall'utente.</span><span class="sxs-lookup"><span data-stu-id="e2a07-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="e2a07-156">**[FromRoute]**  viene dedotto per i nomi di parametro di azione corrispondenti a un parametro nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="e2a07-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="e2a07-157">Quando più route corrispondono a un parametro di azione, tutti i valori di route vengono considerati `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="e2a07-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="e2a07-158">**[FromQuery]**  viene dedotto per tutti gli altri parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e2a07-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="e2a07-159">Le regole di inferenza predefinite vengono disabilitate con il codice seguente in *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="e2a07-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="e2a07-160">Inferenza di richieste multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="e2a07-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="e2a07-161">Quando un parametro di azione è annotato con l'attributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), viene dedotto il tipo di contenuto `multipart/form-data` per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="e2a07-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="e2a07-162">Il comportamento predefinito viene disabilitato con il codice seguente in *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="e2a07-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="e2a07-163">Requisiti del routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="e2a07-163">Attribute routing requirement</span></span>

<span data-ttu-id="e2a07-164">Il routing degli attributi diventa un requisito.</span><span class="sxs-lookup"><span data-stu-id="e2a07-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="e2a07-165">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e2a07-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="e2a07-166">Le azioni non sono accessibili tramite le [route convenzionali](xref:mvc/controllers/routing#conventional-routing) definite in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) o da [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="e2a07-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e2a07-167">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e2a07-167">Additional resources</span></span>

* [<span data-ttu-id="e2a07-168">Tipi restituiti per le azioni del controller</span><span class="sxs-lookup"><span data-stu-id="e2a07-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="e2a07-169">Formattatori personalizzati</span><span class="sxs-lookup"><span data-stu-id="e2a07-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="e2a07-170">Formattare i dati di risposta</span><span class="sxs-lookup"><span data-stu-id="e2a07-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="e2a07-171">Pagine della Guida con Swagger</span><span class="sxs-lookup"><span data-stu-id="e2a07-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="e2a07-172">Routing ad azioni del controller</span><span class="sxs-lookup"><span data-stu-id="e2a07-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
