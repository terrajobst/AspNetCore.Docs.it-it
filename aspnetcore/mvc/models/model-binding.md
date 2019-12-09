---
title: Associazione di modelli in ASP.NET Core
author: rick-anderson
description: Informazioni su come funziona l'associazione di modelli in ASP.NET Core e su come personalizzarne il comportamento.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/21/2019
uid: mvc/models/model-binding
ms.openlocfilehash: da6cc25e0bbb1b2301529b34eab4c91f9ccb46eb
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944295"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="9b896-103">Associazione di modelli in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b896-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9b896-104">Questo articolo illustra cos'è l'associazione di modelli, come funziona e come personalizzarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="9b896-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="9b896-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9b896-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="9b896-106">Che cos'è l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9b896-106">What is Model binding</span></span>

<span data-ttu-id="9b896-107">I controller e le pagine Razor operano sui dati provenienti dalle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="9b896-108">Ad esempio, i dati di route possono fornire una chiave del record e i campi modulo inviati possono fornire valori per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="9b896-109">La scrittura di codice per recuperare tutti i valori e convertirli da stringhe in tipi .NET sarebbe noiosa e soggetta a errori.</span><span class="sxs-lookup"><span data-stu-id="9b896-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="9b896-110">L'associazione di modelli consente di automatizzare questo processo.</span><span class="sxs-lookup"><span data-stu-id="9b896-110">Model binding automates this process.</span></span> <span data-ttu-id="9b896-111">Il sistema di associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="9b896-111">The model binding system:</span></span>

* <span data-ttu-id="9b896-112">Recupera i dati da diverse origini, ad esempio i dati di route, i campi modulo e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="9b896-113">Fornisce i dati ai controller e alle pagine Razor in parametri di metodo e proprietà pubbliche.</span><span class="sxs-lookup"><span data-stu-id="9b896-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="9b896-114">Converte dati stringa in tipi .NET.</span><span class="sxs-lookup"><span data-stu-id="9b896-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="9b896-115">Aggiorna le proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="9b896-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="9b896-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="9b896-116">Example</span></span>

<span data-ttu-id="9b896-117">Si supponga di avere il metodo di azione seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="9b896-118">E l'app riceve una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="9b896-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="9b896-119">L'associazione di modelli esegue i passaggi seguenti dopo che il sistema di routing seleziona il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="9b896-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="9b896-120">Trova il primo parametro di `GetByID`, un intero denominato `id`.</span><span class="sxs-lookup"><span data-stu-id="9b896-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="9b896-121">Esamina le origini disponibili nella richiesta HTTP e trova `id` = "2" nei dati di route.</span><span class="sxs-lookup"><span data-stu-id="9b896-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="9b896-122">Converte la stringa "2" nell'intero 2.</span><span class="sxs-lookup"><span data-stu-id="9b896-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="9b896-123">Trova il parametro successivo di `GetByID`, un valore booleano denominato `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="9b896-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="9b896-124">Esamina le origini e trova "DogsOnly=true" nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="9b896-125">Per la corrispondenza dei nomi non viene applicata la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="9b896-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="9b896-126">Converte la stringa "true" nel valore booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="9b896-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="9b896-127">Il framework chiama quindi il metodo `GetById`, passando 2 per il parametro `id` e `true` per il parametro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="9b896-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="9b896-128">Nell'esempio precedente le destinazioni dell'associazione di modelli sono parametri di metodo che sono tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="9b896-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="9b896-129">Le destinazioni possono essere anche le proprietà di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="9b896-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="9b896-130">Dopo l'associazione di ogni proprietà, viene eseguita la [convalida dei modelli](xref:mvc/models/validation) per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="9b896-131">Il record dei dati associati al modello e di eventuali errori di associazione o convalida viene archiviato in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) oppure [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="9b896-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="9b896-132">Per scoprire se questo processo ha esito positivo, l'app controlla il flag [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="9b896-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="9b896-133">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="9b896-133">Targets</span></span>

<span data-ttu-id="9b896-134">L'associazione di modelli cerca di trovare i valori per i tipi di destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="9b896-135">Parametri del metodo di azione del controller a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="9b896-136">Parametri del metodo del gestore di Razor Pages a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="9b896-137">Proprietà pubbliche di un controller o una classe `PageModel`, se specificate dagli attributi.</span><span class="sxs-lookup"><span data-stu-id="9b896-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="9b896-138">Attributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="9b896-138">[BindProperty] attribute</span></span>

<span data-ttu-id="9b896-139">Può essere applicato a una proprietà pubblica di un controller o una classe `PageModel` per fare in modo che l'associazione di modelli usi tale proprietà come destinazione:</span><span class="sxs-lookup"><span data-stu-id="9b896-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="9b896-140">Attributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="9b896-140">[BindProperties] attribute</span></span>

<span data-ttu-id="9b896-141">Disponibile in ASP.NET Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="9b896-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="9b896-142">Può essere applicato a un controller o una classe `PageModel` per indicare all'associazione del modello di usare tutte le proprietà pubbliche della classe come destinazione:</span><span class="sxs-lookup"><span data-stu-id="9b896-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="9b896-143">Associazione di modelli per le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="9b896-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="9b896-144">Per impostazione predefinita, le proprietà non vengono associate per le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="9b896-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="9b896-145">In genere, per una richiesta GET è sufficiente un parametro ID record.</span><span class="sxs-lookup"><span data-stu-id="9b896-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="9b896-146">L'ID record viene usato per cercare l'elemento nel database.</span><span class="sxs-lookup"><span data-stu-id="9b896-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="9b896-147">Pertanto, non è necessario associare una proprietà che contiene un'istanza del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="9b896-148">Negli scenari in cui si vogliono associare le proprietà ai dati dalle richieste GET, impostare la proprietà `SupportsGet` su `true`:</span><span class="sxs-lookup"><span data-stu-id="9b896-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="9b896-149">Origini</span><span class="sxs-lookup"><span data-stu-id="9b896-149">Sources</span></span>

<span data-ttu-id="9b896-150">Per impostazione predefinita, l'associazione di modelli ottiene i dati sotto forma di coppie chiave-valore dalle origini seguenti in una richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="9b896-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="9b896-151">Campi modulo</span><span class="sxs-lookup"><span data-stu-id="9b896-151">Form fields</span></span>
1. <span data-ttu-id="9b896-152">Il corpo della richiesta (per i controller [ con l'attributo [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="9b896-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="9b896-153">Dati della route</span><span class="sxs-lookup"><span data-stu-id="9b896-153">Route data</span></span>
1. <span data-ttu-id="9b896-154">Parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="9b896-154">Query string parameters</span></span>
1. <span data-ttu-id="9b896-155">File caricati</span><span class="sxs-lookup"><span data-stu-id="9b896-155">Uploaded files</span></span>

<span data-ttu-id="9b896-156">Per ogni parametro o proprietà di destinazione, le origini vengono analizzate nell'ordine indicato nell'elenco precedente.</span><span class="sxs-lookup"><span data-stu-id="9b896-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="9b896-157">Esistono tuttavia alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="9b896-157">There are a few exceptions:</span></span>

* <span data-ttu-id="9b896-158">I dati di route e i valori delle stringhe di query vengono usati solo per i tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="9b896-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="9b896-159">I file caricati vengono associati solo ai tipi di destinazione che implementano `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="9b896-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="9b896-160">Se l'origine predefinita non è corretta, usare uno degli attributi seguenti per specificare l'origine:</span><span class="sxs-lookup"><span data-stu-id="9b896-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="9b896-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -ottiene i valori dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="9b896-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -ottiene i valori dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="9b896-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="9b896-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -ottiene i valori dai campi del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="9b896-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -ottiene i valori dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="9b896-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -ottiene i valori dalle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="9b896-166">Questi attributi:</span><span class="sxs-lookup"><span data-stu-id="9b896-166">These attributes:</span></span>

* <span data-ttu-id="9b896-167">Vengono aggiunti singolarmente alle proprietà del modello (non alla classe del modello), come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="9b896-168">Accettano facoltativamente un valore di nome di modello nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="9b896-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="9b896-169">Questa opzione è disponibile nel caso in cui il nome della proprietà non corrisponda al valore nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="9b896-170">Il valore nella richiesta, ad esempio, potrebbe essere un'intestazione con un segno meno nel nome, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="9b896-171">Attributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="9b896-171">[FromBody] attribute</span></span>

<span data-ttu-id="9b896-172">Applicare l'attributo `[FromBody]` a un parametro per popolarne le proprietà dal corpo di una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="9b896-173">Il runtime di ASP.NET Core delega la responsabilità della lettura del corpo a un formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="9b896-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="9b896-174">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="9b896-175">Quando `[FromBody]` viene applicato a un parametro di tipo complesso, tutti gli attributi di origine di associazione applicati alle relative proprietà vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="9b896-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="9b896-176">Ad esempio, il `Create` azione seguente specifica che il relativo parametro `pet` viene popolato dal corpo:</span><span class="sxs-lookup"><span data-stu-id="9b896-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="9b896-177">La classe `Pet` specifica che la relativa proprietà `Breed` viene popolata da un parametro della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="9b896-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="9b896-178">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="9b896-178">In the preceding example:</span></span>

* <span data-ttu-id="9b896-179">L'attributo `[FromQuery]` viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="9b896-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="9b896-180">La proprietà `Breed` non viene popolata da un parametro della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="9b896-181">I formattatori di input leggono solo il corpo e non comprendono gli attributi di origine del binding.</span><span class="sxs-lookup"><span data-stu-id="9b896-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="9b896-182">Se nel corpo viene trovato un valore appropriato, tale valore viene usato per popolare la proprietà `Breed`.</span><span class="sxs-lookup"><span data-stu-id="9b896-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="9b896-183">Non applicare `[FromBody]` a più di un parametro per ogni metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="9b896-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="9b896-184">Quando il flusso di richieste viene letto da un formattatore di input, non è più disponibile per la lettura per l'associazione di altri parametri `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="9b896-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="9b896-185">Origini aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9b896-185">Additional sources</span></span>

<span data-ttu-id="9b896-186">I dati di origine vengono forniti al sistema di associazione di modelli dai *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="9b896-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="9b896-187">È possibile scrivere e registrare i provider di valori personalizzati che recuperano i dati per l'associazione di modelli da altre origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="9b896-188">Ad esempio, potrebbe essere necessario recuperare dati dai cookie o dallo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="9b896-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="9b896-189">Per ottenere dati da una nuova origine:</span><span class="sxs-lookup"><span data-stu-id="9b896-189">To get data from a new source:</span></span>

* <span data-ttu-id="9b896-190">Creare una classe che implementi `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="9b896-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="9b896-191">Creare una classe che implementi `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="9b896-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="9b896-192">Registrare la classe factory in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="9b896-193">L'app di esempio include un esempio di [provider di valori](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) e [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) che ottiene i valori dai cookie.</span><span class="sxs-lookup"><span data-stu-id="9b896-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="9b896-194">Ecco il codice di registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9b896-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="9b896-195">Il codice illustrato posiziona il provider di valori personalizzati dopo tutti i provider di valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="9b896-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="9b896-196">Per renderlo il primo nell'elenco, chiamare `Insert(0, new CookieValueProviderFactory())` invece di `Add`.</span><span class="sxs-lookup"><span data-stu-id="9b896-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="9b896-197">Nessuna origine per una proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="9b896-197">No source for a model property</span></span>

<span data-ttu-id="9b896-198">Per impostazione predefinita, non viene creato un errore di stato del modello se non viene trovato alcun valore per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="9b896-199">La proprietà viene impostata su Null o su un valore predefinito:</span><span class="sxs-lookup"><span data-stu-id="9b896-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="9b896-200">I tipi semplici nullable vengono impostati su `null`.</span><span class="sxs-lookup"><span data-stu-id="9b896-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="9b896-201">I tipi valore non nullable vengono impostati su `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="9b896-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="9b896-202">Ad esempio, un parametro `int id` viene impostato su 0.</span><span class="sxs-lookup"><span data-stu-id="9b896-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="9b896-203">Per i tipi complessi, l'associazione di modelli crea un'istanza usando il costruttore predefinito, senza impostare proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="9b896-204">Le matrici vengono impostate su `Array.Empty<T>()`, ad eccezione delle matrici `byte[]` che vengono impostate su `null`.</span><span class="sxs-lookup"><span data-stu-id="9b896-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="9b896-205">Se lo stato del modello deve essere invalidato quando non viene trovato alcun elemento nei campi del modulo per una proprietà del modello, usare l'attributo [`[BindRequired]`](#bindrequired-attribute) .</span><span class="sxs-lookup"><span data-stu-id="9b896-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="9b896-206">Si noti che questo comportamento `[BindRequired]` si applica all'associazione di modelli dai dati di moduli inviati e non ai dati JSON o XML nel corpo di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="9b896-207">I dati del corpo della richiesta vengono gestiti dai [formattatori di input](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="9b896-208">Errori di conversione dei tipi</span><span class="sxs-lookup"><span data-stu-id="9b896-208">Type conversion errors</span></span>

<span data-ttu-id="9b896-209">Se viene trovata un'origine ma non può essere convertita nel tipo di destinazione, lo stato del modello viene contrassegnato come non valido.</span><span class="sxs-lookup"><span data-stu-id="9b896-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="9b896-210">Il parametro o la proprietà di destinazione viene impostato su Null o su un valore predefinito, come indicato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="9b896-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="9b896-211">In un controller API con l'attributo `[ApiController]`, uno stato del modello non valido genera una risposta HTTP 400 automatica.</span><span class="sxs-lookup"><span data-stu-id="9b896-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="9b896-212">In una pagina Razor visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="9b896-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="9b896-213">La convalida sul lato client consente di rilevare la maggior parte dei dati non validi che, in caso contrario, verrebbero inviati a un modulo di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9b896-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="9b896-214">Questa convalida rende difficile attivare il codice sopra evidenziato.</span><span class="sxs-lookup"><span data-stu-id="9b896-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="9b896-215">L'app di esempio include un pulsante **Submit with Invalid Date** (Invia con data non valida) che inserisce dati non validi nel campo **Hire Date** (Data assunzione) e invia il modulo.</span><span class="sxs-lookup"><span data-stu-id="9b896-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="9b896-216">Questo pulsante illustra il funzionamento del codice per visualizzare di nuovo la pagina quando si verificano errori di conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="9b896-217">Quando la pagina viene nuovamente visualizzata dal codice precedente, l'input non valido non viene visualizzato nel campo modulo.</span><span class="sxs-lookup"><span data-stu-id="9b896-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="9b896-218">Questo avviene perché la proprietà del modello è stata impostata su Null o su un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="9b896-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="9b896-219">L'input non valido viene visualizzato in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="9b896-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="9b896-220">Ma se si vogliono visualizzare di nuovo i dati non validi nel campo modulo, è consigliabile impostare la proprietà del modello come stringa ed eseguire manualmente la conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="9b896-221">La stessa strategia è consigliata se si vuole evitare che gli errori di conversione del tipo causino errori di stato del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="9b896-222">In tal caso, impostare la proprietà del modello come stringa.</span><span class="sxs-lookup"><span data-stu-id="9b896-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="9b896-223">Tipi semplici</span><span class="sxs-lookup"><span data-stu-id="9b896-223">Simple types</span></span>

<span data-ttu-id="9b896-224">I tipi semplici in cui lo strumento di associazione di modelli può convertire le stringhe di origine includono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="9b896-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="9b896-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="9b896-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="9b896-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="9b896-227">Char</span><span class="sxs-lookup"><span data-stu-id="9b896-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="9b896-228">DateTime</span><span class="sxs-lookup"><span data-stu-id="9b896-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="9b896-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9b896-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="9b896-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="9b896-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="9b896-231">Double</span><span class="sxs-lookup"><span data-stu-id="9b896-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="9b896-232">Enum</span><span class="sxs-lookup"><span data-stu-id="9b896-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="9b896-233">Guid</span><span class="sxs-lookup"><span data-stu-id="9b896-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="9b896-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="9b896-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="9b896-235">Single</span><span class="sxs-lookup"><span data-stu-id="9b896-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="9b896-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9b896-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="9b896-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="9b896-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="9b896-238">Uri</span><span class="sxs-lookup"><span data-stu-id="9b896-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="9b896-239">Version</span><span class="sxs-lookup"><span data-stu-id="9b896-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="9b896-240">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="9b896-240">Complex types</span></span>

<span data-ttu-id="9b896-241">Un tipo pubblico deve avere un costruttore predefinito pubblico e proprietà scrivibili pubbliche da associare.</span><span class="sxs-lookup"><span data-stu-id="9b896-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="9b896-242">Quando si verifica l'associazione di modelli, vengono create istanze della classe usando il costruttore predefinito pubblico.</span><span class="sxs-lookup"><span data-stu-id="9b896-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="9b896-243">Per ogni proprietà del tipo complesso, l'associazione di modelli cerca il modello di nome *prefisso.nome_proprietà* nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="9b896-244">Se non viene trovato, cerca semplicemente *nome_proprietà* senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="9b896-245">Per l'associazione a un parametro, il prefisso è il nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="9b896-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="9b896-246">Per l'associazione a una proprietà pubblica `PageModel`, il prefisso è il nome della proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="9b896-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="9b896-247">Alcuni attributi hanno una proprietà `Prefix` che consente di eseguire l'override dell'utilizzo predefinito del nome di un parametro o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="9b896-248">Ad esempio, si supponga che il tipo complesso sia la classe `Instructor` seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="9b896-249">Prefisso = nome del parametro</span><span class="sxs-lookup"><span data-stu-id="9b896-249">Prefix = parameter name</span></span>

<span data-ttu-id="9b896-250">Se il modello da associare è un parametro denominato `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="9b896-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="9b896-251">L'associazione di modelli cerca prima di tutto la chiave `instructorToUpdate.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="9b896-252">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="9b896-253">Prefisso = nome della proprietà</span><span class="sxs-lookup"><span data-stu-id="9b896-253">Prefix = property name</span></span>

<span data-ttu-id="9b896-254">Se il modello da associare è una proprietà denominata `Instructor` del controller o della classe `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="9b896-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="9b896-255">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="9b896-256">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="9b896-257">Prefisso personalizzato</span><span class="sxs-lookup"><span data-stu-id="9b896-257">Custom prefix</span></span>

<span data-ttu-id="9b896-258">Se il modello da associare è un parametro denominato `instructorToUpdate` e un attributo `Bind` specifica `Instructor` come prefisso:</span><span class="sxs-lookup"><span data-stu-id="9b896-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="9b896-259">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="9b896-260">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="9b896-261">Attributi per le destinazioni di tipo complesso</span><span class="sxs-lookup"><span data-stu-id="9b896-261">Attributes for complex type targets</span></span>

<span data-ttu-id="9b896-262">Sono disponibili vari attributi predefiniti per controllare l'associazione di modelli di tipi complessi:</span><span class="sxs-lookup"><span data-stu-id="9b896-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="9b896-263">Questi attributi influiscono sul modello di associazione quando i dati di modulo inviati sono l'origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="9b896-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="9b896-264">Non influiscono sui formattatori di input, che elaborano corpi di richieste JSON e XML inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="9b896-265">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="9b896-266">Vedere anche la discussione relativa all'attributo `[Required]` in [Convalida del modello](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="9b896-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="9b896-267">Attributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="9b896-267">[BindRequired] attribute</span></span>

<span data-ttu-id="9b896-268">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="9b896-269">Con questo attributo l'associazione di modelli aggiunge un errore di stato del modello se non è possibile eseguire l'associazione per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="9b896-270">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="9b896-271">Attributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="9b896-271">[BindNever] attribute</span></span>

<span data-ttu-id="9b896-272">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="9b896-273">Impedisce all'associazione di modelli di impostare una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="9b896-274">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="9b896-275">Attributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="9b896-275">[Bind] attribute</span></span>

<span data-ttu-id="9b896-276">Può essere applicato a una classe o a un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="9b896-277">Specifica quali proprietà di un modello devono essere incluse nell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="9b896-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="9b896-278">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato qualsiasi gestore o metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="9b896-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="9b896-279">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato il metodo `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="9b896-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="9b896-280">L'attributo `[Bind]` può essere usato per evitare l'overposting negli scenari di *creazione*.</span><span class="sxs-lookup"><span data-stu-id="9b896-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="9b896-281">Non funziona bene negli scenari di modifica perché le proprietà escluse vengono impostate su Null o su un valore predefinito anziché rimanere inalterate.</span><span class="sxs-lookup"><span data-stu-id="9b896-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="9b896-282">Per difendersi dall'overposting, sono consigliati i modelli di visualizzazione anziché l'attributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="9b896-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="9b896-283">Per altre informazioni, vedere [Nota sulla sicurezza relativa all'overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="9b896-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="9b896-284">Raccolte</span><span class="sxs-lookup"><span data-stu-id="9b896-284">Collections</span></span>

<span data-ttu-id="9b896-285">Per le destinazioni che sono raccolte di tipi semplici, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="9b896-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="9b896-286">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="9b896-287">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-287">For example:</span></span>

* <span data-ttu-id="9b896-288">Si supponga che il parametro da associare sia una matrice denominata `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="9b896-289">I dati di modulo o di stringhe di query possono essere in uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-289">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="9b896-290">Il formato seguente è supportato solo nei dati di modulo:</span><span class="sxs-lookup"><span data-stu-id="9b896-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="9b896-291">Per tutti i formati di esempio precedenti, l'associazione di modelli passa una matrice di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="9b896-292">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="9b896-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="9b896-293">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="9b896-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="9b896-294">Per i formati di dati che usano numeri con indice (... [0]... [1]...) è necessario assicurarsi che la numerazione sia progressiva a partire da zero.</span><span class="sxs-lookup"><span data-stu-id="9b896-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="9b896-295">Se sono presenti eventuali interruzioni nella numerazione con indice, tutti gli elementi dopo l'interruzione vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="9b896-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="9b896-296">Ad esempio, se gli indici sono 0 e 2 anziché 0 e 1, il secondo elemento viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="9b896-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="9b896-297">Dizionari</span><span class="sxs-lookup"><span data-stu-id="9b896-297">Dictionaries</span></span>

<span data-ttu-id="9b896-298">Per le destinazioni `Dictionary`, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="9b896-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="9b896-299">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="9b896-300">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-300">For example:</span></span>

* <span data-ttu-id="9b896-301">Si supponga che il parametro di destinazione sia un elemento `Dictionary<int, string>` denominato `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="9b896-302">I dati di modulo o di stringhe di query inviati possono essere simili a uno degli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-302">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="9b896-303">Per tutti i formati di esempio precedenti, l'associazione di modelli passa un dizionario di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="9b896-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="9b896-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="9b896-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="9b896-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="9b896-306">Comportamento di globalizzazione dell'associazione di modelli dati della route e stringhe di query</span><span class="sxs-lookup"><span data-stu-id="9b896-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="9b896-307">Il provider del valore della route ASP.NET Core e il provider del valore della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="9b896-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="9b896-308">Considera i valori come impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="9b896-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="9b896-309">Si prevede che gli URL siano impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="9b896-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="9b896-310">Al contrario, i valori provenienti dai dati del form subiscono una conversione con distinzione delle impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="9b896-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="9b896-311">Questa operazione è progettata in modo che gli URL siano condivisibili tra le impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="9b896-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="9b896-312">Per rendere il provider del valore di route ASP.NET Core e il provider di valori della stringa di query sottoposti a una conversione dipendente dalle impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="9b896-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="9b896-313">Ereditano da <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="9b896-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="9b896-314">Copiare il codice da [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="9b896-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="9b896-315">Sostituire il [valore delle impostazioni cultura](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passato al costruttore del provider di valori con [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="9b896-315">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="9b896-316">Sostituire la factory del provider di valori predefinito in opzioni MVC con la nuova:</span><span class="sxs-lookup"><span data-stu-id="9b896-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="9b896-317">Tipi di dati speciali</span><span class="sxs-lookup"><span data-stu-id="9b896-317">Special data types</span></span>

<span data-ttu-id="9b896-318">Esistono alcuni tipi di dati speciali che l'associazione di modelli può gestire.</span><span class="sxs-lookup"><span data-stu-id="9b896-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="9b896-319">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="9b896-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="9b896-320">Un file caricato incluso nella richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="9b896-321">È anche supportato `IEnumerable<IFormFile>` per più file.</span><span class="sxs-lookup"><span data-stu-id="9b896-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="9b896-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="9b896-322">CancellationToken</span></span>

<span data-ttu-id="9b896-323">usato per annullare l'attività nei controller asincroni.</span><span class="sxs-lookup"><span data-stu-id="9b896-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="9b896-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="9b896-324">FormCollection</span></span>

<span data-ttu-id="9b896-325">Usato per recuperare tutti i valori dai dati di modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="9b896-326">Formattatori di input</span><span class="sxs-lookup"><span data-stu-id="9b896-326">Input formatters</span></span>

<span data-ttu-id="9b896-327">I dati nel corpo della richiesta possono essere in formato JSON, XML o in altri formati.</span><span class="sxs-lookup"><span data-stu-id="9b896-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="9b896-328">Per analizzare questi dati, l'associazione di modelli usa un *formattatore di input* configurato in modo da gestire un particolare tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="9b896-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="9b896-329">Per impostazione predefinita, ASP.NET Core include i formattatori di input basati su JSON per la gestione dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="9b896-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="9b896-330">È possibile aggiungere altri formattatori per altri tipi di contenuto.</span><span class="sxs-lookup"><span data-stu-id="9b896-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="9b896-331">ASP.NET Core consente di selezionare i formattatori di input in base all'attributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="9b896-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="9b896-332">Se non è presente alcun attributo, viene usata l'[intestazione Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="9b896-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="9b896-333">Per usare i formattatori di input XML predefiniti:</span><span class="sxs-lookup"><span data-stu-id="9b896-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="9b896-334">Installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="9b896-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="9b896-335">In `Startup.ConfigureServices` chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="9b896-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="9b896-336">Applicare l'attributo `Consumes` alle classi controller o ai metodi di azione che devono aspettarsi XML nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="9b896-337">Per altre informazioni, vedere [Introduzione alla serializzazione XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="9b896-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="9b896-338">Escludere i tipi specificati dall'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9b896-338">Exclude specified types from model binding</span></span>

<span data-ttu-id="9b896-339">Il comportamento dell'associazione di modelli e del sistema di convalida è determinato da [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="9b896-339">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="9b896-340">È possibile personalizzare `ModelMetadata` mediante l'aggiunta di un provider di dettagli a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="9b896-340">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="9b896-341">Sono disponibili provider di dettagli predefiniti per la disabilitazione dell'associazione di modelli o la convalida per i tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="9b896-341">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="9b896-342">Per disabilitare l'associazione di modelli per tutti i modelli di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-342">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b896-343">Ad esempio, per disabilitare l'associazione di modelli per tutti i modelli di tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="9b896-343">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="9b896-344">Per disabilitare la convalida per le proprietà di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-344">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b896-345">Ad esempio, per disabilitare la convalida per le proprietà di tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="9b896-345">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="9b896-346">Strumenti di associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="9b896-346">Custom model binders</span></span>

<span data-ttu-id="9b896-347">È possibile estendere l'associazione di modelli scrivendo uno strumento di associazione di modelli personalizzato e usando l'attributo `[ModelBinder]` per selezionarlo per una determinata destinazione.</span><span class="sxs-lookup"><span data-stu-id="9b896-347">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="9b896-348">Altre informazioni sull'[associazione di modelli personalizzata](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="9b896-348">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="9b896-349">Associazione di modelli manuale</span><span class="sxs-lookup"><span data-stu-id="9b896-349">Manual model binding</span></span>

<span data-ttu-id="9b896-350">L'associazione di modelli può essere richiamata manualmente usando il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9b896-350">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="9b896-351">Il metodo è definito in entrambe le classi `ControllerBase` e `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="9b896-351">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="9b896-352">Gli overload del metodo consentono di specificare il prefisso e il provider di valori da usare.</span><span class="sxs-lookup"><span data-stu-id="9b896-352">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="9b896-353">Il metodo restituisce `false` se l'associazione di modelli non riesce.</span><span class="sxs-lookup"><span data-stu-id="9b896-353">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="9b896-354">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-354">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="9b896-355">Attributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="9b896-355">[FromServices] attribute</span></span>

<span data-ttu-id="9b896-356">Il nome di questo attributo segue il modello degli attributi di associazione di modelli che specificano un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-356">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="9b896-357">Non si tratta però dell'associazione di dati da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="9b896-357">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="9b896-358">Ottiene un'istanza di un tipo dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9b896-358">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9b896-359">Lo scopo è fornire un'alternativa all'inserimento del costruttore per quando è necessario un servizio solo se viene chiamato un metodo specifico.</span><span class="sxs-lookup"><span data-stu-id="9b896-359">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b896-360">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9b896-360">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9b896-361">Questo articolo illustra cos'è l'associazione di modelli, come funziona e come personalizzarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="9b896-361">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="9b896-362">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9b896-362">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="9b896-363">Che cos'è l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9b896-363">What is Model binding</span></span>

<span data-ttu-id="9b896-364">I controller e le pagine Razor operano sui dati provenienti dalle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-364">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="9b896-365">Ad esempio, i dati di route possono fornire una chiave del record e i campi modulo inviati possono fornire valori per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-365">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="9b896-366">La scrittura di codice per recuperare tutti i valori e convertirli da stringhe in tipi .NET sarebbe noiosa e soggetta a errori.</span><span class="sxs-lookup"><span data-stu-id="9b896-366">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="9b896-367">L'associazione di modelli consente di automatizzare questo processo.</span><span class="sxs-lookup"><span data-stu-id="9b896-367">Model binding automates this process.</span></span> <span data-ttu-id="9b896-368">Il sistema di associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="9b896-368">The model binding system:</span></span>

* <span data-ttu-id="9b896-369">Recupera i dati da diverse origini, ad esempio i dati di route, i campi modulo e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-369">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="9b896-370">Fornisce i dati ai controller e alle pagine Razor in parametri di metodo e proprietà pubbliche.</span><span class="sxs-lookup"><span data-stu-id="9b896-370">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="9b896-371">Converte dati stringa in tipi .NET.</span><span class="sxs-lookup"><span data-stu-id="9b896-371">Converts string data to .NET types.</span></span>
* <span data-ttu-id="9b896-372">Aggiorna le proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="9b896-372">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="9b896-373">Esempio</span><span class="sxs-lookup"><span data-stu-id="9b896-373">Example</span></span>

<span data-ttu-id="9b896-374">Si supponga di avere il metodo di azione seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-374">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="9b896-375">E l'app riceve una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="9b896-375">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="9b896-376">L'associazione di modelli esegue i passaggi seguenti dopo che il sistema di routing seleziona il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="9b896-376">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="9b896-377">Trova il primo parametro di `GetByID`, un intero denominato `id`.</span><span class="sxs-lookup"><span data-stu-id="9b896-377">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="9b896-378">Esamina le origini disponibili nella richiesta HTTP e trova `id` = "2" nei dati di route.</span><span class="sxs-lookup"><span data-stu-id="9b896-378">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="9b896-379">Converte la stringa "2" nell'intero 2.</span><span class="sxs-lookup"><span data-stu-id="9b896-379">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="9b896-380">Trova il parametro successivo di `GetByID`, un valore booleano denominato `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="9b896-380">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="9b896-381">Esamina le origini e trova "DogsOnly=true" nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-381">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="9b896-382">Per la corrispondenza dei nomi non viene applicata la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="9b896-382">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="9b896-383">Converte la stringa "true" nel valore booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="9b896-383">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="9b896-384">Il framework chiama quindi il metodo `GetById`, passando 2 per il parametro `id` e `true` per il parametro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="9b896-384">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="9b896-385">Nell'esempio precedente le destinazioni dell'associazione di modelli sono parametri di metodo che sono tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="9b896-385">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="9b896-386">Le destinazioni possono essere anche le proprietà di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="9b896-386">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="9b896-387">Dopo l'associazione di ogni proprietà, viene eseguita la [convalida dei modelli](xref:mvc/models/validation) per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-387">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="9b896-388">Il record dei dati associati al modello e di eventuali errori di associazione o convalida viene archiviato in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) oppure [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="9b896-388">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="9b896-389">Per scoprire se questo processo ha esito positivo, l'app controlla il flag [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="9b896-389">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="9b896-390">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="9b896-390">Targets</span></span>

<span data-ttu-id="9b896-391">L'associazione di modelli cerca di trovare i valori per i tipi di destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-391">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="9b896-392">Parametri del metodo di azione del controller a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-392">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="9b896-393">Parametri del metodo del gestore di Razor Pages a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-393">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="9b896-394">Proprietà pubbliche di un controller o una classe `PageModel`, se specificate dagli attributi.</span><span class="sxs-lookup"><span data-stu-id="9b896-394">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="9b896-395">Attributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="9b896-395">[BindProperty] attribute</span></span>

<span data-ttu-id="9b896-396">Può essere applicato a una proprietà pubblica di un controller o una classe `PageModel` per fare in modo che l'associazione di modelli usi tale proprietà come destinazione:</span><span class="sxs-lookup"><span data-stu-id="9b896-396">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="9b896-397">Attributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="9b896-397">[BindProperties] attribute</span></span>

<span data-ttu-id="9b896-398">Disponibile in ASP.NET Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="9b896-398">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="9b896-399">Può essere applicato a un controller o una classe `PageModel` per indicare all'associazione del modello di usare tutte le proprietà pubbliche della classe come destinazione:</span><span class="sxs-lookup"><span data-stu-id="9b896-399">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="9b896-400">Associazione di modelli per le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="9b896-400">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="9b896-401">Per impostazione predefinita, le proprietà non vengono associate per le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="9b896-401">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="9b896-402">In genere, per una richiesta GET è sufficiente un parametro ID record.</span><span class="sxs-lookup"><span data-stu-id="9b896-402">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="9b896-403">L'ID record viene usato per cercare l'elemento nel database.</span><span class="sxs-lookup"><span data-stu-id="9b896-403">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="9b896-404">Pertanto, non è necessario associare una proprietà che contiene un'istanza del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-404">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="9b896-405">Negli scenari in cui si vogliono associare le proprietà ai dati dalle richieste GET, impostare la proprietà `SupportsGet` su `true`:</span><span class="sxs-lookup"><span data-stu-id="9b896-405">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="9b896-406">Origini</span><span class="sxs-lookup"><span data-stu-id="9b896-406">Sources</span></span>

<span data-ttu-id="9b896-407">Per impostazione predefinita, l'associazione di modelli ottiene i dati sotto forma di coppie chiave-valore dalle origini seguenti in una richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="9b896-407">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="9b896-408">Campi modulo</span><span class="sxs-lookup"><span data-stu-id="9b896-408">Form fields</span></span>
1. <span data-ttu-id="9b896-409">Il corpo della richiesta (per i controller [ con l'attributo [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="9b896-409">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="9b896-410">Dati della route</span><span class="sxs-lookup"><span data-stu-id="9b896-410">Route data</span></span>
1. <span data-ttu-id="9b896-411">Parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="9b896-411">Query string parameters</span></span>
1. <span data-ttu-id="9b896-412">File caricati</span><span class="sxs-lookup"><span data-stu-id="9b896-412">Uploaded files</span></span>

<span data-ttu-id="9b896-413">Per ogni parametro o proprietà di destinazione, le origini vengono analizzate nell'ordine indicato nell'elenco precedente.</span><span class="sxs-lookup"><span data-stu-id="9b896-413">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="9b896-414">Esistono tuttavia alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="9b896-414">There are a few exceptions:</span></span>

* <span data-ttu-id="9b896-415">I dati di route e i valori delle stringhe di query vengono usati solo per i tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="9b896-415">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="9b896-416">I file caricati vengono associati solo ai tipi di destinazione che implementano `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="9b896-416">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="9b896-417">Se l'origine predefinita non è corretta, usare uno degli attributi seguenti per specificare l'origine:</span><span class="sxs-lookup"><span data-stu-id="9b896-417">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="9b896-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -ottiene i valori dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="9b896-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -ottiene i valori dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="9b896-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="9b896-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -ottiene i valori dai campi del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="9b896-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -ottiene i valori dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="9b896-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -ottiene i valori dalle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="9b896-423">Questi attributi:</span><span class="sxs-lookup"><span data-stu-id="9b896-423">These attributes:</span></span>

* <span data-ttu-id="9b896-424">Vengono aggiunti singolarmente alle proprietà del modello (non alla classe del modello), come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-424">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="9b896-425">Accettano facoltativamente un valore di nome di modello nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="9b896-425">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="9b896-426">Questa opzione è disponibile nel caso in cui il nome della proprietà non corrisponda al valore nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-426">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="9b896-427">Il valore nella richiesta, ad esempio, potrebbe essere un'intestazione con un segno meno nel nome, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-427">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="9b896-428">Attributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="9b896-428">[FromBody] attribute</span></span>

<span data-ttu-id="9b896-429">Applicare l'attributo `[FromBody]` a un parametro per popolarne le proprietà dal corpo di una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-429">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="9b896-430">Il runtime di ASP.NET Core delega la responsabilità della lettura del corpo a un formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="9b896-430">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="9b896-431">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-431">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="9b896-432">Quando `[FromBody]` viene applicato a un parametro di tipo complesso, tutti gli attributi di origine di associazione applicati alle relative proprietà vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="9b896-432">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="9b896-433">Ad esempio, il `Create` azione seguente specifica che il relativo parametro `pet` viene popolato dal corpo:</span><span class="sxs-lookup"><span data-stu-id="9b896-433">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="9b896-434">La classe `Pet` specifica che la relativa proprietà `Breed` viene popolata da un parametro della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="9b896-434">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="9b896-435">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="9b896-435">In the preceding example:</span></span>

* <span data-ttu-id="9b896-436">L'attributo `[FromQuery]` viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="9b896-436">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="9b896-437">La proprietà `Breed` non viene popolata da un parametro della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9b896-437">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="9b896-438">I formattatori di input leggono solo il corpo e non comprendono gli attributi di origine del binding.</span><span class="sxs-lookup"><span data-stu-id="9b896-438">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="9b896-439">Se nel corpo viene trovato un valore appropriato, tale valore viene usato per popolare la proprietà `Breed`.</span><span class="sxs-lookup"><span data-stu-id="9b896-439">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="9b896-440">Non applicare `[FromBody]` a più di un parametro per ogni metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="9b896-440">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="9b896-441">Quando il flusso di richieste viene letto da un formattatore di input, non è più disponibile per la lettura per l'associazione di altri parametri `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="9b896-441">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="9b896-442">Origini aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9b896-442">Additional sources</span></span>

<span data-ttu-id="9b896-443">I dati di origine vengono forniti al sistema di associazione di modelli dai *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="9b896-443">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="9b896-444">È possibile scrivere e registrare i provider di valori personalizzati che recuperano i dati per l'associazione di modelli da altre origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-444">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="9b896-445">Ad esempio, potrebbe essere necessario recuperare dati dai cookie o dallo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="9b896-445">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="9b896-446">Per ottenere dati da una nuova origine:</span><span class="sxs-lookup"><span data-stu-id="9b896-446">To get data from a new source:</span></span>

* <span data-ttu-id="9b896-447">Creare una classe che implementi `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="9b896-447">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="9b896-448">Creare una classe che implementi `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="9b896-448">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="9b896-449">Registrare la classe factory in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-449">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="9b896-450">L'app di esempio include un esempio di [provider di valori](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) e [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) che ottiene i valori dai cookie.</span><span class="sxs-lookup"><span data-stu-id="9b896-450">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="9b896-451">Ecco il codice di registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9b896-451">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="9b896-452">Il codice illustrato posiziona il provider di valori personalizzati dopo tutti i provider di valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="9b896-452">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="9b896-453">Per renderlo il primo nell'elenco, chiamare `Insert(0, new CookieValueProviderFactory())` invece di `Add`.</span><span class="sxs-lookup"><span data-stu-id="9b896-453">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="9b896-454">Nessuna origine per una proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="9b896-454">No source for a model property</span></span>

<span data-ttu-id="9b896-455">Per impostazione predefinita, non viene creato un errore di stato del modello se non viene trovato alcun valore per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-455">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="9b896-456">La proprietà viene impostata su Null o su un valore predefinito:</span><span class="sxs-lookup"><span data-stu-id="9b896-456">The property is set to null or a default value:</span></span>

* <span data-ttu-id="9b896-457">I tipi semplici nullable vengono impostati su `null`.</span><span class="sxs-lookup"><span data-stu-id="9b896-457">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="9b896-458">I tipi valore non nullable vengono impostati su `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="9b896-458">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="9b896-459">Ad esempio, un parametro `int id` viene impostato su 0.</span><span class="sxs-lookup"><span data-stu-id="9b896-459">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="9b896-460">Per i tipi complessi, l'associazione di modelli crea un'istanza usando il costruttore predefinito, senza impostare proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-460">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="9b896-461">Le matrici vengono impostate su `Array.Empty<T>()`, ad eccezione delle matrici `byte[]` che vengono impostate su `null`.</span><span class="sxs-lookup"><span data-stu-id="9b896-461">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="9b896-462">Se lo stato del modello deve essere invalidato quando non viene trovato alcun elemento nei campi del modulo per una proprietà del modello, usare l'attributo [`[BindRequired]`](#bindrequired-attribute) .</span><span class="sxs-lookup"><span data-stu-id="9b896-462">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="9b896-463">Si noti che questo comportamento `[BindRequired]` si applica all'associazione di modelli dai dati di moduli inviati e non ai dati JSON o XML nel corpo di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-463">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="9b896-464">I dati del corpo della richiesta vengono gestiti dai [formattatori di input](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-464">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="9b896-465">Errori di conversione dei tipi</span><span class="sxs-lookup"><span data-stu-id="9b896-465">Type conversion errors</span></span>

<span data-ttu-id="9b896-466">Se viene trovata un'origine ma non può essere convertita nel tipo di destinazione, lo stato del modello viene contrassegnato come non valido.</span><span class="sxs-lookup"><span data-stu-id="9b896-466">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="9b896-467">Il parametro o la proprietà di destinazione viene impostato su Null o su un valore predefinito, come indicato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="9b896-467">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="9b896-468">In un controller API con l'attributo `[ApiController]`, uno stato del modello non valido genera una risposta HTTP 400 automatica.</span><span class="sxs-lookup"><span data-stu-id="9b896-468">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="9b896-469">In una pagina Razor visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="9b896-469">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="9b896-470">La convalida sul lato client consente di rilevare la maggior parte dei dati non validi che, in caso contrario, verrebbero inviati a un modulo di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9b896-470">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="9b896-471">Questa convalida rende difficile attivare il codice sopra evidenziato.</span><span class="sxs-lookup"><span data-stu-id="9b896-471">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="9b896-472">L'app di esempio include un pulsante **Submit with Invalid Date** (Invia con data non valida) che inserisce dati non validi nel campo **Hire Date** (Data assunzione) e invia il modulo.</span><span class="sxs-lookup"><span data-stu-id="9b896-472">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="9b896-473">Questo pulsante illustra il funzionamento del codice per visualizzare di nuovo la pagina quando si verificano errori di conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-473">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="9b896-474">Quando la pagina viene nuovamente visualizzata dal codice precedente, l'input non valido non viene visualizzato nel campo modulo.</span><span class="sxs-lookup"><span data-stu-id="9b896-474">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="9b896-475">Questo avviene perché la proprietà del modello è stata impostata su Null o su un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="9b896-475">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="9b896-476">L'input non valido viene visualizzato in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="9b896-476">The invalid input does appear in an error message.</span></span> <span data-ttu-id="9b896-477">Ma se si vogliono visualizzare di nuovo i dati non validi nel campo modulo, è consigliabile impostare la proprietà del modello come stringa ed eseguire manualmente la conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-477">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="9b896-478">La stessa strategia è consigliata se si vuole evitare che gli errori di conversione del tipo causino errori di stato del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-478">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="9b896-479">In tal caso, impostare la proprietà del modello come stringa.</span><span class="sxs-lookup"><span data-stu-id="9b896-479">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="9b896-480">Tipi semplici</span><span class="sxs-lookup"><span data-stu-id="9b896-480">Simple types</span></span>

<span data-ttu-id="9b896-481">I tipi semplici in cui lo strumento di associazione di modelli può convertire le stringhe di origine includono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-481">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="9b896-482">Boolean</span><span class="sxs-lookup"><span data-stu-id="9b896-482">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="9b896-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="9b896-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="9b896-484">Char</span><span class="sxs-lookup"><span data-stu-id="9b896-484">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="9b896-485">DateTime</span><span class="sxs-lookup"><span data-stu-id="9b896-485">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="9b896-486">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9b896-486">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="9b896-487">Decimal</span><span class="sxs-lookup"><span data-stu-id="9b896-487">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="9b896-488">Double</span><span class="sxs-lookup"><span data-stu-id="9b896-488">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="9b896-489">Enum</span><span class="sxs-lookup"><span data-stu-id="9b896-489">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="9b896-490">Guid</span><span class="sxs-lookup"><span data-stu-id="9b896-490">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="9b896-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="9b896-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="9b896-492">Single</span><span class="sxs-lookup"><span data-stu-id="9b896-492">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="9b896-493">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9b896-493">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="9b896-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="9b896-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="9b896-495">Uri</span><span class="sxs-lookup"><span data-stu-id="9b896-495">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="9b896-496">Version</span><span class="sxs-lookup"><span data-stu-id="9b896-496">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="9b896-497">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="9b896-497">Complex types</span></span>

<span data-ttu-id="9b896-498">Un tipo pubblico deve avere un costruttore predefinito pubblico e proprietà scrivibili pubbliche da associare.</span><span class="sxs-lookup"><span data-stu-id="9b896-498">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="9b896-499">Quando si verifica l'associazione di modelli, vengono create istanze della classe usando il costruttore predefinito pubblico.</span><span class="sxs-lookup"><span data-stu-id="9b896-499">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="9b896-500">Per ogni proprietà del tipo complesso, l'associazione di modelli cerca il modello di nome *prefisso.nome_proprietà* nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-500">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="9b896-501">Se non viene trovato, cerca semplicemente *nome_proprietà* senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-501">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="9b896-502">Per l'associazione a un parametro, il prefisso è il nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="9b896-502">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="9b896-503">Per l'associazione a una proprietà pubblica `PageModel`, il prefisso è il nome della proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="9b896-503">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="9b896-504">Alcuni attributi hanno una proprietà `Prefix` che consente di eseguire l'override dell'utilizzo predefinito del nome di un parametro o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="9b896-504">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="9b896-505">Ad esempio, si supponga che il tipo complesso sia la classe `Instructor` seguente:</span><span class="sxs-lookup"><span data-stu-id="9b896-505">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="9b896-506">Prefisso = nome del parametro</span><span class="sxs-lookup"><span data-stu-id="9b896-506">Prefix = parameter name</span></span>

<span data-ttu-id="9b896-507">Se il modello da associare è un parametro denominato `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="9b896-507">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="9b896-508">L'associazione di modelli cerca prima di tutto la chiave `instructorToUpdate.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-508">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="9b896-509">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-509">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="9b896-510">Prefisso = nome della proprietà</span><span class="sxs-lookup"><span data-stu-id="9b896-510">Prefix = property name</span></span>

<span data-ttu-id="9b896-511">Se il modello da associare è una proprietà denominata `Instructor` del controller o della classe `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="9b896-511">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="9b896-512">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-512">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="9b896-513">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-513">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="9b896-514">Prefisso personalizzato</span><span class="sxs-lookup"><span data-stu-id="9b896-514">Custom prefix</span></span>

<span data-ttu-id="9b896-515">Se il modello da associare è un parametro denominato `instructorToUpdate` e un attributo `Bind` specifica `Instructor` come prefisso:</span><span class="sxs-lookup"><span data-stu-id="9b896-515">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="9b896-516">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="9b896-516">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="9b896-517">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-517">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="9b896-518">Attributi per le destinazioni di tipo complesso</span><span class="sxs-lookup"><span data-stu-id="9b896-518">Attributes for complex type targets</span></span>

<span data-ttu-id="9b896-519">Sono disponibili vari attributi predefiniti per controllare l'associazione di modelli di tipi complessi:</span><span class="sxs-lookup"><span data-stu-id="9b896-519">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="9b896-520">Questi attributi influiscono sul modello di associazione quando i dati di modulo inviati sono l'origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="9b896-520">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="9b896-521">Non influiscono sui formattatori di input, che elaborano corpi di richieste JSON e XML inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-521">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="9b896-522">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="9b896-522">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="9b896-523">Vedere anche la discussione relativa all'attributo `[Required]` in [Convalida del modello](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="9b896-523">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="9b896-524">Attributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="9b896-524">[BindRequired] attribute</span></span>

<span data-ttu-id="9b896-525">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-525">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="9b896-526">Con questo attributo l'associazione di modelli aggiunge un errore di stato del modello se non è possibile eseguire l'associazione per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-526">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="9b896-527">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-527">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="9b896-528">Attributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="9b896-528">[BindNever] attribute</span></span>

<span data-ttu-id="9b896-529">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-529">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="9b896-530">Impedisce all'associazione di modelli di impostare una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9b896-530">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="9b896-531">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-531">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="9b896-532">Attributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="9b896-532">[Bind] attribute</span></span>

<span data-ttu-id="9b896-533">Può essere applicato a una classe o a un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="9b896-533">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="9b896-534">Specifica quali proprietà di un modello devono essere incluse nell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="9b896-534">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="9b896-535">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato qualsiasi gestore o metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="9b896-535">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="9b896-536">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato il metodo `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="9b896-536">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="9b896-537">L'attributo `[Bind]` può essere usato per evitare l'overposting negli scenari di *creazione*.</span><span class="sxs-lookup"><span data-stu-id="9b896-537">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="9b896-538">Non funziona bene negli scenari di modifica perché le proprietà escluse vengono impostate su Null o su un valore predefinito anziché rimanere inalterate.</span><span class="sxs-lookup"><span data-stu-id="9b896-538">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="9b896-539">Per difendersi dall'overposting, sono consigliati i modelli di visualizzazione anziché l'attributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="9b896-539">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="9b896-540">Per altre informazioni, vedere [Nota sulla sicurezza relativa all'overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="9b896-540">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="9b896-541">Raccolte</span><span class="sxs-lookup"><span data-stu-id="9b896-541">Collections</span></span>

<span data-ttu-id="9b896-542">Per le destinazioni che sono raccolte di tipi semplici, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="9b896-542">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="9b896-543">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-543">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="9b896-544">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-544">For example:</span></span>

* <span data-ttu-id="9b896-545">Si supponga che il parametro da associare sia una matrice denominata `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-545">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="9b896-546">I dati di modulo o di stringhe di query possono essere in uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-546">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="9b896-547">Il formato seguente è supportato solo nei dati di modulo:</span><span class="sxs-lookup"><span data-stu-id="9b896-547">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="9b896-548">Per tutti i formati di esempio precedenti, l'associazione di modelli passa una matrice di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-548">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="9b896-549">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="9b896-549">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="9b896-550">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="9b896-550">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="9b896-551">Per i formati di dati che usano numeri con indice (... [0]... [1]...) è necessario assicurarsi che la numerazione sia progressiva a partire da zero.</span><span class="sxs-lookup"><span data-stu-id="9b896-551">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="9b896-552">Se sono presenti eventuali interruzioni nella numerazione con indice, tutti gli elementi dopo l'interruzione vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="9b896-552">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="9b896-553">Ad esempio, se gli indici sono 0 e 2 anziché 0 e 1, il secondo elemento viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="9b896-553">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="9b896-554">Dizionari</span><span class="sxs-lookup"><span data-stu-id="9b896-554">Dictionaries</span></span>

<span data-ttu-id="9b896-555">Per le destinazioni `Dictionary`, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="9b896-555">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="9b896-556">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="9b896-556">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="9b896-557">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-557">For example:</span></span>

* <span data-ttu-id="9b896-558">Si supponga che il parametro di destinazione sia un elemento `Dictionary<int, string>` denominato `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-558">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="9b896-559">I dati di modulo o di stringhe di query inviati possono essere simili a uno degli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b896-559">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="9b896-560">Per tutti i formati di esempio precedenti, l'associazione di modelli passa un dizionario di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="9b896-560">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="9b896-561">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="9b896-561">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="9b896-562">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="9b896-562">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="9b896-563">Comportamento di globalizzazione dell'associazione di modelli dati della route e stringhe di query</span><span class="sxs-lookup"><span data-stu-id="9b896-563">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="9b896-564">Il provider del valore della route ASP.NET Core e il provider del valore della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="9b896-564">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="9b896-565">Considera i valori come impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="9b896-565">Treat values as invariant culture.</span></span>
* <span data-ttu-id="9b896-566">Si prevede che gli URL siano impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="9b896-566">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="9b896-567">Al contrario, i valori provenienti dai dati del form subiscono una conversione con distinzione delle impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="9b896-567">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="9b896-568">Questa operazione è progettata in modo che gli URL siano condivisibili tra le impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="9b896-568">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="9b896-569">Per rendere il provider del valore di route ASP.NET Core e il provider di valori della stringa di query sottoposti a una conversione dipendente dalle impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="9b896-569">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="9b896-570">Ereditano da <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="9b896-570">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="9b896-571">Copiare il codice da [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="9b896-571">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="9b896-572">Sostituire il [valore delle impostazioni cultura](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passato al costruttore del provider di valori con [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="9b896-572">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="9b896-573">Sostituire la factory del provider di valori predefinito in opzioni MVC con la nuova:</span><span class="sxs-lookup"><span data-stu-id="9b896-573">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="9b896-574">Tipi di dati speciali</span><span class="sxs-lookup"><span data-stu-id="9b896-574">Special data types</span></span>

<span data-ttu-id="9b896-575">Esistono alcuni tipi di dati speciali che l'associazione di modelli può gestire.</span><span class="sxs-lookup"><span data-stu-id="9b896-575">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="9b896-576">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="9b896-576">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="9b896-577">Un file caricato incluso nella richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b896-577">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="9b896-578">È anche supportato `IEnumerable<IFormFile>` per più file.</span><span class="sxs-lookup"><span data-stu-id="9b896-578">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="9b896-579">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="9b896-579">CancellationToken</span></span>

<span data-ttu-id="9b896-580">usato per annullare l'attività nei controller asincroni.</span><span class="sxs-lookup"><span data-stu-id="9b896-580">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="9b896-581">FormCollection</span><span class="sxs-lookup"><span data-stu-id="9b896-581">FormCollection</span></span>

<span data-ttu-id="9b896-582">Usato per recuperare tutti i valori dai dati di modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="9b896-582">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="9b896-583">Formattatori di input</span><span class="sxs-lookup"><span data-stu-id="9b896-583">Input formatters</span></span>

<span data-ttu-id="9b896-584">I dati nel corpo della richiesta possono essere in formato JSON, XML o in altri formati.</span><span class="sxs-lookup"><span data-stu-id="9b896-584">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="9b896-585">Per analizzare questi dati, l'associazione di modelli usa un *formattatore di input* configurato in modo da gestire un particolare tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="9b896-585">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="9b896-586">Per impostazione predefinita, ASP.NET Core include i formattatori di input basati su JSON per la gestione dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="9b896-586">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="9b896-587">È possibile aggiungere altri formattatori per altri tipi di contenuto.</span><span class="sxs-lookup"><span data-stu-id="9b896-587">You can add other formatters for other content types.</span></span>

<span data-ttu-id="9b896-588">ASP.NET Core consente di selezionare i formattatori di input in base all'attributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="9b896-588">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="9b896-589">Se non è presente alcun attributo, viene usata l'[intestazione Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="9b896-589">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="9b896-590">Per usare i formattatori di input XML predefiniti:</span><span class="sxs-lookup"><span data-stu-id="9b896-590">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="9b896-591">Installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="9b896-591">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="9b896-592">In `Startup.ConfigureServices` chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="9b896-592">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="9b896-593">Applicare l'attributo `Consumes` alle classi controller o ai metodi di azione che devono aspettarsi XML nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b896-593">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="9b896-594">Per altre informazioni, vedere [Introduzione alla serializzazione XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="9b896-594">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="9b896-595">Escludere i tipi specificati dall'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9b896-595">Exclude specified types from model binding</span></span>

<span data-ttu-id="9b896-596">Il comportamento dell'associazione di modelli e del sistema di convalida è determinato da [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="9b896-596">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="9b896-597">È possibile personalizzare `ModelMetadata` mediante l'aggiunta di un provider di dettagli a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="9b896-597">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="9b896-598">Sono disponibili provider di dettagli predefiniti per la disabilitazione dell'associazione di modelli o la convalida per i tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="9b896-598">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="9b896-599">Per disabilitare l'associazione di modelli per tutti i modelli di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-599">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b896-600">Ad esempio, per disabilitare l'associazione di modelli per tutti i modelli di tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="9b896-600">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="9b896-601">Per disabilitare la convalida per le proprietà di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b896-601">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b896-602">Ad esempio, per disabilitare la convalida per le proprietà di tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="9b896-602">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="9b896-603">Strumenti di associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="9b896-603">Custom model binders</span></span>

<span data-ttu-id="9b896-604">È possibile estendere l'associazione di modelli scrivendo uno strumento di associazione di modelli personalizzato e usando l'attributo `[ModelBinder]` per selezionarlo per una determinata destinazione.</span><span class="sxs-lookup"><span data-stu-id="9b896-604">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="9b896-605">Altre informazioni sull'[associazione di modelli personalizzata](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="9b896-605">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="9b896-606">Associazione di modelli manuale</span><span class="sxs-lookup"><span data-stu-id="9b896-606">Manual model binding</span></span>

<span data-ttu-id="9b896-607">L'associazione di modelli può essere richiamata manualmente usando il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9b896-607">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="9b896-608">Il metodo è definito in entrambe le classi `ControllerBase` e `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="9b896-608">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="9b896-609">Gli overload del metodo consentono di specificare il prefisso e il provider di valori da usare.</span><span class="sxs-lookup"><span data-stu-id="9b896-609">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="9b896-610">Il metodo restituisce `false` se l'associazione di modelli non riesce.</span><span class="sxs-lookup"><span data-stu-id="9b896-610">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="9b896-611">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="9b896-611">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="9b896-612">Attributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="9b896-612">[FromServices] attribute</span></span>

<span data-ttu-id="9b896-613">Il nome di questo attributo segue il modello degli attributi di associazione di modelli che specificano un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="9b896-613">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="9b896-614">Non si tratta però dell'associazione di dati da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="9b896-614">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="9b896-615">Ottiene un'istanza di un tipo dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9b896-615">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9b896-616">Lo scopo è fornire un'alternativa all'inserimento del costruttore per quando è necessario un servizio solo se viene chiamato un metodo specifico.</span><span class="sxs-lookup"><span data-stu-id="9b896-616">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b896-617">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9b896-617">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
