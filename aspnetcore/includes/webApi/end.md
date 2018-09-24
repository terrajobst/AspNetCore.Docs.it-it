## <a name="implement-the-other-crud-operations"></a>Implementare le altre operazioni CRUD

Nelle sezioni seguenti vengono aggiunti i metodi `Create`, `Update` e `Delete` al controller.

### <a name="create"></a>Crea

Aggiungere il metodo `Create` seguente:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). L'attributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.

::: moniker-end

Il metodo `CreatedAtRoute`:

* Restituisce una risposta 201. HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.
* Aggiunge un'intestazione Location alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Usa la route denominata "GetTodo" per creare l'URL. La route denominata "GetTodo" è definita in `GetById`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Usare Postman per inviare una richiesta di creazione

* Avvia l'app.
* Aprire Postman.

![Console di Postman](../../tutorials/first-web-api/_static/pmc.png)

* Aggiornare il numero di porta nell'URL localhost.
* Impostare il metodo HTTP su *POST*.
* Fare clic sulla scheda **Body** (Corpo).
* Selezionare il pulsante di opzione **raw** (non elaborato).
* Impostare il tipo su *JSON (application/json)*.
* Immettere un corpo della richiesta con un elemento attività simile al codice JSON seguente:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Fare clic sul pulsante **Invia**.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Se dopo aver fatto clic su **Invia** non viene visualizzata nessuna risposta, disattivare l'opzione **SSL certification verification** (Verifica certificazione SSL). L'opzione è disponibile in **File** > **Impostazioni**. Fare di nuovo clic sul pulsante **Invia** dopo aver disabilitato l'impostazione.

::: moniker-end

Fare clic sulla scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta) e copiare l'intestazione **Location** (Posizione):

![Scheda Headers (Intestazioni) della console Postman](../../tutorials/first-web-api/_static/pmc2.png)

L'URI dell'intestazione Location può essere usato per accedere al nuovo elemento.

### <a name="update"></a>Aggiorna

Aggiungere il metodo `Update` seguente:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` è simile a `Create` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta. Per supportare gli aggiornamenti parziali usare HTTP PATCH.

Usare Postman per aggiornare il nome dell'elemento attività in "walk cat":

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminare

Aggiungere il metodo `Delete` seguente:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta `Delete` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Usare Postman per eliminare l'elemento attività :

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
