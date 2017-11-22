## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5abf5-101">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="5abf5-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="5abf5-102">Nelle sezioni seguenti vengono aggiunti i metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="5abf5-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="5abf5-103">Crea</span><span class="sxs-lookup"><span data-stu-id="5abf5-103">Create</span></span>

<span data-ttu-id="5abf5-104">Aggiungere il seguente metodo `Create`.</span><span class="sxs-lookup"><span data-stu-id="5abf5-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5abf5-105">Il codice precedente è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="5abf5-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5abf5-106">L'attributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="5abf5-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5abf5-107">Il metodo `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="5abf5-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="5abf5-108">Restituisce una risposta 201.</span><span class="sxs-lookup"><span data-stu-id="5abf5-108">Returns a 201 response.</span></span> <span data-ttu-id="5abf5-109">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="5abf5-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="5abf5-110">Aggiunge un'intestazione Location alla risposta.</span><span class="sxs-lookup"><span data-stu-id="5abf5-110">Adds a Location header to the response.</span></span> <span data-ttu-id="5abf5-111">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="5abf5-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5abf5-112">Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5abf5-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="5abf5-113">Usa la route denominata "GetTodo" per creare l'URL.</span><span class="sxs-lookup"><span data-stu-id="5abf5-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="5abf5-114">La route denominata "GetTodo" è definita in `GetById`:</span><span class="sxs-lookup"><span data-stu-id="5abf5-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5abf5-115">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="5abf5-115">Use Postman to send a Create request</span></span>

![Console di Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="5abf5-117">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="5abf5-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="5abf5-118">Selezionare il pulsante di opzione **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="5abf5-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="5abf5-119">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="5abf5-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="5abf5-120">Impostare il tipo su JSON</span><span class="sxs-lookup"><span data-stu-id="5abf5-120">Set the type to JSON</span></span>
* <span data-ttu-id="5abf5-121">Nell'editor chiave-valore immettere un elemento attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5abf5-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="5abf5-122">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="5abf5-122">Select **Send**</span></span>
* <span data-ttu-id="5abf5-123">Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="5abf5-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Scheda Headers (Intestazioni) della console Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="5abf5-125">L'URI dell'intestazione Location può essere usato per accedere al nuovo elemento.</span><span class="sxs-lookup"><span data-stu-id="5abf5-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="5abf5-126">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="5abf5-126">Update</span></span>

<span data-ttu-id="5abf5-127">Aggiungere il metodo `Update` seguente:</span><span class="sxs-lookup"><span data-stu-id="5abf5-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="5abf5-128">`Update` è simile a `Create` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5abf5-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="5abf5-129">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5abf5-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5abf5-130">In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="5abf5-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5abf5-131">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="5abf5-131">To support partial updates, use HTTP PATCH.</span></span>

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5abf5-133">Eliminare</span><span class="sxs-lookup"><span data-stu-id="5abf5-133">Delete</span></span>

<span data-ttu-id="5abf5-134">Aggiungere il metodo `Delete` seguente:</span><span class="sxs-lookup"><span data-stu-id="5abf5-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5abf5-135">La risposta `Delete` è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5abf5-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="5abf5-136">Test `Delete`:</span><span class="sxs-lookup"><span data-stu-id="5abf5-136">Test `Delete`:</span></span> 

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
