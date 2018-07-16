::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="aab52-101">Il codice precedente definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="aab52-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="aab52-102">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="aab52-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="aab52-103">Il codice precedente definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="aab52-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="aab52-104">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="aab52-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="aab52-105">La classe è annotata con un attributo `[ApiController]` per abilitare alcune funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="aab52-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="aab52-106">Per informazioni sulle funzionalità abilitate dall'attributo, vedere [Annotare una classe con l'attributo ApiController](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="aab52-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="aab52-107">Il costruttore del controller usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="aab52-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="aab52-108">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="aab52-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="aab52-109">Se non esiste, il costruttore aggiunge un elemento al database in memoria.</span><span class="sxs-lookup"><span data-stu-id="aab52-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="aab52-110">Ottenere elementi attività</span><span class="sxs-lookup"><span data-stu-id="aab52-110">Get to-do items</span></span>

<span data-ttu-id="aab52-111">Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="aab52-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="aab52-112">Questi metodi implementano i due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="aab52-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="aab52-113">Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="aab52-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="aab52-114">Più avanti nell'esercitazione viene illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="aab52-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="aab52-115">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="aab52-115">Routing and URL paths</span></span>

<span data-ttu-id="aab52-116">L'attributo `[HttpGet]` indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="aab52-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="aab52-117">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="aab52-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="aab52-118">Prendere la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="aab52-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="aab52-119">Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="aab52-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="aab52-120">In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo".</span><span class="sxs-lookup"><span data-stu-id="aab52-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="aab52-121">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="aab52-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="aab52-122">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="aab52-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="aab52-123">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="aab52-123">This sample doesn't use a template.</span></span> <span data-ttu-id="aab52-124">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="aab52-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="aab52-125">Nel metodo `GetById` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="aab52-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="aab52-126">Quando viene chiamato, `GetById` assegna il valore di `"{id}"` nell'URL al parametro `id` del metodo.</span><span class="sxs-lookup"><span data-stu-id="aab52-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="aab52-127">`Name = "GetTodo"` crea una route denominata.</span><span class="sxs-lookup"><span data-stu-id="aab52-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="aab52-128">Le route denominate:</span><span class="sxs-lookup"><span data-stu-id="aab52-128">Named routes:</span></span>

* <span data-ttu-id="aab52-129">Consentono all'app di creare un collegamento HTTP usando il nome della route.</span><span class="sxs-lookup"><span data-stu-id="aab52-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="aab52-130">Sono spiegate più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="aab52-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="aab52-131">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="aab52-131">Return values</span></span>

<span data-ttu-id="aab52-132">Il metodo `GetAll` restituisce una raccolta di oggetti `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="aab52-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="aab52-133">MVC serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="aab52-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="aab52-134">Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="aab52-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="aab52-135">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="aab52-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="aab52-136">Per contro il metodo `GetById` restituisce il [tipo IActionResult](xref:web-api/action-return-types#iactionresult-type), più generico, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="aab52-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="aab52-137">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="aab52-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="aab52-138">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="aab52-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="aab52-139">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="aab52-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="aab52-140">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="aab52-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="aab52-141">La restituzione di [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="aab52-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="aab52-142">Per contro il metodo `GetById` restituisce il [tipo ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) più generale, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="aab52-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="aab52-143">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="aab52-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="aab52-144">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="aab52-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="aab52-145">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="aab52-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="aab52-146">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="aab52-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="aab52-147">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="aab52-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
