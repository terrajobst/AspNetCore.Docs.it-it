---
title: Associazione di modelli in ASP.NET Core
author: rick-anderson
description: Informazioni su come funziona l'associazione di modelli in ASP.NET Core e su come personalizzarne il comportamento.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 12/18/2019
uid: mvc/models/model-binding
ms.openlocfilehash: d36e42ef2517068ade3f874dc62cc7587ee3ca98
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355666"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="53f65-103">Associazione di modelli in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53f65-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="53f65-104">Questo articolo illustra cos'è l'associazione di modelli, come funziona e come personalizzarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="53f65-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="53f65-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="53f65-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="53f65-106">Che cos'è l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="53f65-106">What is Model binding</span></span>

<span data-ttu-id="53f65-107">I controller e le pagine Razor operano sui dati provenienti dalle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="53f65-108">Ad esempio, i dati di route possono fornire una chiave del record e i campi modulo inviati possono fornire valori per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="53f65-109">La scrittura di codice per recuperare tutti i valori e convertirli da stringhe in tipi .NET sarebbe noiosa e soggetta a errori.</span><span class="sxs-lookup"><span data-stu-id="53f65-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="53f65-110">L'associazione di modelli consente di automatizzare questo processo.</span><span class="sxs-lookup"><span data-stu-id="53f65-110">Model binding automates this process.</span></span> <span data-ttu-id="53f65-111">Il sistema di associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="53f65-111">The model binding system:</span></span>

* <span data-ttu-id="53f65-112">Recupera i dati da diverse origini, ad esempio i dati di route, i campi modulo e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="53f65-113">Fornisce i dati ai controller e alle pagine Razor in parametri di metodo e proprietà pubbliche.</span><span class="sxs-lookup"><span data-stu-id="53f65-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="53f65-114">Converte dati stringa in tipi .NET.</span><span class="sxs-lookup"><span data-stu-id="53f65-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="53f65-115">Aggiorna le proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="53f65-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="53f65-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="53f65-116">Example</span></span>

<span data-ttu-id="53f65-117">Si supponga di avere il metodo di azione seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="53f65-118">E l'app riceve una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="53f65-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="53f65-119">L'associazione di modelli esegue i passaggi seguenti dopo che il sistema di routing seleziona il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="53f65-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="53f65-120">Trova il primo parametro di `GetByID`, un intero denominato `id`.</span><span class="sxs-lookup"><span data-stu-id="53f65-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="53f65-121">Esamina le origini disponibili nella richiesta HTTP e trova `id` = "2" nei dati di route.</span><span class="sxs-lookup"><span data-stu-id="53f65-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="53f65-122">Converte la stringa "2" nell'intero 2.</span><span class="sxs-lookup"><span data-stu-id="53f65-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="53f65-123">Trova il parametro successivo di `GetByID`, un valore booleano denominato `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="53f65-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="53f65-124">Esamina le origini e trova "DogsOnly=true" nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="53f65-125">Per la corrispondenza dei nomi non viene applicata la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="53f65-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="53f65-126">Converte la stringa "true" nel valore booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="53f65-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="53f65-127">Il framework chiama quindi il metodo `GetById`, passando 2 per il parametro `id` e `true` per il parametro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="53f65-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="53f65-128">Nell'esempio precedente le destinazioni dell'associazione di modelli sono parametri di metodo che sono tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="53f65-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="53f65-129">Le destinazioni possono essere anche le proprietà di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="53f65-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="53f65-130">Dopo l'associazione di ogni proprietà, viene eseguita la [convalida dei modelli](xref:mvc/models/validation) per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="53f65-131">Il record dei dati associati al modello e di eventuali errori di associazione o convalida viene archiviato in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) oppure [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="53f65-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="53f65-132">Per scoprire se questo processo ha esito positivo, l'app controlla il flag [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="53f65-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="53f65-133">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="53f65-133">Targets</span></span>

<span data-ttu-id="53f65-134">L'associazione di modelli cerca di trovare i valori per i tipi di destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="53f65-135">Parametri del metodo di azione del controller a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="53f65-136">Parametri del metodo del gestore di Razor Pages a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="53f65-137">Proprietà pubbliche di un controller o una classe `PageModel`, se specificate dagli attributi.</span><span class="sxs-lookup"><span data-stu-id="53f65-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="53f65-138">Attributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="53f65-138">[BindProperty] attribute</span></span>

<span data-ttu-id="53f65-139">Può essere applicato a una proprietà pubblica di un controller o una classe `PageModel` per fare in modo che l'associazione di modelli usi tale proprietà come destinazione:</span><span class="sxs-lookup"><span data-stu-id="53f65-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="53f65-140">Attributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="53f65-140">[BindProperties] attribute</span></span>

<span data-ttu-id="53f65-141">Disponibile in ASP.NET Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="53f65-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="53f65-142">Può essere applicato a un controller o una classe `PageModel` per indicare all'associazione del modello di usare tutte le proprietà pubbliche della classe come destinazione:</span><span class="sxs-lookup"><span data-stu-id="53f65-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="53f65-143">Associazione di modelli per le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="53f65-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="53f65-144">Per impostazione predefinita, le proprietà non vengono associate per le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="53f65-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="53f65-145">In genere, per una richiesta GET è sufficiente un parametro ID record.</span><span class="sxs-lookup"><span data-stu-id="53f65-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="53f65-146">L'ID record viene usato per cercare l'elemento nel database.</span><span class="sxs-lookup"><span data-stu-id="53f65-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="53f65-147">Pertanto, non è necessario associare una proprietà che contiene un'istanza del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="53f65-148">Negli scenari in cui si vogliono associare le proprietà ai dati dalle richieste GET, impostare la proprietà `SupportsGet` su `true`:</span><span class="sxs-lookup"><span data-stu-id="53f65-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="53f65-149">Origini</span><span class="sxs-lookup"><span data-stu-id="53f65-149">Sources</span></span>

<span data-ttu-id="53f65-150">Per impostazione predefinita, l'associazione di modelli ottiene i dati sotto forma di coppie chiave-valore dalle origini seguenti in una richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="53f65-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="53f65-151">Campi modulo</span><span class="sxs-lookup"><span data-stu-id="53f65-151">Form fields</span></span>
1. <span data-ttu-id="53f65-152">Il corpo della richiesta (per i controller [ con l'attributo [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="53f65-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="53f65-153">Dati della route</span><span class="sxs-lookup"><span data-stu-id="53f65-153">Route data</span></span>
1. <span data-ttu-id="53f65-154">Parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="53f65-154">Query string parameters</span></span>
1. <span data-ttu-id="53f65-155">File caricati</span><span class="sxs-lookup"><span data-stu-id="53f65-155">Uploaded files</span></span>

<span data-ttu-id="53f65-156">Per ogni parametro o proprietà di destinazione, le origini vengono analizzate nell'ordine indicato nell'elenco precedente.</span><span class="sxs-lookup"><span data-stu-id="53f65-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="53f65-157">Esistono tuttavia alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="53f65-157">There are a few exceptions:</span></span>

* <span data-ttu-id="53f65-158">I dati di route e i valori delle stringhe di query vengono usati solo per i tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="53f65-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="53f65-159">I file caricati vengono associati solo ai tipi di destinazione che implementano `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="53f65-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="53f65-160">Se l'origine predefinita non è corretta, usare uno degli attributi seguenti per specificare l'origine:</span><span class="sxs-lookup"><span data-stu-id="53f65-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="53f65-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -ottiene i valori dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="53f65-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -ottiene i valori dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="53f65-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="53f65-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -ottiene i valori dai campi del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="53f65-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -ottiene i valori dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="53f65-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -ottiene i valori dalle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="53f65-166">Questi attributi:</span><span class="sxs-lookup"><span data-stu-id="53f65-166">These attributes:</span></span>

* <span data-ttu-id="53f65-167">Vengono aggiunti singolarmente alle proprietà del modello (non alla classe del modello), come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="53f65-168">Accettano facoltativamente un valore di nome di modello nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="53f65-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="53f65-169">Questa opzione è disponibile nel caso in cui il nome della proprietà non corrisponda al valore nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="53f65-170">Il valore nella richiesta, ad esempio, potrebbe essere un'intestazione con un segno meno nel nome, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="53f65-171">Attributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="53f65-171">[FromBody] attribute</span></span>

<span data-ttu-id="53f65-172">Applicare l'attributo `[FromBody]` a un parametro per popolarne le proprietà dal corpo di una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="53f65-173">Il runtime di ASP.NET Core delega la responsabilità della lettura del corpo a un formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="53f65-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="53f65-174">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="53f65-175">Quando `[FromBody]` viene applicato a un parametro di tipo complesso, tutti gli attributi di origine di associazione applicati alle relative proprietà vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="53f65-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="53f65-176">Ad esempio, il `Create` azione seguente specifica che il relativo parametro `pet` viene popolato dal corpo:</span><span class="sxs-lookup"><span data-stu-id="53f65-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="53f65-177">La classe `Pet` specifica che la relativa proprietà `Breed` viene popolata da un parametro della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="53f65-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="53f65-178">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="53f65-178">In the preceding example:</span></span>

* <span data-ttu-id="53f65-179">L'attributo `[FromQuery]` viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="53f65-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="53f65-180">La proprietà `Breed` non viene popolata da un parametro della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="53f65-181">I formattatori di input leggono solo il corpo e non comprendono gli attributi di origine del binding.</span><span class="sxs-lookup"><span data-stu-id="53f65-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="53f65-182">Se nel corpo viene trovato un valore appropriato, tale valore viene usato per popolare la proprietà `Breed`.</span><span class="sxs-lookup"><span data-stu-id="53f65-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="53f65-183">Non applicare `[FromBody]` a più di un parametro per ogni metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="53f65-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="53f65-184">Quando il flusso di richieste viene letto da un formattatore di input, non è più disponibile per la lettura per l'associazione di altri parametri `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="53f65-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="53f65-185">Origini aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53f65-185">Additional sources</span></span>

<span data-ttu-id="53f65-186">I dati di origine vengono forniti al sistema di associazione di modelli dai *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="53f65-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="53f65-187">È possibile scrivere e registrare i provider di valori personalizzati che recuperano i dati per l'associazione di modelli da altre origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="53f65-188">Ad esempio, potrebbe essere necessario recuperare dati dai cookie o dallo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="53f65-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="53f65-189">Per ottenere dati da una nuova origine:</span><span class="sxs-lookup"><span data-stu-id="53f65-189">To get data from a new source:</span></span>

* <span data-ttu-id="53f65-190">Creare una classe che implementi `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="53f65-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="53f65-191">Creare una classe che implementi `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="53f65-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="53f65-192">Registrare la classe factory in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="53f65-193">L'app di esempio include un esempio di [provider di valori](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) e [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) che ottiene i valori dai cookie.</span><span class="sxs-lookup"><span data-stu-id="53f65-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="53f65-194">Ecco il codice di registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53f65-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4)]

<span data-ttu-id="53f65-195">Il codice illustrato posiziona il provider di valori personalizzati dopo tutti i provider di valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="53f65-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="53f65-196">Per renderlo il primo nell'elenco, chiamare `Insert(0, new CookieValueProviderFactory())` invece di `Add`.</span><span class="sxs-lookup"><span data-stu-id="53f65-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="53f65-197">Nessuna origine per una proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="53f65-197">No source for a model property</span></span>

<span data-ttu-id="53f65-198">Per impostazione predefinita, non viene creato un errore di stato del modello se non viene trovato alcun valore per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="53f65-199">La proprietà viene impostata su Null o su un valore predefinito:</span><span class="sxs-lookup"><span data-stu-id="53f65-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="53f65-200">I tipi semplici nullable vengono impostati su `null`.</span><span class="sxs-lookup"><span data-stu-id="53f65-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="53f65-201">I tipi valore non nullable vengono impostati su `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="53f65-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="53f65-202">Ad esempio, un parametro `int id` viene impostato su 0.</span><span class="sxs-lookup"><span data-stu-id="53f65-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="53f65-203">Per i tipi complessi, l'associazione di modelli crea un'istanza usando il costruttore predefinito, senza impostare proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="53f65-204">Le matrici vengono impostate su `Array.Empty<T>()`, ad eccezione delle matrici `byte[]` che vengono impostate su `null`.</span><span class="sxs-lookup"><span data-stu-id="53f65-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="53f65-205">Se lo stato del modello deve essere invalidato quando non viene trovato alcun elemento nei campi del modulo per una proprietà del modello, usare l'attributo [`[BindRequired]`](#bindrequired-attribute) .</span><span class="sxs-lookup"><span data-stu-id="53f65-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="53f65-206">Si noti che questo comportamento `[BindRequired]` si applica all'associazione di modelli dai dati di moduli inviati e non ai dati JSON o XML nel corpo di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="53f65-207">I dati del corpo della richiesta vengono gestiti dai [formattatori di input](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="53f65-208">Errori di conversione dei tipi</span><span class="sxs-lookup"><span data-stu-id="53f65-208">Type conversion errors</span></span>

<span data-ttu-id="53f65-209">Se viene trovata un'origine ma non può essere convertita nel tipo di destinazione, lo stato del modello viene contrassegnato come non valido.</span><span class="sxs-lookup"><span data-stu-id="53f65-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="53f65-210">Il parametro o la proprietà di destinazione viene impostato su Null o su un valore predefinito, come indicato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="53f65-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="53f65-211">In un controller API con l'attributo `[ApiController]`, uno stato del modello non valido genera una risposta HTTP 400 automatica.</span><span class="sxs-lookup"><span data-stu-id="53f65-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="53f65-212">In una pagina Razor visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="53f65-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="53f65-213">La convalida sul lato client consente di rilevare la maggior parte dei dati non validi che, in caso contrario, verrebbero inviati a un modulo di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53f65-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="53f65-214">Questa convalida rende difficile attivare il codice sopra evidenziato.</span><span class="sxs-lookup"><span data-stu-id="53f65-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="53f65-215">L'app di esempio include un pulsante **Submit with Invalid Date** (Invia con data non valida) che inserisce dati non validi nel campo **Hire Date** (Data assunzione) e invia il modulo.</span><span class="sxs-lookup"><span data-stu-id="53f65-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="53f65-216">Questo pulsante illustra il funzionamento del codice per visualizzare di nuovo la pagina quando si verificano errori di conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="53f65-217">Quando la pagina viene nuovamente visualizzata dal codice precedente, l'input non valido non viene visualizzato nel campo modulo.</span><span class="sxs-lookup"><span data-stu-id="53f65-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="53f65-218">Questo avviene perché la proprietà del modello è stata impostata su Null o su un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="53f65-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="53f65-219">L'input non valido viene visualizzato in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="53f65-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="53f65-220">Ma se si vogliono visualizzare di nuovo i dati non validi nel campo modulo, è consigliabile impostare la proprietà del modello come stringa ed eseguire manualmente la conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="53f65-221">La stessa strategia è consigliata se si vuole evitare che gli errori di conversione del tipo causino errori di stato del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="53f65-222">In tal caso, impostare la proprietà del modello come stringa.</span><span class="sxs-lookup"><span data-stu-id="53f65-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="53f65-223">Tipi semplici</span><span class="sxs-lookup"><span data-stu-id="53f65-223">Simple types</span></span>

<span data-ttu-id="53f65-224">I tipi semplici in cui lo strumento di associazione di modelli può convertire le stringhe di origine includono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="53f65-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="53f65-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="53f65-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="53f65-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="53f65-227">Char</span><span class="sxs-lookup"><span data-stu-id="53f65-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="53f65-228">DateTime</span><span class="sxs-lookup"><span data-stu-id="53f65-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="53f65-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="53f65-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="53f65-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="53f65-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="53f65-231">Double</span><span class="sxs-lookup"><span data-stu-id="53f65-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="53f65-232">Enum</span><span class="sxs-lookup"><span data-stu-id="53f65-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="53f65-233">Guid</span><span class="sxs-lookup"><span data-stu-id="53f65-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="53f65-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="53f65-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="53f65-235">Single</span><span class="sxs-lookup"><span data-stu-id="53f65-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="53f65-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="53f65-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="53f65-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="53f65-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="53f65-238">Uri</span><span class="sxs-lookup"><span data-stu-id="53f65-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="53f65-239">Version</span><span class="sxs-lookup"><span data-stu-id="53f65-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="53f65-240">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="53f65-240">Complex types</span></span>

<span data-ttu-id="53f65-241">Un tipo pubblico deve avere un costruttore predefinito pubblico e proprietà scrivibili pubbliche da associare.</span><span class="sxs-lookup"><span data-stu-id="53f65-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="53f65-242">Quando si verifica l'associazione di modelli, vengono create istanze della classe usando il costruttore predefinito pubblico.</span><span class="sxs-lookup"><span data-stu-id="53f65-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="53f65-243">Per ogni proprietà del tipo complesso, l'associazione di modelli cerca il modello di nome *prefisso.nome_proprietà* nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="53f65-244">Se non viene trovato, cerca semplicemente *nome_proprietà* senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="53f65-245">Per l'associazione a un parametro, il prefisso è il nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="53f65-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="53f65-246">Per l'associazione a una proprietà pubblica `PageModel`, il prefisso è il nome della proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="53f65-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="53f65-247">Alcuni attributi hanno una proprietà `Prefix` che consente di eseguire l'override dell'utilizzo predefinito del nome di un parametro o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="53f65-248">Ad esempio, si supponga che il tipo complesso sia la classe `Instructor` seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="53f65-249">Prefisso = nome del parametro</span><span class="sxs-lookup"><span data-stu-id="53f65-249">Prefix = parameter name</span></span>

<span data-ttu-id="53f65-250">Se il modello da associare è un parametro denominato `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="53f65-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="53f65-251">L'associazione di modelli cerca prima di tutto la chiave `instructorToUpdate.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="53f65-252">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="53f65-253">Prefisso = nome della proprietà</span><span class="sxs-lookup"><span data-stu-id="53f65-253">Prefix = property name</span></span>

<span data-ttu-id="53f65-254">Se il modello da associare è una proprietà denominata `Instructor` del controller o della classe `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="53f65-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="53f65-255">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="53f65-256">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="53f65-257">Prefisso personalizzato</span><span class="sxs-lookup"><span data-stu-id="53f65-257">Custom prefix</span></span>

<span data-ttu-id="53f65-258">Se il modello da associare è un parametro denominato `instructorToUpdate` e un attributo `Bind` specifica `Instructor` come prefisso:</span><span class="sxs-lookup"><span data-stu-id="53f65-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="53f65-259">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="53f65-260">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="53f65-261">Attributi per le destinazioni di tipo complesso</span><span class="sxs-lookup"><span data-stu-id="53f65-261">Attributes for complex type targets</span></span>

<span data-ttu-id="53f65-262">Sono disponibili vari attributi predefiniti per controllare l'associazione di modelli di tipi complessi:</span><span class="sxs-lookup"><span data-stu-id="53f65-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="53f65-263">Questi attributi influiscono sul modello di associazione quando i dati di modulo inviati sono l'origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="53f65-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="53f65-264">Non influiscono sui formattatori di input, che elaborano corpi di richieste JSON e XML inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="53f65-265">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="53f65-266">Vedere anche la discussione relativa all'attributo `[Required]` in [Convalida del modello](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="53f65-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="53f65-267">Attributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="53f65-267">[BindRequired] attribute</span></span>

<span data-ttu-id="53f65-268">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="53f65-269">Con questo attributo l'associazione di modelli aggiunge un errore di stato del modello se non è possibile eseguire l'associazione per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="53f65-270">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="53f65-271">Attributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="53f65-271">[BindNever] attribute</span></span>

<span data-ttu-id="53f65-272">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="53f65-273">Impedisce all'associazione di modelli di impostare una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="53f65-274">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="53f65-275">Attributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="53f65-275">[Bind] attribute</span></span>

<span data-ttu-id="53f65-276">Può essere applicato a una classe o a un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="53f65-277">Specifica quali proprietà di un modello devono essere incluse nell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="53f65-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="53f65-278">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato qualsiasi gestore o metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="53f65-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="53f65-279">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato il metodo `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="53f65-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="53f65-280">L'attributo `[Bind]` può essere usato per evitare l'overposting negli scenari di *creazione*.</span><span class="sxs-lookup"><span data-stu-id="53f65-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="53f65-281">Non funziona bene negli scenari di modifica perché le proprietà escluse vengono impostate su Null o su un valore predefinito anziché rimanere inalterate.</span><span class="sxs-lookup"><span data-stu-id="53f65-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="53f65-282">Per difendersi dall'overposting, sono consigliati i modelli di visualizzazione anziché l'attributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="53f65-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="53f65-283">Per altre informazioni, vedere [Nota sulla sicurezza relativa all'overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="53f65-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="53f65-284">Raccolte</span><span class="sxs-lookup"><span data-stu-id="53f65-284">Collections</span></span>

<span data-ttu-id="53f65-285">Per le destinazioni che sono raccolte di tipi semplici, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="53f65-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="53f65-286">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="53f65-287">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-287">For example:</span></span>

* <span data-ttu-id="53f65-288">Si supponga che il parametro da associare sia una matrice denominata `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="53f65-289">I dati di modulo o di stringhe di query possono essere in uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="53f65-290">Il formato seguente è supportato solo nei dati di modulo:</span><span class="sxs-lookup"><span data-stu-id="53f65-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="53f65-291">Per tutti i formati di esempio precedenti, l'associazione di modelli passa una matrice di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="53f65-292">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="53f65-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="53f65-293">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="53f65-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="53f65-294">Per i formati di dati che usano numeri con indice (... [0]... [1]...) è necessario assicurarsi che la numerazione sia progressiva a partire da zero.</span><span class="sxs-lookup"><span data-stu-id="53f65-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="53f65-295">Se sono presenti eventuali interruzioni nella numerazione con indice, tutti gli elementi dopo l'interruzione vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="53f65-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="53f65-296">Ad esempio, se gli indici sono 0 e 2 anziché 0 e 1, il secondo elemento viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="53f65-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="53f65-297">Dizionari</span><span class="sxs-lookup"><span data-stu-id="53f65-297">Dictionaries</span></span>

<span data-ttu-id="53f65-298">Per le destinazioni `Dictionary`, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="53f65-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="53f65-299">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="53f65-300">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-300">For example:</span></span>

* <span data-ttu-id="53f65-301">Si supponga che il parametro di destinazione sia un elemento `Dictionary<int, string>` denominato `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="53f65-302">I dati di modulo o di stringhe di query inviati possono essere simili a uno degli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="53f65-303">Per tutti i formati di esempio precedenti, l'associazione di modelli passa un dizionario di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="53f65-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="53f65-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="53f65-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="53f65-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="53f65-306">Comportamento di globalizzazione dell'associazione di modelli dati della route e stringhe di query</span><span class="sxs-lookup"><span data-stu-id="53f65-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="53f65-307">Il provider del valore della route ASP.NET Core e il provider del valore della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="53f65-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="53f65-308">Considera i valori come impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="53f65-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="53f65-309">Si prevede che gli URL siano impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="53f65-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="53f65-310">Al contrario, i valori provenienti dai dati del form subiscono una conversione con distinzione delle impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="53f65-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="53f65-311">Questa operazione è progettata in modo che gli URL siano condivisibili tra le impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="53f65-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="53f65-312">Per rendere il provider del valore di route ASP.NET Core e il provider di valori della stringa di query sottoposti a una conversione dipendente dalle impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="53f65-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="53f65-313">Ereditano da <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="53f65-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="53f65-314">Copiare il codice da [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="53f65-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="53f65-315">Sostituire il [valore delle impostazioni cultura](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passato al costruttore del provider di valori con [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="53f65-315">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="53f65-316">Sostituire la factory del provider di valori predefinito in opzioni MVC con la nuova:</span><span class="sxs-lookup"><span data-stu-id="53f65-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="53f65-317">Tipi di dati speciali</span><span class="sxs-lookup"><span data-stu-id="53f65-317">Special data types</span></span>

<span data-ttu-id="53f65-318">Esistono alcuni tipi di dati speciali che l'associazione di modelli può gestire.</span><span class="sxs-lookup"><span data-stu-id="53f65-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="53f65-319">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="53f65-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="53f65-320">Un file caricato incluso nella richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="53f65-321">È anche supportato `IEnumerable<IFormFile>` per più file.</span><span class="sxs-lookup"><span data-stu-id="53f65-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="53f65-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="53f65-322">CancellationToken</span></span>

<span data-ttu-id="53f65-323">usato per annullare l'attività nei controller asincroni.</span><span class="sxs-lookup"><span data-stu-id="53f65-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="53f65-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="53f65-324">FormCollection</span></span>

<span data-ttu-id="53f65-325">Usato per recuperare tutti i valori dai dati di modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="53f65-326">Formattatori di input</span><span class="sxs-lookup"><span data-stu-id="53f65-326">Input formatters</span></span>

<span data-ttu-id="53f65-327">I dati nel corpo della richiesta possono essere in formato JSON, XML o in altri formati.</span><span class="sxs-lookup"><span data-stu-id="53f65-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="53f65-328">Per analizzare questi dati, l'associazione di modelli usa un *formattatore di input* configurato in modo da gestire un particolare tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="53f65-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="53f65-329">Per impostazione predefinita, ASP.NET Core include i formattatori di input basati su JSON per la gestione dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="53f65-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="53f65-330">È possibile aggiungere altri formattatori per altri tipi di contenuto.</span><span class="sxs-lookup"><span data-stu-id="53f65-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="53f65-331">ASP.NET Core consente di selezionare i formattatori di input in base all'attributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="53f65-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="53f65-332">Se non è presente alcun attributo, viene usata l'[intestazione Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="53f65-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="53f65-333">Per usare i formattatori di input XML predefiniti:</span><span class="sxs-lookup"><span data-stu-id="53f65-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="53f65-334">Installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="53f65-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="53f65-335">In `Startup.ConfigureServices` chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="53f65-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=10)]

* <span data-ttu-id="53f65-336">Applicare l'attributo `Consumes` alle classi controller o ai metodi di azione che devono aspettarsi XML nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="53f65-337">Per altre informazioni, vedere [Introduzione alla serializzazione XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="53f65-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

### <a name="customize-model-binding-with-input-formatters"></a><span data-ttu-id="53f65-338">Personalizzare l'associazione di modelli con formattatori di input</span><span class="sxs-lookup"><span data-stu-id="53f65-338">Customize model binding with input formatters</span></span>

<span data-ttu-id="53f65-339">Un formattatore di input assume la piena responsabilità per la lettura dei dati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-339">An input formatter takes full responsibility for reading data from the request body.</span></span> <span data-ttu-id="53f65-340">Per personalizzare questo processo, configurare le API usate dal formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="53f65-340">To customize this process, configure the APIs used by the input formatter.</span></span> <span data-ttu-id="53f65-341">In questa sezione viene descritto come personalizzare il formattatore di input basato su `System.Text.Json`per comprendere un tipo personalizzato denominato `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="53f65-341">This section describes how to customize the `System.Text.Json`-based input formatter to understand a custom type named `ObjectId`.</span></span> 

<span data-ttu-id="53f65-342">Si consideri il modello seguente, che contiene una proprietà `ObjectId` personalizzata denominata `Id`:</span><span class="sxs-lookup"><span data-stu-id="53f65-342">Consider the following model, which contains a custom `ObjectId` property named `Id`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ModelWithObjectId.cs?name=snippet_Class&highlight=3)]

<span data-ttu-id="53f65-343">Per personalizzare il processo di associazione del modello quando si usa `System.Text.Json`, creare una classe derivata da <xref:System.Text.Json.Serialization.JsonConverter%601>:</span><span class="sxs-lookup"><span data-stu-id="53f65-343">To customize the model binding process when using `System.Text.Json`, create a class derived from <xref:System.Text.Json.Serialization.JsonConverter%601>:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/JsonConverters/ObjectIdConverter.cs?name=snippet_Class)]

<span data-ttu-id="53f65-344">Per usare un convertitore personalizzato, applicare l'attributo <xref:System.Text.Json.Serialization.JsonConverterAttribute> al tipo.</span><span class="sxs-lookup"><span data-stu-id="53f65-344">To use a custom converter, apply the <xref:System.Text.Json.Serialization.JsonConverterAttribute> attribute to the type.</span></span> <span data-ttu-id="53f65-345">Nell'esempio seguente il tipo di `ObjectId` viene configurato con `ObjectIdConverter` come convertitore personalizzato:</span><span class="sxs-lookup"><span data-stu-id="53f65-345">In the following example, the `ObjectId` type is configured with `ObjectIdConverter` as its custom converter:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ObjectId.cs?name=snippet_Class&highlight=1)]

<span data-ttu-id="53f65-346">Per ulteriori informazioni, vedere [come scrivere convertitori personalizzati](/dotnet/standard/serialization/system-text-json-converters-how-to).</span><span class="sxs-lookup"><span data-stu-id="53f65-346">For more information, see [How to write custom converters](/dotnet/standard/serialization/system-text-json-converters-how-to).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="53f65-347">Escludere i tipi specificati dall'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="53f65-347">Exclude specified types from model binding</span></span>

<span data-ttu-id="53f65-348">Il comportamento dell'associazione di modelli e del sistema di convalida è determinato da [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="53f65-348">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="53f65-349">È possibile personalizzare `ModelMetadata` mediante l'aggiunta di un provider di dettagli a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="53f65-349">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="53f65-350">Sono disponibili provider di dettagli predefiniti per la disabilitazione dell'associazione di modelli o la convalida per i tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="53f65-350">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="53f65-351">Per disabilitare l'associazione di modelli per tutti i modelli di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-351">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="53f65-352">Ad esempio, per disabilitare l'associazione di modelli per tutti i modelli di tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="53f65-352">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=5-6)]

<span data-ttu-id="53f65-353">Per disabilitare la convalida per le proprietà di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-353">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="53f65-354">Ad esempio, per disabilitare la convalida per le proprietà di tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="53f65-354">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=7-8)]

## <a name="custom-model-binders"></a><span data-ttu-id="53f65-355">Strumenti di associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="53f65-355">Custom model binders</span></span>

<span data-ttu-id="53f65-356">È possibile estendere l'associazione di modelli scrivendo uno strumento di associazione di modelli personalizzato e usando l'attributo `[ModelBinder]` per selezionarlo per una determinata destinazione.</span><span class="sxs-lookup"><span data-stu-id="53f65-356">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="53f65-357">Altre informazioni sull'[associazione di modelli personalizzata](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="53f65-357">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="53f65-358">Associazione di modelli manuale</span><span class="sxs-lookup"><span data-stu-id="53f65-358">Manual model binding</span></span> 

<span data-ttu-id="53f65-359">L'associazione di modelli può essere richiamata manualmente usando il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="53f65-359">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="53f65-360">Il metodo è definito in entrambe le classi `ControllerBase` e `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="53f65-360">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="53f65-361">Gli overload del metodo consentono di specificare il prefisso e il provider di valori da usare.</span><span class="sxs-lookup"><span data-stu-id="53f65-361">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="53f65-362">Il metodo restituisce `false` se l'associazione di modelli non riesce.</span><span class="sxs-lookup"><span data-stu-id="53f65-362">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="53f65-363">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-363">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

<span data-ttu-id="53f65-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> usa i provider di valori per ottenere i dati dal corpo del form, dalla stringa di query e dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="53f65-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>  uses value providers to get data from the form body, query string, and route data.</span></span> <span data-ttu-id="53f65-365">`TryUpdateModelAsync` è in genere:</span><span class="sxs-lookup"><span data-stu-id="53f65-365">`TryUpdateModelAsync` is typically:</span></span> 

* <span data-ttu-id="53f65-366">Usato con le app Razor Pages e MVC usando i controller e le visualizzazioni per impedire l'overposting.</span><span class="sxs-lookup"><span data-stu-id="53f65-366">Used with Razor Pages and MVC apps using controllers and views to prevent over-posting.</span></span>
* <span data-ttu-id="53f65-367">Non utilizzato con un'API Web, a meno che non venga utilizzato da dati del modulo, stringhe di query e dati di route.</span><span class="sxs-lookup"><span data-stu-id="53f65-367">Not used with a web API unless consumed from form data, query strings, and route data.</span></span> <span data-ttu-id="53f65-368">Gli endpoint dell'API Web che utilizzano JSON utilizzano [formattatori di input](#input-formatters) per deserializzare il corpo della richiesta in un oggetto.</span><span class="sxs-lookup"><span data-stu-id="53f65-368">Web API endpoints that consume JSON use [Input formatters](#input-formatters) to deserialize the request body into an object.</span></span>

<span data-ttu-id="53f65-369">Per ulteriori informazioni, vedere [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).</span><span class="sxs-lookup"><span data-stu-id="53f65-369">For more information, see [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).</span></span>

## <a name="fromservices-attribute"></a><span data-ttu-id="53f65-370">Attributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="53f65-370">[FromServices] attribute</span></span>

<span data-ttu-id="53f65-371">Il nome di questo attributo segue il modello degli attributi di associazione di modelli che specificano un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-371">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="53f65-372">Non si tratta però dell'associazione di dati da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="53f65-372">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="53f65-373">Ottiene un'istanza di un tipo dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="53f65-373">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="53f65-374">Lo scopo è fornire un'alternativa all'inserimento del costruttore per quando è necessario un servizio solo se viene chiamato un metodo specifico.</span><span class="sxs-lookup"><span data-stu-id="53f65-374">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53f65-375">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53f65-375">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="53f65-376">Questo articolo illustra cos'è l'associazione di modelli, come funziona e come personalizzarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="53f65-376">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="53f65-377">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="53f65-377">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="53f65-378">Che cos'è l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="53f65-378">What is Model binding</span></span>

<span data-ttu-id="53f65-379">I controller e le pagine Razor operano sui dati provenienti dalle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-379">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="53f65-380">Ad esempio, i dati di route possono fornire una chiave del record e i campi modulo inviati possono fornire valori per le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-380">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="53f65-381">La scrittura di codice per recuperare tutti i valori e convertirli da stringhe in tipi .NET sarebbe noiosa e soggetta a errori.</span><span class="sxs-lookup"><span data-stu-id="53f65-381">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="53f65-382">L'associazione di modelli consente di automatizzare questo processo.</span><span class="sxs-lookup"><span data-stu-id="53f65-382">Model binding automates this process.</span></span> <span data-ttu-id="53f65-383">Il sistema di associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="53f65-383">The model binding system:</span></span>

* <span data-ttu-id="53f65-384">Recupera i dati da diverse origini, ad esempio i dati di route, i campi modulo e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-384">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="53f65-385">Fornisce i dati ai controller e alle pagine Razor in parametri di metodo e proprietà pubbliche.</span><span class="sxs-lookup"><span data-stu-id="53f65-385">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="53f65-386">Converte dati stringa in tipi .NET.</span><span class="sxs-lookup"><span data-stu-id="53f65-386">Converts string data to .NET types.</span></span>
* <span data-ttu-id="53f65-387">Aggiorna le proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="53f65-387">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="53f65-388">Esempio</span><span class="sxs-lookup"><span data-stu-id="53f65-388">Example</span></span>

<span data-ttu-id="53f65-389">Si supponga di avere il metodo di azione seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-389">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="53f65-390">E l'app riceve una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="53f65-390">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="53f65-391">L'associazione di modelli esegue i passaggi seguenti dopo che il sistema di routing seleziona il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="53f65-391">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="53f65-392">Trova il primo parametro di `GetByID`, un intero denominato `id`.</span><span class="sxs-lookup"><span data-stu-id="53f65-392">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="53f65-393">Esamina le origini disponibili nella richiesta HTTP e trova `id` = "2" nei dati di route.</span><span class="sxs-lookup"><span data-stu-id="53f65-393">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="53f65-394">Converte la stringa "2" nell'intero 2.</span><span class="sxs-lookup"><span data-stu-id="53f65-394">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="53f65-395">Trova il parametro successivo di `GetByID`, un valore booleano denominato `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="53f65-395">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="53f65-396">Esamina le origini e trova "DogsOnly=true" nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-396">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="53f65-397">Per la corrispondenza dei nomi non viene applicata la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="53f65-397">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="53f65-398">Converte la stringa "true" nel valore booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="53f65-398">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="53f65-399">Il framework chiama quindi il metodo `GetById`, passando 2 per il parametro `id` e `true` per il parametro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="53f65-399">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="53f65-400">Nell'esempio precedente le destinazioni dell'associazione di modelli sono parametri di metodo che sono tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="53f65-400">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="53f65-401">Le destinazioni possono essere anche le proprietà di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="53f65-401">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="53f65-402">Dopo l'associazione di ogni proprietà, viene eseguita la [convalida dei modelli](xref:mvc/models/validation) per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-402">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="53f65-403">Il record dei dati associati al modello e di eventuali errori di associazione o convalida viene archiviato in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) oppure [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="53f65-403">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="53f65-404">Per scoprire se questo processo ha esito positivo, l'app controlla il flag [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="53f65-404">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="53f65-405">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="53f65-405">Targets</span></span>

<span data-ttu-id="53f65-406">L'associazione di modelli cerca di trovare i valori per i tipi di destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-406">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="53f65-407">Parametri del metodo di azione del controller a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-407">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="53f65-408">Parametri del metodo del gestore di Razor Pages a cui viene indirizzata una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-408">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="53f65-409">Proprietà pubbliche di un controller o una classe `PageModel`, se specificate dagli attributi.</span><span class="sxs-lookup"><span data-stu-id="53f65-409">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="53f65-410">Attributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="53f65-410">[BindProperty] attribute</span></span>

<span data-ttu-id="53f65-411">Può essere applicato a una proprietà pubblica di un controller o una classe `PageModel` per fare in modo che l'associazione di modelli usi tale proprietà come destinazione:</span><span class="sxs-lookup"><span data-stu-id="53f65-411">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="53f65-412">Attributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="53f65-412">[BindProperties] attribute</span></span>

<span data-ttu-id="53f65-413">Disponibile in ASP.NET Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="53f65-413">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="53f65-414">Può essere applicato a un controller o una classe `PageModel` per indicare all'associazione del modello di usare tutte le proprietà pubbliche della classe come destinazione:</span><span class="sxs-lookup"><span data-stu-id="53f65-414">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="53f65-415">Associazione di modelli per le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="53f65-415">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="53f65-416">Per impostazione predefinita, le proprietà non vengono associate per le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="53f65-416">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="53f65-417">In genere, per una richiesta GET è sufficiente un parametro ID record.</span><span class="sxs-lookup"><span data-stu-id="53f65-417">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="53f65-418">L'ID record viene usato per cercare l'elemento nel database.</span><span class="sxs-lookup"><span data-stu-id="53f65-418">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="53f65-419">Pertanto, non è necessario associare una proprietà che contiene un'istanza del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-419">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="53f65-420">Negli scenari in cui si vogliono associare le proprietà ai dati dalle richieste GET, impostare la proprietà `SupportsGet` su `true`:</span><span class="sxs-lookup"><span data-stu-id="53f65-420">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="53f65-421">Origini</span><span class="sxs-lookup"><span data-stu-id="53f65-421">Sources</span></span>

<span data-ttu-id="53f65-422">Per impostazione predefinita, l'associazione di modelli ottiene i dati sotto forma di coppie chiave-valore dalle origini seguenti in una richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="53f65-422">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="53f65-423">Campi modulo</span><span class="sxs-lookup"><span data-stu-id="53f65-423">Form fields</span></span>
1. <span data-ttu-id="53f65-424">Il corpo della richiesta (per i controller [ con l'attributo [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="53f65-424">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="53f65-425">Dati della route</span><span class="sxs-lookup"><span data-stu-id="53f65-425">Route data</span></span>
1. <span data-ttu-id="53f65-426">Parametri della stringa di query</span><span class="sxs-lookup"><span data-stu-id="53f65-426">Query string parameters</span></span>
1. <span data-ttu-id="53f65-427">File caricati</span><span class="sxs-lookup"><span data-stu-id="53f65-427">Uploaded files</span></span>

<span data-ttu-id="53f65-428">Per ogni parametro o proprietà di destinazione, le origini vengono analizzate nell'ordine indicato nell'elenco precedente.</span><span class="sxs-lookup"><span data-stu-id="53f65-428">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="53f65-429">Esistono tuttavia alcune eccezioni:</span><span class="sxs-lookup"><span data-stu-id="53f65-429">There are a few exceptions:</span></span>

* <span data-ttu-id="53f65-430">I dati di route e i valori delle stringhe di query vengono usati solo per i tipi semplici.</span><span class="sxs-lookup"><span data-stu-id="53f65-430">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="53f65-431">I file caricati vengono associati solo ai tipi di destinazione che implementano `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="53f65-431">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="53f65-432">Se l'origine predefinita non è corretta, usare uno degli attributi seguenti per specificare l'origine:</span><span class="sxs-lookup"><span data-stu-id="53f65-432">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="53f65-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -ottiene i valori dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="53f65-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -ottiene i valori dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="53f65-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="53f65-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -ottiene i valori dai campi del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="53f65-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -ottiene i valori dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="53f65-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -ottiene i valori dalle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="53f65-438">Questi attributi:</span><span class="sxs-lookup"><span data-stu-id="53f65-438">These attributes:</span></span>

* <span data-ttu-id="53f65-439">Vengono aggiunti singolarmente alle proprietà del modello (non alla classe del modello), come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-439">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="53f65-440">Accettano facoltativamente un valore di nome di modello nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="53f65-440">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="53f65-441">Questa opzione è disponibile nel caso in cui il nome della proprietà non corrisponda al valore nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-441">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="53f65-442">Il valore nella richiesta, ad esempio, potrebbe essere un'intestazione con un segno meno nel nome, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-442">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="53f65-443">Attributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="53f65-443">[FromBody] attribute</span></span>

<span data-ttu-id="53f65-444">Applicare l'attributo `[FromBody]` a un parametro per popolarne le proprietà dal corpo di una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-444">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="53f65-445">Il runtime di ASP.NET Core delega la responsabilità della lettura del corpo a un formattatore di input.</span><span class="sxs-lookup"><span data-stu-id="53f65-445">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="53f65-446">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-446">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="53f65-447">Quando `[FromBody]` viene applicato a un parametro di tipo complesso, tutti gli attributi di origine di associazione applicati alle relative proprietà vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="53f65-447">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="53f65-448">Ad esempio, il `Create` azione seguente specifica che il relativo parametro `pet` viene popolato dal corpo:</span><span class="sxs-lookup"><span data-stu-id="53f65-448">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="53f65-449">La classe `Pet` specifica che la relativa proprietà `Breed` viene popolata da un parametro della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="53f65-449">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="53f65-450">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="53f65-450">In the preceding example:</span></span>

* <span data-ttu-id="53f65-451">L'attributo `[FromQuery]` viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="53f65-451">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="53f65-452">La proprietà `Breed` non viene popolata da un parametro della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="53f65-452">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="53f65-453">I formattatori di input leggono solo il corpo e non comprendono gli attributi di origine del binding.</span><span class="sxs-lookup"><span data-stu-id="53f65-453">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="53f65-454">Se nel corpo viene trovato un valore appropriato, tale valore viene usato per popolare la proprietà `Breed`.</span><span class="sxs-lookup"><span data-stu-id="53f65-454">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="53f65-455">Non applicare `[FromBody]` a più di un parametro per ogni metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="53f65-455">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="53f65-456">Quando il flusso di richieste viene letto da un formattatore di input, non è più disponibile per la lettura per l'associazione di altri parametri `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="53f65-456">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="53f65-457">Origini aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53f65-457">Additional sources</span></span>

<span data-ttu-id="53f65-458">I dati di origine vengono forniti al sistema di associazione di modelli dai *provider di valori*.</span><span class="sxs-lookup"><span data-stu-id="53f65-458">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="53f65-459">È possibile scrivere e registrare i provider di valori personalizzati che recuperano i dati per l'associazione di modelli da altre origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-459">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="53f65-460">Ad esempio, potrebbe essere necessario recuperare dati dai cookie o dallo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="53f65-460">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="53f65-461">Per ottenere dati da una nuova origine:</span><span class="sxs-lookup"><span data-stu-id="53f65-461">To get data from a new source:</span></span>

* <span data-ttu-id="53f65-462">Creare una classe che implementi `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="53f65-462">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="53f65-463">Creare una classe che implementi `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="53f65-463">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="53f65-464">Registrare la classe factory in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-464">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="53f65-465">L'app di esempio include un esempio di [provider di valori](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) e [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) che ottiene i valori dai cookie.</span><span class="sxs-lookup"><span data-stu-id="53f65-465">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="53f65-466">Ecco il codice di registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53f65-466">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="53f65-467">Il codice illustrato posiziona il provider di valori personalizzati dopo tutti i provider di valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="53f65-467">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="53f65-468">Per renderlo il primo nell'elenco, chiamare `Insert(0, new CookieValueProviderFactory())` invece di `Add`.</span><span class="sxs-lookup"><span data-stu-id="53f65-468">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="53f65-469">Nessuna origine per una proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="53f65-469">No source for a model property</span></span>

<span data-ttu-id="53f65-470">Per impostazione predefinita, non viene creato un errore di stato del modello se non viene trovato alcun valore per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-470">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="53f65-471">La proprietà viene impostata su Null o su un valore predefinito:</span><span class="sxs-lookup"><span data-stu-id="53f65-471">The property is set to null or a default value:</span></span>

* <span data-ttu-id="53f65-472">I tipi semplici nullable vengono impostati su `null`.</span><span class="sxs-lookup"><span data-stu-id="53f65-472">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="53f65-473">I tipi valore non nullable vengono impostati su `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="53f65-473">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="53f65-474">Ad esempio, un parametro `int id` viene impostato su 0.</span><span class="sxs-lookup"><span data-stu-id="53f65-474">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="53f65-475">Per i tipi complessi, l'associazione di modelli crea un'istanza usando il costruttore predefinito, senza impostare proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-475">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="53f65-476">Le matrici vengono impostate su `Array.Empty<T>()`, ad eccezione delle matrici `byte[]` che vengono impostate su `null`.</span><span class="sxs-lookup"><span data-stu-id="53f65-476">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="53f65-477">Se lo stato del modello deve essere invalidato quando non viene trovato alcun elemento nei campi del modulo per una proprietà del modello, usare l'attributo [`[BindRequired]`](#bindrequired-attribute) .</span><span class="sxs-lookup"><span data-stu-id="53f65-477">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="53f65-478">Si noti che questo comportamento `[BindRequired]` si applica all'associazione di modelli dai dati di moduli inviati e non ai dati JSON o XML nel corpo di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-478">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="53f65-479">I dati del corpo della richiesta vengono gestiti dai [formattatori di input](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-479">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="53f65-480">Errori di conversione dei tipi</span><span class="sxs-lookup"><span data-stu-id="53f65-480">Type conversion errors</span></span>

<span data-ttu-id="53f65-481">Se viene trovata un'origine ma non può essere convertita nel tipo di destinazione, lo stato del modello viene contrassegnato come non valido.</span><span class="sxs-lookup"><span data-stu-id="53f65-481">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="53f65-482">Il parametro o la proprietà di destinazione viene impostato su Null o su un valore predefinito, come indicato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="53f65-482">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="53f65-483">In un controller API con l'attributo `[ApiController]`, uno stato del modello non valido genera una risposta HTTP 400 automatica.</span><span class="sxs-lookup"><span data-stu-id="53f65-483">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="53f65-484">In una pagina Razor visualizzare di nuovo la pagina con un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="53f65-484">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="53f65-485">La convalida sul lato client consente di rilevare la maggior parte dei dati non validi che, in caso contrario, verrebbero inviati a un modulo di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53f65-485">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="53f65-486">Questa convalida rende difficile attivare il codice sopra evidenziato.</span><span class="sxs-lookup"><span data-stu-id="53f65-486">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="53f65-487">L'app di esempio include un pulsante **Submit with Invalid Date** (Invia con data non valida) che inserisce dati non validi nel campo **Hire Date** (Data assunzione) e invia il modulo.</span><span class="sxs-lookup"><span data-stu-id="53f65-487">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="53f65-488">Questo pulsante illustra il funzionamento del codice per visualizzare di nuovo la pagina quando si verificano errori di conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-488">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="53f65-489">Quando la pagina viene nuovamente visualizzata dal codice precedente, l'input non valido non viene visualizzato nel campo modulo.</span><span class="sxs-lookup"><span data-stu-id="53f65-489">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="53f65-490">Questo avviene perché la proprietà del modello è stata impostata su Null o su un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="53f65-490">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="53f65-491">L'input non valido viene visualizzato in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="53f65-491">The invalid input does appear in an error message.</span></span> <span data-ttu-id="53f65-492">Ma se si vogliono visualizzare di nuovo i dati non validi nel campo modulo, è consigliabile impostare la proprietà del modello come stringa ed eseguire manualmente la conversione dei dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-492">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="53f65-493">La stessa strategia è consigliata se si vuole evitare che gli errori di conversione del tipo causino errori di stato del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-493">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="53f65-494">In tal caso, impostare la proprietà del modello come stringa.</span><span class="sxs-lookup"><span data-stu-id="53f65-494">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="53f65-495">Tipi semplici</span><span class="sxs-lookup"><span data-stu-id="53f65-495">Simple types</span></span>

<span data-ttu-id="53f65-496">I tipi semplici in cui lo strumento di associazione di modelli può convertire le stringhe di origine includono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-496">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="53f65-497">Boolean</span><span class="sxs-lookup"><span data-stu-id="53f65-497">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="53f65-498">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="53f65-498">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="53f65-499">Char</span><span class="sxs-lookup"><span data-stu-id="53f65-499">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="53f65-500">DateTime</span><span class="sxs-lookup"><span data-stu-id="53f65-500">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="53f65-501">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="53f65-501">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="53f65-502">Decimal</span><span class="sxs-lookup"><span data-stu-id="53f65-502">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="53f65-503">Double</span><span class="sxs-lookup"><span data-stu-id="53f65-503">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="53f65-504">Enum</span><span class="sxs-lookup"><span data-stu-id="53f65-504">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="53f65-505">Guid</span><span class="sxs-lookup"><span data-stu-id="53f65-505">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="53f65-506">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="53f65-506">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="53f65-507">Single</span><span class="sxs-lookup"><span data-stu-id="53f65-507">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="53f65-508">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="53f65-508">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="53f65-509">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="53f65-509">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="53f65-510">Uri</span><span class="sxs-lookup"><span data-stu-id="53f65-510">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="53f65-511">Version</span><span class="sxs-lookup"><span data-stu-id="53f65-511">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="53f65-512">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="53f65-512">Complex types</span></span>

<span data-ttu-id="53f65-513">Un tipo pubblico deve avere un costruttore predefinito pubblico e proprietà scrivibili pubbliche da associare.</span><span class="sxs-lookup"><span data-stu-id="53f65-513">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="53f65-514">Quando si verifica l'associazione di modelli, vengono create istanze della classe usando il costruttore predefinito pubblico.</span><span class="sxs-lookup"><span data-stu-id="53f65-514">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="53f65-515">Per ogni proprietà del tipo complesso, l'associazione di modelli cerca il modello di nome *prefisso.nome_proprietà* nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-515">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="53f65-516">Se non viene trovato, cerca semplicemente *nome_proprietà* senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-516">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="53f65-517">Per l'associazione a un parametro, il prefisso è il nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="53f65-517">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="53f65-518">Per l'associazione a una proprietà pubblica `PageModel`, il prefisso è il nome della proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="53f65-518">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="53f65-519">Alcuni attributi hanno una proprietà `Prefix` che consente di eseguire l'override dell'utilizzo predefinito del nome di un parametro o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="53f65-519">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="53f65-520">Ad esempio, si supponga che il tipo complesso sia la classe `Instructor` seguente:</span><span class="sxs-lookup"><span data-stu-id="53f65-520">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="53f65-521">Prefisso = nome del parametro</span><span class="sxs-lookup"><span data-stu-id="53f65-521">Prefix = parameter name</span></span>

<span data-ttu-id="53f65-522">Se il modello da associare è un parametro denominato `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="53f65-522">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="53f65-523">L'associazione di modelli cerca prima di tutto la chiave `instructorToUpdate.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-523">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="53f65-524">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-524">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="53f65-525">Prefisso = nome della proprietà</span><span class="sxs-lookup"><span data-stu-id="53f65-525">Prefix = property name</span></span>

<span data-ttu-id="53f65-526">Se il modello da associare è una proprietà denominata `Instructor` del controller o della classe `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="53f65-526">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="53f65-527">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-527">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="53f65-528">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-528">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="53f65-529">Prefisso personalizzato</span><span class="sxs-lookup"><span data-stu-id="53f65-529">Custom prefix</span></span>

<span data-ttu-id="53f65-530">Se il modello da associare è un parametro denominato `instructorToUpdate` e un attributo `Bind` specifica `Instructor` come prefisso:</span><span class="sxs-lookup"><span data-stu-id="53f65-530">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="53f65-531">L'associazione di modelli cerca prima di tutto la chiave `Instructor.ID` nelle origini.</span><span class="sxs-lookup"><span data-stu-id="53f65-531">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="53f65-532">Se non viene trovata, cerca `ID` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-532">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="53f65-533">Attributi per le destinazioni di tipo complesso</span><span class="sxs-lookup"><span data-stu-id="53f65-533">Attributes for complex type targets</span></span>

<span data-ttu-id="53f65-534">Sono disponibili vari attributi predefiniti per controllare l'associazione di modelli di tipi complessi:</span><span class="sxs-lookup"><span data-stu-id="53f65-534">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="53f65-535">Questi attributi influiscono sul modello di associazione quando i dati di modulo inviati sono l'origine dei valori.</span><span class="sxs-lookup"><span data-stu-id="53f65-535">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="53f65-536">Non influiscono sui formattatori di input, che elaborano corpi di richieste JSON e XML inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-536">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="53f65-537">I formattatori di input sono descritti [più avanti in questo articolo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="53f65-537">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="53f65-538">Vedere anche la discussione relativa all'attributo `[Required]` in [Convalida del modello](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="53f65-538">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="53f65-539">Attributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="53f65-539">[BindRequired] attribute</span></span>

<span data-ttu-id="53f65-540">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-540">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="53f65-541">Con questo attributo l'associazione di modelli aggiunge un errore di stato del modello se non è possibile eseguire l'associazione per una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-541">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="53f65-542">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-542">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="53f65-543">Attributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="53f65-543">[BindNever] attribute</span></span>

<span data-ttu-id="53f65-544">Può essere applicato solo alle proprietà del modello e non ai parametri di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-544">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="53f65-545">Impedisce all'associazione di modelli di impostare una proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="53f65-545">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="53f65-546">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-546">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="53f65-547">Attributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="53f65-547">[Bind] attribute</span></span>

<span data-ttu-id="53f65-548">Può essere applicato a una classe o a un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="53f65-548">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="53f65-549">Specifica quali proprietà di un modello devono essere incluse nell'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="53f65-549">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="53f65-550">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato qualsiasi gestore o metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="53f65-550">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="53f65-551">Nell'esempio seguente vengono associate solo le proprietà specificate del modello `Instructor` quando viene chiamato il metodo `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="53f65-551">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="53f65-552">L'attributo `[Bind]` può essere usato per evitare l'overposting negli scenari di *creazione*.</span><span class="sxs-lookup"><span data-stu-id="53f65-552">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="53f65-553">Non funziona bene negli scenari di modifica perché le proprietà escluse vengono impostate su Null o su un valore predefinito anziché rimanere inalterate.</span><span class="sxs-lookup"><span data-stu-id="53f65-553">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="53f65-554">Per difendersi dall'overposting, sono consigliati i modelli di visualizzazione anziché l'attributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="53f65-554">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="53f65-555">Per altre informazioni, vedere [Nota sulla sicurezza relativa all'overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="53f65-555">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="53f65-556">Raccolte</span><span class="sxs-lookup"><span data-stu-id="53f65-556">Collections</span></span>

<span data-ttu-id="53f65-557">Per le destinazioni che sono raccolte di tipi semplici, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="53f65-557">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="53f65-558">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-558">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="53f65-559">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-559">For example:</span></span>

* <span data-ttu-id="53f65-560">Si supponga che il parametro da associare sia una matrice denominata `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-560">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="53f65-561">I dati di modulo o di stringhe di query possono essere in uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-561">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="53f65-562">Il formato seguente è supportato solo nei dati di modulo:</span><span class="sxs-lookup"><span data-stu-id="53f65-562">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="53f65-563">Per tutti i formati di esempio precedenti, l'associazione di modelli passa una matrice di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-563">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="53f65-564">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="53f65-564">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="53f65-565">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="53f65-565">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="53f65-566">Per i formati di dati che usano numeri con indice (... [0]... [1]...) è necessario assicurarsi che la numerazione sia progressiva a partire da zero.</span><span class="sxs-lookup"><span data-stu-id="53f65-566">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="53f65-567">Se sono presenti eventuali interruzioni nella numerazione con indice, tutti gli elementi dopo l'interruzione vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="53f65-567">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="53f65-568">Ad esempio, se gli indici sono 0 e 2 anziché 0 e 1, il secondo elemento viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="53f65-568">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="53f65-569">Dizionari</span><span class="sxs-lookup"><span data-stu-id="53f65-569">Dictionaries</span></span>

<span data-ttu-id="53f65-570">Per le destinazioni `Dictionary`, l'associazione di modelli cerca le corrispondenze per *nome_parametro* oppure *nome_proprietà*.</span><span class="sxs-lookup"><span data-stu-id="53f65-570">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="53f65-571">Se non viene trovata alcuna corrispondenza, viene cercato uno dei formati supportati senza il prefisso.</span><span class="sxs-lookup"><span data-stu-id="53f65-571">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="53f65-572">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-572">For example:</span></span>

* <span data-ttu-id="53f65-573">Si supponga che il parametro di destinazione sia un elemento `Dictionary<int, string>` denominato `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-573">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="53f65-574">I dati di modulo o di stringhe di query inviati possono essere simili a uno degli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="53f65-574">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="53f65-575">Per tutti i formati di esempio precedenti, l'associazione di modelli passa un dizionario di due elementi al parametro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="53f65-575">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="53f65-576">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="53f65-576">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="53f65-577">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="53f65-577">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="53f65-578">Comportamento di globalizzazione dell'associazione di modelli dati della route e stringhe di query</span><span class="sxs-lookup"><span data-stu-id="53f65-578">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="53f65-579">Il provider del valore della route ASP.NET Core e il provider del valore della stringa di query:</span><span class="sxs-lookup"><span data-stu-id="53f65-579">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="53f65-580">Considera i valori come impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="53f65-580">Treat values as invariant culture.</span></span>
* <span data-ttu-id="53f65-581">Si prevede che gli URL siano impostazioni cultura invarianti.</span><span class="sxs-lookup"><span data-stu-id="53f65-581">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="53f65-582">Al contrario, i valori provenienti dai dati del form subiscono una conversione con distinzione delle impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="53f65-582">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="53f65-583">Questa operazione è progettata in modo che gli URL siano condivisibili tra le impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="53f65-583">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="53f65-584">Per rendere il provider del valore di route ASP.NET Core e il provider di valori della stringa di query sottoposti a una conversione dipendente dalle impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="53f65-584">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="53f65-585">Ereditano da <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="53f65-585">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="53f65-586">Copiare il codice da [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="53f65-586">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="53f65-587">Sostituire il [valore delle impostazioni cultura](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passato al costruttore del provider di valori con [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="53f65-587">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="53f65-588">Sostituire la factory del provider di valori predefinito in opzioni MVC con la nuova:</span><span class="sxs-lookup"><span data-stu-id="53f65-588">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="53f65-589">Tipi di dati speciali</span><span class="sxs-lookup"><span data-stu-id="53f65-589">Special data types</span></span>

<span data-ttu-id="53f65-590">Esistono alcuni tipi di dati speciali che l'associazione di modelli può gestire.</span><span class="sxs-lookup"><span data-stu-id="53f65-590">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="53f65-591">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="53f65-591">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="53f65-592">Un file caricato incluso nella richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="53f65-592">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="53f65-593">È anche supportato `IEnumerable<IFormFile>` per più file.</span><span class="sxs-lookup"><span data-stu-id="53f65-593">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="53f65-594">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="53f65-594">CancellationToken</span></span>

<span data-ttu-id="53f65-595">usato per annullare l'attività nei controller asincroni.</span><span class="sxs-lookup"><span data-stu-id="53f65-595">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="53f65-596">FormCollection</span><span class="sxs-lookup"><span data-stu-id="53f65-596">FormCollection</span></span>

<span data-ttu-id="53f65-597">Usato per recuperare tutti i valori dai dati di modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="53f65-597">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="53f65-598">Formattatori di input</span><span class="sxs-lookup"><span data-stu-id="53f65-598">Input formatters</span></span>

<span data-ttu-id="53f65-599">I dati nel corpo della richiesta possono essere in formato JSON, XML o in altri formati.</span><span class="sxs-lookup"><span data-stu-id="53f65-599">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="53f65-600">Per analizzare questi dati, l'associazione di modelli usa un *formattatore di input* configurato in modo da gestire un particolare tipo di contenuto.</span><span class="sxs-lookup"><span data-stu-id="53f65-600">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="53f65-601">Per impostazione predefinita, ASP.NET Core include i formattatori di input basati su JSON per la gestione dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="53f65-601">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="53f65-602">È possibile aggiungere altri formattatori per altri tipi di contenuto.</span><span class="sxs-lookup"><span data-stu-id="53f65-602">You can add other formatters for other content types.</span></span>

<span data-ttu-id="53f65-603">ASP.NET Core consente di selezionare i formattatori di input in base all'attributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="53f65-603">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="53f65-604">Se non è presente alcun attributo, viene usata l'[intestazione Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="53f65-604">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="53f65-605">Per usare i formattatori di input XML predefiniti:</span><span class="sxs-lookup"><span data-stu-id="53f65-605">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="53f65-606">Installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="53f65-606">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="53f65-607">In `Startup.ConfigureServices` chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="53f65-607">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="53f65-608">Applicare l'attributo `Consumes` alle classi controller o ai metodi di azione che devono aspettarsi XML nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="53f65-608">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="53f65-609">Per altre informazioni, vedere [Introduzione alla serializzazione XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="53f65-609">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="53f65-610">Escludere i tipi specificati dall'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="53f65-610">Exclude specified types from model binding</span></span>

<span data-ttu-id="53f65-611">Il comportamento dell'associazione di modelli e del sistema di convalida è determinato da [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="53f65-611">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="53f65-612">È possibile personalizzare `ModelMetadata` mediante l'aggiunta di un provider di dettagli a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="53f65-612">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="53f65-613">Sono disponibili provider di dettagli predefiniti per la disabilitazione dell'associazione di modelli o la convalida per i tipi specificati.</span><span class="sxs-lookup"><span data-stu-id="53f65-613">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="53f65-614">Per disabilitare l'associazione di modelli per tutti i modelli di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-614">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="53f65-615">Ad esempio, per disabilitare l'associazione di modelli per tutti i modelli di tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="53f65-615">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="53f65-616">Per disabilitare la convalida per le proprietà di un tipo specificato, aggiungere un <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53f65-616">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="53f65-617">Ad esempio, per disabilitare la convalida per le proprietà di tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="53f65-617">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="53f65-618">Strumenti di associazione di modelli personalizzati</span><span class="sxs-lookup"><span data-stu-id="53f65-618">Custom model binders</span></span>

<span data-ttu-id="53f65-619">È possibile estendere l'associazione di modelli scrivendo uno strumento di associazione di modelli personalizzato e usando l'attributo `[ModelBinder]` per selezionarlo per una determinata destinazione.</span><span class="sxs-lookup"><span data-stu-id="53f65-619">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="53f65-620">Altre informazioni sull'[associazione di modelli personalizzata](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="53f65-620">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="53f65-621">Associazione di modelli manuale</span><span class="sxs-lookup"><span data-stu-id="53f65-621">Manual model binding</span></span>

<span data-ttu-id="53f65-622">L'associazione di modelli può essere richiamata manualmente usando il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="53f65-622">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="53f65-623">Il metodo è definito in entrambe le classi `ControllerBase` e `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="53f65-623">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="53f65-624">Gli overload del metodo consentono di specificare il prefisso e il provider di valori da usare.</span><span class="sxs-lookup"><span data-stu-id="53f65-624">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="53f65-625">Il metodo restituisce `false` se l'associazione di modelli non riesce.</span><span class="sxs-lookup"><span data-stu-id="53f65-625">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="53f65-626">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="53f65-626">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="53f65-627">Attributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="53f65-627">[FromServices] attribute</span></span>

<span data-ttu-id="53f65-628">Il nome di questo attributo segue il modello degli attributi di associazione di modelli che specificano un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="53f65-628">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="53f65-629">Non si tratta però dell'associazione di dati da un provider di valori.</span><span class="sxs-lookup"><span data-stu-id="53f65-629">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="53f65-630">Ottiene un'istanza di un tipo dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="53f65-630">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="53f65-631">Lo scopo è fornire un'alternativa all'inserimento del costruttore per quando è necessario un servizio solo se viene chiamato un metodo specifico.</span><span class="sxs-lookup"><span data-stu-id="53f65-631">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53f65-632">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="53f65-632">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
