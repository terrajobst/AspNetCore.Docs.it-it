## <a name="implement-the-other-crud-operations"></a>Implementare le altre operazioni CRUD

Nelle sezioni seguenti vengono aggiunti i metodi `Create`, `Update` e `Delete` al controller.

### <a name="create"></a>Crea

Aggiungere il seguente metodo `Create`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il codice precedente è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). L'attributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.

Il metodo `CreatedAtRoute`:

* Restituisce una risposta 201. HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.
* Aggiunge un'intestazione Location alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Usa la route denominata "GetTodo" per creare l'URL. La route denominata "GetTodo" è definita in `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a>Usare Postman per inviare una richiesta di creazione

![Console di Postman](../../tutorials/first-web-api/_static/pmc.png)

* Impostare il metodo HTTP su `POST`.
* Selezionare il pulsante di opzione **Body** (Corpo).
* Selezionare il pulsante di opzione **raw** (non elaborato).
* Impostare il tipo su JSON
* Nell'editor chiave-valore immettere un elemento attività, ad esempio:

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Selezionare **Send** (Invia).
* Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):

![Scheda Headers (Intestazioni) della console Postman](../../tutorials/first-web-api/_static/pmget.png)

L'URI dell'intestazione Location può essere usato per accedere al nuovo elemento.

### <a name="update"></a>Aggiorna

Aggiungere il metodo `Update` seguente:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` è simile a `Create` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta. Per supportare gli aggiornamenti parziali usare HTTP PATCH.

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminare

Aggiungere il metodo `Delete` seguente:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta `Delete` è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Test `Delete`: 

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
