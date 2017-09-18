<span data-ttu-id="f1aff-101">[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="f1aff-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="f1aff-102">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f1aff-102">The preceding code:</span></span>

* <span data-ttu-id="f1aff-103">Definisce una classe controller vuota.</span><span class="sxs-lookup"><span data-stu-id="f1aff-103">Defines an empty controller class.</span></span> <span data-ttu-id="f1aff-104">Nelle sezioni successive verranno aggiunti i metodi per implementare l'API.</span><span class="sxs-lookup"><span data-stu-id="f1aff-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="f1aff-105">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext `) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f1aff-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="f1aff-106">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f1aff-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="f1aff-107">Se non esiste, il costruttore aggiunge un elemento al database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f1aff-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="f1aff-108">Recupero degli elementi attività</span><span class="sxs-lookup"><span data-stu-id="f1aff-108">Getting to-do items</span></span>

<span data-ttu-id="f1aff-109">Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f1aff-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="f1aff-110">[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="f1aff-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="f1aff-111">Questi metodi implementano i due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="f1aff-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="f1aff-112">Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="f1aff-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="f1aff-113">Più avanti nell'esercitazione verrà illustrato come visualizzare la risposta HTTP usando [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="f1aff-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="f1aff-114">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="f1aff-114">Routing and URL paths</span></span>

<span data-ttu-id="f1aff-115">L'attributo `[HttpGet]` specifica un metodo HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f1aff-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="f1aff-116">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f1aff-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f1aff-117">Prendere la stringa di modello nell'attributo della route del controller:</span><span class="sxs-lookup"><span data-stu-id="f1aff-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="f1aff-118">[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f1aff-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="f1aff-119">Sostituire "[Controller]" con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="f1aff-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f1aff-120">In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo".</span><span class="sxs-lookup"><span data-stu-id="f1aff-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="f1aff-121">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f1aff-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="f1aff-122">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="f1aff-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="f1aff-123">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="f1aff-123">This sample doesn't use a template.</span></span> <span data-ttu-id="f1aff-124">Per altre informazioni, vedere [Routing degli attributi con attributi Http [verbo]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f1aff-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="f1aff-125">Nel metodo `GetById`:</span><span class="sxs-lookup"><span data-stu-id="f1aff-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="f1aff-126">`"{id}"` è una variabile segnaposto per l'ID dell'elemento `todo`.</span><span class="sxs-lookup"><span data-stu-id="f1aff-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="f1aff-127">Quando `GetById` viene richiamato, assegna il valore di "{id}" nell'URL al parametro `id` del metodo.</span><span class="sxs-lookup"><span data-stu-id="f1aff-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="f1aff-128">`Name = "GetTodo"` crea una route denominata e consente di eseguire un collegamento a questa route in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1aff-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="f1aff-129">Verrà fornito un esempio in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="f1aff-129">I'll explain it with an example later.</span></span> <span data-ttu-id="f1aff-130">Per informazioni dettagliate, vedere [Routing ad azioni del controller](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="f1aff-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="f1aff-131">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="f1aff-131">Return values</span></span>

<span data-ttu-id="f1aff-132">Il metodo `GetAll` restituisce un tipo `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="f1aff-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="f1aff-133">MVC serializza automaticamente l'oggetto su [JSON](http://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f1aff-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f1aff-134">Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="f1aff-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f1aff-135">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="f1aff-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="f1aff-136">Al contrario, il metodo `GetById` restituisce il tipo `IActionResult` più generale, che rappresenta un'ampia gamma di tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="f1aff-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="f1aff-137">`GetById` presenta due tipi restituiti diversi:</span><span class="sxs-lookup"><span data-stu-id="f1aff-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="f1aff-138">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404.</span><span class="sxs-lookup"><span data-stu-id="f1aff-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="f1aff-139">Questa operazione viene effettuata restituendo `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="f1aff-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="f1aff-140">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f1aff-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f1aff-141">Questa operazione viene effettuata restituendo `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="f1aff-141">This is done by returning an `ObjectResult`</span></span>
