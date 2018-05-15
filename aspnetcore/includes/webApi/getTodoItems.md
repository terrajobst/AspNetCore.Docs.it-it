::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d588-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="3d588-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="3d588-102">Il codice precedente definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="3d588-102">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="3d588-103">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="3d588-103">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d588-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="3d588-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="3d588-105">Il codice precedente definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="3d588-105">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="3d588-106">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="3d588-106">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="3d588-107">La classe è annotata con un attributo `[ApiController]` per abilitare alcune funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="3d588-107">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="3d588-108">Per informazioni sulle funzionalità abilitate dall'attributo, vedere [Annotare una classe con l'attributo ApiController](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="3d588-108">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="3d588-109">Il costruttore del controller usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="3d588-109">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="3d588-110">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="3d588-110">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="3d588-111">Se non esiste, il costruttore aggiunge un elemento al database in memoria.</span><span class="sxs-lookup"><span data-stu-id="3d588-111">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="3d588-112">Ottenere elementi attività</span><span class="sxs-lookup"><span data-stu-id="3d588-112">Get to-do items</span></span>

<span data-ttu-id="3d588-113">Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="3d588-113">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d588-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="3d588-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d588-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="3d588-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end

<span data-ttu-id="3d588-116">Questi metodi implementano i due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="3d588-116">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="3d588-117">Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="3d588-117">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="3d588-118">Più avanti nell'esercitazione viene illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="3d588-118">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="3d588-119">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="3d588-119">Routing and URL paths</span></span>

<span data-ttu-id="3d588-120">L'attributo `[HttpGet]` indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3d588-120">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="3d588-121">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="3d588-121">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="3d588-122">Prendere la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="3d588-122">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d588-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="3d588-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d588-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="3d588-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end

* <span data-ttu-id="3d588-125">Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="3d588-125">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="3d588-126">In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo".</span><span class="sxs-lookup"><span data-stu-id="3d588-126">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="3d588-127">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="3d588-127">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="3d588-128">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="3d588-128">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="3d588-129">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="3d588-129">This sample doesn't use a template.</span></span> <span data-ttu-id="3d588-130">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="3d588-130">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="3d588-131">Nel metodo `GetById` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="3d588-131">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="3d588-132">Quando viene chiamato, `GetById` assegna il valore di `"{id}"` nell'URL al parametro `id` del metodo.</span><span class="sxs-lookup"><span data-stu-id="3d588-132">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d588-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="3d588-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d588-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="3d588-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

<span data-ttu-id="3d588-135">`Name = "GetTodo"` crea una route denominata.</span><span class="sxs-lookup"><span data-stu-id="3d588-135">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="3d588-136">Le route denominate:</span><span class="sxs-lookup"><span data-stu-id="3d588-136">Named routes:</span></span>

* <span data-ttu-id="3d588-137">Consentono all'app di creare un collegamento HTTP usando il nome della route.</span><span class="sxs-lookup"><span data-stu-id="3d588-137">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="3d588-138">Sono spiegate più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3d588-138">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="3d588-139">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="3d588-139">Return values</span></span>

<span data-ttu-id="3d588-140">Il metodo `GetAll` restituisce una raccolta di oggetti `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3d588-140">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="3d588-141">MVC serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="3d588-141">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="3d588-142">Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="3d588-142">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="3d588-143">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="3d588-143">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d588-144">Per contro il metodo `GetById` restituisce il [tipo IActionResult](xref:web-api/action-return-types#iactionresult-type), più generico, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="3d588-144">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="3d588-145">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="3d588-145">`GetById` has two different return types:</span></span>

* <span data-ttu-id="3d588-146">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="3d588-146">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="3d588-147">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3d588-147">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="3d588-148">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="3d588-148">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3d588-149">La restituzione di [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3d588-149">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d588-150">Per contro il metodo `GetById` restituisce il [tipo ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) più generale, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="3d588-150">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="3d588-151">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="3d588-151">`GetById` has two different return types:</span></span>

* <span data-ttu-id="3d588-152">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="3d588-152">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="3d588-153">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3d588-153">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="3d588-154">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="3d588-154">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3d588-155">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3d588-155">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end