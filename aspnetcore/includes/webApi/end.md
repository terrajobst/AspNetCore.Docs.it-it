## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f053b-101">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="f053b-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="f053b-102">Nelle sezioni seguenti vengono aggiunti i metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="f053b-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="f053b-103">Crea</span><span class="sxs-lookup"><span data-stu-id="f053b-103">Create</span></span>

<span data-ttu-id="f053b-104">Aggiungere il metodo `Create` seguente:</span><span class="sxs-lookup"><span data-stu-id="f053b-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f053b-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="f053b-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="f053b-106">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="f053b-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f053b-107">L'attributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f053b-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f053b-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="f053b-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="f053b-109">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="f053b-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f053b-110">MVC ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f053b-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="f053b-111">Il metodo `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="f053b-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="f053b-112">Restituisce una risposta 201.</span><span class="sxs-lookup"><span data-stu-id="f053b-112">Returns a 201 response.</span></span> <span data-ttu-id="f053b-113">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="f053b-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f053b-114">Aggiunge un'intestazione Location alla risposta.</span><span class="sxs-lookup"><span data-stu-id="f053b-114">Adds a Location header to the response.</span></span> <span data-ttu-id="f053b-115">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="f053b-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f053b-116">Vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f053b-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f053b-117">Usa la route denominata "GetTodo" per creare l'URL.</span><span class="sxs-lookup"><span data-stu-id="f053b-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="f053b-118">La route denominata "GetTodo" è definita in `GetById`:</span><span class="sxs-lookup"><span data-stu-id="f053b-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f053b-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="f053b-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f053b-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="f053b-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f053b-121">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="f053b-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f053b-122">Avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="f053b-122">Start the app.</span></span>
* <span data-ttu-id="f053b-123">Aprire Postman.</span><span class="sxs-lookup"><span data-stu-id="f053b-123">Open Postman.</span></span>

![Console di Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="f053b-125">Aggiornare il numero di porta nell'URL localhost.</span><span class="sxs-lookup"><span data-stu-id="f053b-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="f053b-126">Impostare il metodo HTTP su *POST*.</span><span class="sxs-lookup"><span data-stu-id="f053b-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="f053b-127">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="f053b-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="f053b-128">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="f053b-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f053b-129">Impostare il tipo su *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="f053b-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="f053b-130">Immettere un corpo della richiesta con un elemento attività simile al codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="f053b-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="f053b-131">Fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f053b-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="f053b-132">Se dopo aver fatto clic su **Invia** non viene visualizzata nessuna risposta, disattivare l'opzione **SSL certification verification** (Verifica certificazione SSL).</span><span class="sxs-lookup"><span data-stu-id="f053b-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="f053b-133">L'opzione è disponibile in **File** > **Impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="f053b-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="f053b-134">Fare di nuovo clic sul pulsante **Invia** dopo aver disabilitato l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="f053b-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="f053b-135">Fare clic sulla scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta) e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="f053b-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Scheda Headers (Intestazioni) della console Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="f053b-137">L'URI dell'intestazione Location può essere usato per accedere al nuovo elemento.</span><span class="sxs-lookup"><span data-stu-id="f053b-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="f053b-138">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="f053b-138">Update</span></span>

<span data-ttu-id="f053b-139">Aggiungere il metodo `Update` seguente:</span><span class="sxs-lookup"><span data-stu-id="f053b-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f053b-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="f053b-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f053b-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="f053b-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="f053b-142">`Update` è simile a `Create` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f053b-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f053b-143">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f053b-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f053b-144">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="f053b-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f053b-145">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f053b-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="f053b-146">Usare Postman per aggiornare il nome dell'elemento attività in "walk cat":</span><span class="sxs-lookup"><span data-stu-id="f053b-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f053b-148">Eliminare</span><span class="sxs-lookup"><span data-stu-id="f053b-148">Delete</span></span>

<span data-ttu-id="f053b-149">Aggiungere il metodo `Delete` seguente:</span><span class="sxs-lookup"><span data-stu-id="f053b-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f053b-150">La risposta `Delete` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f053b-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="f053b-151">Usare Postman per eliminare l'elemento attività :</span><span class="sxs-lookup"><span data-stu-id="f053b-151">Use Postman to delete the to-do item:</span></span>

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
