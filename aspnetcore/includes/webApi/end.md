## <a name="implement-the-other-crud-operations"></a>Implementare le altre operazioni CRUD

Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller. Dato che si tratta di variazioni di un tema, di seguito viene mostrato il codice evidenziando le differenze principali. Compilare il progetto dopo l'aggiunta o la modifica del codice.

### <a name="create"></a>Crea

[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Questo è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html). L'attributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.

Il metodo `CreatedAtRoute` restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server. `CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

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

È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa appena creata. Richiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Aggiorna

[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` è simile a `Create` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta. Per supportare gli aggiornamenti parziali usare HTTP PATCH.

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminare

[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console Postman con risposta 204 (No Content)](../../tutorials/first-web-api/_static/pmd.png)
