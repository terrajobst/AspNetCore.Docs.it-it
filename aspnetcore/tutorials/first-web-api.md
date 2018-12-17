---
title: "Esercitazione: Creare un'API Web con ASP.NET Core MVC"
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC
ms.author: riande
monikerRange: '> aspnetcore-2.1'
ms.custom: mvc
ms.date: 11/19/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 1af14b85cbaefc00fd97db7c721c4f9436a65fb2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121466"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a>Esercitazione: Creare un'API Web con ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Creare un progetto di API Web.
> * Aggiungere una classe modello.
> * Creare il contesto del database.
> * Registrare il contesto del database.
> * Aggiungere un controller.
> * Aggiungere metodi CRUD.
> * Configurare routing e percorsi URL.
> * Specificare valori restituiti.
> * Chiamare l'API Web con Postman.
> * Chiamare l'API Web con jQuery.

Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.

## <a name="overview"></a>Panoramica

Questa esercitazione consente di creare l'API seguente:

|API | Description | Corpo della richiesta | Corpo della risposta |
|--- | ---- | ---- | ---- |
|GET /api/todo | Ottiene tutti gli elementi attività | nessuno | Matrice di elementi attività|
|GET /api/todo/{id} | Ottiene un elemento in base all'ID | nessuno | Elemento attività|
|POST /api/todo | Aggiunge un nuovo elemento | Elemento attività | Elemento attività |
|PUT /api/todo/{id} | Aggiorna un elemento esistente &nbsp; | Elemento attività | nessuno |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Elimina un elemento &nbsp; &nbsp; | nessuno | nessuno|

Il diagramma seguente visualizza la struttura dell'app.

![Il client è rappresentato da una casella a sinistra e invia una richiesta e riceve una risposta dall'applicazione, una casella a destra. Nella riquadro dell'applicazione tre caselle rappresentano il controller, il modello e il livello di accesso ai dati. La richiesta viene ricevuta dal controller dell'applicazione e vengono eseguite operazioni di lettura/scrittura tra il controller e il livello di accesso ai dati. Il modello viene serializzato e restituito al client nella risposta.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Creare un progetto Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Scegliere **Nuovo** > **Progetto** dal menu **File**.
* Selezionare il modello **Applicazione Web ASP.NET Core**. Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.
* Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core. Selezionare il modello **API**, quindi scegliere **OK**. **Non** selezionare **Abilita supporto Docker**.

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.
* Eseguire i comandi seguenti:

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.

* Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Selezionare **File** > **Nuova soluzione**.

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* Selezionare **.NET Core App** > **ASP.NET Core Web API** > **Next**.

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero **.NET Core 2.2*.

* Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>Testare l'API

Il modello di progetto crea un'API `values`. Chiamare il metodo `Get` da un browser per eseguire il test dell'app.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Premere CTRL+F5 per eseguire l'app. Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.

Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**. Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Premere CTRL+F5 per eseguire l'app. In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app. Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso. Viene restituito un errore HTTP 404 (Non trovato). Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).

---

Viene restituito il codice JSON seguente:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un *modello* è un set di classi che rappresentano i dati gestiti dall'app. Il modello per questa app è una classe `TodoItem` singola.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

* Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.

* Sostituire il codice del modello con il codice seguente:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aggiungere una cartella denominata *Modelli*.

* Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.

* Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.

* Sostituire il codice del modello con il codice seguente:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

La proprietà `Id` funziona come chiave univoca in un database relazionale.

È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

## <a name="add-a-database-context"></a>Aggiungere un contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aggiungere una classe `TodoContext` alla cartella *Models*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Aggiungere una classe `TodoContext` alla cartella *Models*:

---

* Sostituire il codice del modello con il codice seguente:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Registrare il contesto del database

In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection). Il contenitore rende disponibile il servizio ai controller.

Aggiornare *Startup.cs* con il codice evidenziato seguente:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Il codice precedente:

* Rimuove le dichiarazioni `using` inutilizzate.
* Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.
* Specifica che il contesto del database userà un database in memoria.

## <a name="add-a-controller"></a>Aggiungere un controller

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.
* Selezionare **Aggiungi** > **Nuovo elemento**.
* Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).
* Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Nella cartella *Controllers* creare una classe denominata `TodoController`.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Nella cartella *Controllers* aggiungere la classe `TodoController`.

---

* Sostituire il codice del modello con il codice seguente:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Il codice precedente:

* Definisce una classe controller API senza metodi.
* Contrassegnare la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute). L'attributo indica che il controller risponde alle richieste di API Web. Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute) (Annotazione con l'attributo ApiController).
* Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller. Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.
* Aggiunge un elemento denominato `Item1` al database se il database è vuoto. Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP. Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API. Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.

## <a name="add-get-methods"></a>Aggiungere metodi Get

Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Questi metodi implementano due metodi GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Testare l'app chiamando i due endpoint da un browser. Ad esempio:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

La chiamata a `GetTodoItems` genera la risposta HTTP seguente:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>Routing e percorsi di URL

L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET. Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:

* Iniziare con la stringa di modello nell'attributo `Route` del controller:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller". In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo". Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.
* Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso. In questo esempio non si usa un modello. Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività. Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

Il parametro `Name = "GetTodo"` crea una route denominata. In seguito si vedrà come l'app può usare il nome per creare un collegamento HTTP usando il nome della route.

## <a name="return-values"></a>Valori restituiti

Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta. Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite. Le eccezioni non gestite vengono convertite in errori 5xx.

I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP. Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:

* Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).
* In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON. La restituzione di `item` risulta in una risposta HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Testare il metodo GetTodoItems

Questa esercitazione usa Postman per testare l'API Web.

* Installare [Postman](https://www.getpostman.com/apps)
* Avviare l'app Web.
* Avviare Postman.
* Disattivare **SSL certificate verification** (Verifica certificato SSL)
  
  * From  **File > Settings** (File > Impostazioni) (scheda **General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).
    > [!WARNING]
    > Riattivare la verifica dei certificati SSL al termine del test del controller.

* Creare una nuova richiesta.
  * Impostare il metodo HTTP su **GET**.
  * Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`. Ad esempio `https://localhost:5001/api/todo`.
* Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.
* Selezionare **Send** (Invia).

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Aggiungere un metodo Create

Aggiungere il metodo `PostTodoItem` seguente:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.

Il metodo `CreatedAtRoute`:

* Restituisce una risposta 201. HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.
* Aggiunge un'intestazione Location alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Usa la route denominata "GetTodo" per creare l'URL. La route denominata "GetTodo" è definita in `GetTodoItem`:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Testare il metodo PostTodoItem

* Compilare il progetto.
* In Postman impostare il metodo HTTP su `POST`.
* Fare clic sulla scheda **Body** (Corpo).
* Selezionare il pulsante di opzione **raw** (non elaborato).
* Impostare il tipo su **JSON (application/json)**.
* Nel corpo della richiesta immettere codice JSON per un elemento attività:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Selezionare **Send** (Invia).

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  Se viene visualizzato un errore HTTP 405 - metodo non concesso, la causa probabile è che il progetto non è stato compilato dopo l'aggiunta del metodo `PostTodoItem`.

### <a name="test-the-location-header-uri"></a>Testare l'URI dell'intestazione della posizione

* Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).
* Copiare il valore dell'intestazione **Location** (Posizione):

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* Impostare il metodo su GET.
* Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)
* Selezionare **Send** (Invia).

## <a name="add-a-puttodoitem-method"></a>Aggiungere un metodo PutTodoItem

Aggiungere il metodo `PutTodoItem` seguente:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche. Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).

### <a name="test-the-puttodoitem-method"></a>Testare il metodo PutTodoItem

Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

L'immagine seguente visualizza l'aggiornamento di Postman:

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>Aggiungere un metodo DeleteTodoItem

Aggiungere il metodo `DeleteTodoItem` seguente:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Testare il metodo DeleteTodoItem

Usare Postman per eliminare un elemento attività:

* Impostare il metodo su `DELETE`.
* Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`
* Selezionare **Send** (Invia).

L'app di esempio consente di eliminare tutti gli elementi, ma quando viene eliminato l'ultimo elemento, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.

## <a name="call-the-api-with-jquery"></a>Chiamare l'API con jQuery

In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web. jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.

Configurare l'app in modo che [gestisca i file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [consenta il mapping di file predefinito](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
Creare una cartella *wwwroot* nella directory del progetto.
::: moniker-end

Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*. Sostituire il contenuto con il markup seguente:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*. Sostituire il contenuto con il codice seguente:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:

* Aprire *Properties\launchSettings.json*.
* Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.

È possibile ottenere jQuery in vari modi. Nel frammento precedente la libreria viene caricata da una rete CDN.

In questo esempio vengono chiamati tutti i metodi CRUD dell'API. Di seguito sono incluse le spiegazioni delle chiamate all'API.

### <a name="get-a-list-of-to-do-items"></a>Ottenere un elenco di elementi attività

La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività. La funzione di callback `success` viene chiamata se la richiesta ha esito positivo. Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Aggiungere un elemento attività

La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta. Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato. L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aggiornare un elemento attività

L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo. L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminare un elemento attività

È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).

Per altre informazioni, vedere le seguenti risorse:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un progetto di API Web.
> * Aggiungere una classe modello.
> * Creare il contesto del database.
> * Registrare il contesto del database.
> * Aggiungere un controller.
> * Aggiungere metodi CRUD.
> * Configurare routing e percorsi URL.
> * Specificare valori restituiti.
> * Chiamare l'API Web con Postman.
> * Chiamare l'API Web con jQuery.

Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
