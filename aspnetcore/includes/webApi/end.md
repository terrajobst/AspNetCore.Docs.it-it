## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="1c2bd-101">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="1c2bd-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="1c2bd-102">Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="1c2bd-103">Dato che si tratta di variazioni di un tema, di seguito viene mostrato il codice evidenziando le differenze principali.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="1c2bd-104">Compilare il progetto dopo l'aggiunta o la modifica del codice.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="1c2bd-105">Crea</span><span class="sxs-lookup"><span data-stu-id="1c2bd-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="1c2bd-106">Questo è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="1c2bd-107">L'attributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="1c2bd-108">Il metodo `CreatedAtRoute` restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="1c2bd-109">`CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="1c2bd-110">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="1c2bd-111">Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="1c2bd-112">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="1c2bd-112">Use Postman to send a Create request</span></span>

![Console di Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="1c2bd-114">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="1c2bd-115">Selezionare il pulsante di opzione **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="1c2bd-116">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="1c2bd-117">Impostare il tipo su JSON</span><span class="sxs-lookup"><span data-stu-id="1c2bd-117">Set the type to JSON</span></span>
* <span data-ttu-id="1c2bd-118">Nell'editor chiave-valore immettere un elemento attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1c2bd-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="1c2bd-119">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-119">Select **Send**</span></span>

* <span data-ttu-id="1c2bd-120">Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="1c2bd-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Scheda Headers (Intestazioni) della console Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="1c2bd-122">È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa appena creata.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="1c2bd-123">Richiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="1c2bd-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="1c2bd-124">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="1c2bd-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="1c2bd-125">`Update` è simile a `Create` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="1c2bd-126">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="1c2bd-127">In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="1c2bd-128">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="1c2bd-128">To support partial updates, use HTTP PATCH.</span></span>

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="1c2bd-130">Eliminare</span><span class="sxs-lookup"><span data-stu-id="1c2bd-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="1c2bd-131">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1c2bd-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
