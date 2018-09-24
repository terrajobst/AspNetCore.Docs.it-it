::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Il codice precedente definisce una classe controller API senza metodi. Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Il codice precedente:

* Definisce una classe controller API senza metodi.
* Crea un nuovo TodoItem quando `TodoItems` è vuoto. Non sarà possibile eliminare tutti i TodoItem perché il costruttore ne crea uno nuovo se `TodoItems` è vuoto.

Nelle sezioni successive vengono aggiunti i metodi per implementare l'API. La classe è annotata con un attributo `[ApiController]` per abilitare alcune funzionalità utili. Per informazioni sulle funzionalità abilitate dall'attributo, vedere [Annotare una classe con l'attributo ApiController](xref:web-api/index#annotate-class-with-apicontrollerattribute).

::: moniker-end

Il costruttore del controller usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext`) nel controller. Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller. Se non esiste, il costruttore aggiunge un elemento al database in memoria.

## <a name="get-to-do-items"></a>Ottenere elementi attività

Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

Questi metodi implementano i due metodi GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Più avanti nell'esercitazione viene illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://curl.haxx.se/docs/manpage.html).

### <a name="routing-and-url-paths"></a>Routing e percorsi di URL

L'attributo `[HttpGet]` indica un metodo che risponde a una richiesta HTTP GET. Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:

* Prendere la stringa di modello nell'attributo `Route` del controller:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller". In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo". Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.
* Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso. In questo esempio non si usa un modello. Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Nel metodo `GetById` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività. Quando viene chiamato, `GetById` assegna il valore di `"{id}"` nell'URL al parametro `id` del metodo.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

`Name = "GetTodo"` crea una route denominata. Le route denominate:

* Consentono all'app di creare un collegamento HTTP usando il nome della route.
* Sono spiegate più avanti nell'esercitazione.

### <a name="return-values"></a>Valori restituiti

Il metodo `GetAll` restituisce una raccolta di oggetti `TodoItem`. MVC serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta. Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite. Le eccezioni non gestite vengono convertite in errori 5xx.

::: moniker range="<= aspnetcore-2.0"

Per contro il metodo `GetById` restituisce il [tipo IActionResult](xref:web-api/action-return-types#iactionresult-type), più generico, che rappresenta un'ampia gamma di tipi restituiti. `GetById` presenta due tipi restituiti diversi:

* Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404. La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.
* In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON. La restituzione di [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) risulta in una risposta HTTP 200.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Per contro il metodo `GetById` restituisce il [tipo ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) più generale, che rappresenta un'ampia gamma di tipi restituiti. `GetById` presenta due tipi restituiti diversi:

* Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404. La restituzione di [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) restituisce una risposta HTTP 404.
* In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON. La restituzione di `item` risulta in una risposta HTTP 200.

::: moniker-end
