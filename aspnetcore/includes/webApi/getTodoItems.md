::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="95391-101">Il codice precedente definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="95391-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="95391-102">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="95391-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="95391-103">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="95391-103">The preceding code:</span></span>

* <span data-ttu-id="95391-104">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="95391-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="95391-105">Crea un nuovo TodoItem quando `TodoItems` è vuoto.</span><span class="sxs-lookup"><span data-stu-id="95391-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="95391-106">Non sarà possibile eliminare tutti i TodoItem perché il costruttore ne crea uno nuovo se `TodoItems` è vuoto.</span><span class="sxs-lookup"><span data-stu-id="95391-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="95391-107">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="95391-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="95391-108">La classe è annotata con un attributo `[ApiController]` per abilitare alcune funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="95391-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="95391-109">Per informazioni sulle funzionalità abilitate dall'attributo, vedere [Annotare una classe con l'attributo ApiController](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="95391-109">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="95391-110">Il costruttore del controller usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="95391-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="95391-111">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="95391-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="95391-112">Se non esiste, il costruttore aggiunge un elemento al database in memoria.</span><span class="sxs-lookup"><span data-stu-id="95391-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="95391-113">Ottenere elementi attività</span><span class="sxs-lookup"><span data-stu-id="95391-113">Get to-do items</span></span>

<span data-ttu-id="95391-114">Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="95391-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="95391-115">Questi metodi implementano i due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="95391-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="95391-116">Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="95391-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="95391-117">Più avanti nell'esercitazione viene illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="95391-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="95391-118">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="95391-118">Routing and URL paths</span></span>

<span data-ttu-id="95391-119">L'attributo `[HttpGet]` indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="95391-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="95391-120">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="95391-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="95391-121">Prendere la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="95391-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="95391-122">Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="95391-122">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="95391-123">In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo".</span><span class="sxs-lookup"><span data-stu-id="95391-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="95391-124">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="95391-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="95391-125">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="95391-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="95391-126">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="95391-126">This sample doesn't use a template.</span></span> <span data-ttu-id="95391-127">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="95391-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="95391-128">Nel metodo `GetById` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="95391-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="95391-129">Quando viene chiamato, `GetById` assegna il valore di `"{id}"` nell'URL al parametro `id` del metodo.</span><span class="sxs-lookup"><span data-stu-id="95391-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="95391-130">`Name = "GetTodo"` crea una route denominata.</span><span class="sxs-lookup"><span data-stu-id="95391-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="95391-131">Le route denominate:</span><span class="sxs-lookup"><span data-stu-id="95391-131">Named routes:</span></span>

* <span data-ttu-id="95391-132">Consentono all'app di creare un collegamento HTTP usando il nome della route.</span><span class="sxs-lookup"><span data-stu-id="95391-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="95391-133">Sono spiegate più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="95391-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="95391-134">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="95391-134">Return values</span></span>

<span data-ttu-id="95391-135">Il metodo `GetAll` restituisce una raccolta di oggetti `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="95391-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="95391-136">MVC serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="95391-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="95391-137">Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="95391-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="95391-138">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="95391-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="95391-139">Per contro il metodo `GetById` restituisce il [tipo IActionResult](xref:web-api/action-return-types#iactionresult-type), più generico, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="95391-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="95391-140">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="95391-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="95391-141">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="95391-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="95391-142">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="95391-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="95391-143">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="95391-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="95391-144">La restituzione di [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="95391-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="95391-145">Per contro il metodo `GetById` restituisce il [tipo ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) più generale, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="95391-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="95391-146">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="95391-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="95391-147">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="95391-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="95391-148">La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="95391-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="95391-149">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="95391-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="95391-150">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="95391-150">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
