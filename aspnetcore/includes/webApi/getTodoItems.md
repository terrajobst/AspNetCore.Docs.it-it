[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e0275-101">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e0275-101">The preceding code:</span></span>

* <span data-ttu-id="e0275-102">Definisce una classe controller vuota.</span><span class="sxs-lookup"><span data-stu-id="e0275-102">Defines an empty controller class.</span></span> <span data-ttu-id="e0275-103">Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="e0275-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="e0275-104">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext `) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e0275-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="e0275-105">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e0275-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e0275-106">Se non esiste, il costruttore aggiunge un elemento al database in memoria.</span><span class="sxs-lookup"><span data-stu-id="e0275-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="e0275-107">Recupero degli elementi attività</span><span class="sxs-lookup"><span data-stu-id="e0275-107">Getting to-do items</span></span>

<span data-ttu-id="e0275-108">Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e0275-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e0275-109">Questi metodi implementano i due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="e0275-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e0275-110">Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="e0275-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="e0275-111">Più avanti nell'esercitazione verrà illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="e0275-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="e0275-112">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="e0275-112">Routing and URL paths</span></span>

<span data-ttu-id="e0275-113">L'attributo `[HttpGet]` specifica un metodo HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e0275-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="e0275-114">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e0275-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e0275-115">Prendere la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="e0275-115">Take the template string in the controller’s `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e0275-116">Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="e0275-116">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e0275-117">In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo".</span><span class="sxs-lookup"><span data-stu-id="e0275-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="e0275-118">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e0275-118">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="e0275-119">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="e0275-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="e0275-120">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="e0275-120">This sample doesn't use a template.</span></span> <span data-ttu-id="e0275-121">Per altre informazioni, vedere [Routing degli attributi con attributi Http [verbo]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e0275-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="e0275-122">Nel metodo `GetById`:</span><span class="sxs-lookup"><span data-stu-id="e0275-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="e0275-123">`"{id}"` è una variabile segnaposto per l'ID dell'elemento `todo`.</span><span class="sxs-lookup"><span data-stu-id="e0275-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="e0275-124">Quando `GetById` viene richiamato, assegna il valore di "{id}" nell'URL al parametro `id` del metodo.</span><span class="sxs-lookup"><span data-stu-id="e0275-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="e0275-125">`Name = "GetTodo"` crea una route denominata.</span><span class="sxs-lookup"><span data-stu-id="e0275-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="e0275-126">Le route denominate:</span><span class="sxs-lookup"><span data-stu-id="e0275-126">Named routes:</span></span>

* <span data-ttu-id="e0275-127">Consentono all'app di creare un collegamento HTTP usando il nome della route.</span><span class="sxs-lookup"><span data-stu-id="e0275-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="e0275-128">Sono spiegate più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e0275-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="e0275-129">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="e0275-129">Return values</span></span>

<span data-ttu-id="e0275-130">Il metodo `GetAll` restituisce un tipo `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="e0275-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="e0275-131">MVC serializza automaticamente l'oggetto su [JSON](http://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="e0275-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e0275-132">Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="e0275-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e0275-133">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="e0275-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="e0275-134">Al contrario, il metodo `GetById` restituisce il tipo `IActionResult` più generale, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="e0275-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="e0275-135">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="e0275-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e0275-136">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="e0275-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e0275-137">La restituzione di `NotFound` restituisce una risposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e0275-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="e0275-138">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="e0275-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e0275-139">La restituzione di `ObjectResult` restituisce una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e0275-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
