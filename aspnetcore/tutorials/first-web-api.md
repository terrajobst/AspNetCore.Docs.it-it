---
title: "Esercitazione: creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541796"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Esercitazione: creare un'API Web con ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Creare un progetto di API Web.
> * Aggiungere una classe modello e un contesto di database.
> * Eseguire lo scaffolding di un controller con i metodi CRUD.
> * Configurare il routing, i percorsi URL e i valori restituiti.
> * Chiamare l'API Web con Postman.

Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.

## <a name="overview"></a>Panoramica

Questa esercitazione crea un'API Web contenente gli endpoint seguenti:

|Endpoint                  |Descrizione            |Corpo della richiesta|Corpo della risposta       |
|--------------------------|-----------------------|------------|--------------------|
|GET /api/TodoItems        |Ottiene tutti gli elementi attività    |Nessuno        |Matrice di elementi attività|
|GET /api/TodoItems/{id}   |Ottiene un elemento in base all'ID      |Nessuno        |Elemento attività          |
|POST /api/TodoItems       |Aggiunge un nuovo elemento         |Elemento attività  |Elemento attività          |
|PUT /api/TodoItems/{id}   |Aggiorna un elemento esistente|Elemento attività  |Nessuno                |
|Elimina/api/TodoItems/{id}|Eliminare un elemento         |Nessuno        |Nessuno                |

Il diagramma seguente visualizza la struttura dell'app.

![Il client è rappresentato da una casella a sinistra. Invia una richiesta e riceve una risposta dall'applicazione, una casella disegnata a destra. Nella riquadro dell'applicazione tre caselle rappresentano il controller, il modello e il livello di accesso ai dati. La richiesta viene ricevuta dal controller dell'applicazione e vengono eseguite operazioni di lettura/scrittura tra il controller e il livello di accesso ai dati. Il modello viene serializzato e restituito al client nella risposta.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Creare un progetto Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Scegliere **Nuovo** > **Progetto** dal menu **File**.
1. Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.
1. Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.
1. Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**. Selezionare il modello **API** e fare clic su **Crea**.

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
1. Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.
1. Eseguire i comandi seguenti:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.

  I comandi precedenti:

  * Creare un nuovo progetto API Web e aprirlo in Visual Studio Code.
  * Aggiungere i pacchetti NuGet necessari nella sezione successiva.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. Selezionare **File** > **Nuova soluzione**.

    ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

1. Selezionare **.NET Core** > **App** > **API** > **Avanti**.

    ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
1. Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** selezionare il **framework di destinazione** * *.NET Core 3.0*.
1. Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>Testare l'API

Il modello di progetto crea un'API `WeatherForecast`. Chiamare il metodo `Get` da un browser per eseguire il test dell'app.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Premere <kbd>CTRL + F5</kbd> per eseguire l'app. Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.

Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**. Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Premere <kbd>CTRL + F5</kbd> per eseguire l'app. In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Selezionare **Esegui** > **Avvia debug** per avviare l'app. Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso. Viene restituito un errore HTTP 404 (Non trovato). Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).

---

Viene restituito un codice JSON simile al seguente:

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a>Aggiungere una classe modello

Un *modello* è un set di classi che rappresentano i dati gestiti dall'app. Il modello per questa app è una classe `TodoItem` singola.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.
1. Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.
1. Sostituire il codice del modello con il codice seguente:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Aggiungere una cartella denominata *Modelli*.
1. Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. Fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.

    ![Nuova cartella](first-web-api-mac/_static/folder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.
1. Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.
1. Sostituire il codice del modello con il codice seguente:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

La proprietà `Id` funziona come chiave univoca in un database relazionale.

È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

## <a name="add-a-database-context"></a>Aggiungere un contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Aggiungere Microsoft.EntityFrameworkCore.SqlServer

1. Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.
1. Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.
1. Nel riquadro sinistro selezionare **Microsoft. EntityFrameworkCore. SqlServer** .
1. Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.
1. Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.

![Gestione pacchetti NuGet](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a>Aggiungere il contesto del database TodoContext

* Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Aggiungere una classe `TodoContext` alla cartella *Models*.

---

* Immettere il codice seguente:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Registrare il contesto del database

In ASP.NET Core, i servizi, ad esempio il contesto DI database, devono essere registrati con il contenitore [di inserimento delle dipendenze](xref:fundamentals/dependency-injection) . Il contenitore rende disponibile il servizio ai controller.

Aggiornare *Startup.cs* con il codice evidenziato seguente:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Il codice precedente:

* Rimuove le dichiarazioni `using` inutilizzate.
* Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.
* Specifica che il contesto del database userà un database in memoria.

## <a name="scaffold-a-controller"></a>Eseguire lo scaffolding di un controller

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.
1. Selezionare **aggiungi**  > **nuovo elemento con impalcatura**.
1. Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.
1. Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):
    * Selezionare **TodoItem (TodoApi. Models)** nella **classe del modello**.
    * Selezionare **TodoContext (TodoApi. Models)** nella **classe del contesto dati**.
    * Selezionare **Aggiungi**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

Eseguire i comandi seguenti:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

I comandi precedenti:

* Aggiungere i pacchetti NuGet necessari per lo scaffolding.
* Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).
* Esegue lo scaffolding di `TodoItemsController`.

---

Il codice generato:

* Definisce una classe controller API senza metodi.
* Contrassegna la classe con l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute). L'attributo indica che il controller risponde alle richieste di API Web. Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.
* Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller. Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.

## <a name="examine-the-posttodoitem-create-method"></a>Esaminare il metodo di creazione PostTodoItem

Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute). Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.

Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:

* Restituisce un codice di stato HTTP 201 in caso di esito positivo. HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.
* Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta. L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato. Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`. La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.

### <a name="install-postman"></a>Installare Postman

Questa esercitazione usa Postman per testare l'API Web.

1. Installare [Postman](https://www.getpostman.com/downloads/)
1. Avviare l'app Web.
1. Avviare Postman.
1. Disattivare **SSL certificate verification** (Verifica certificato SSL)
1. In **File**  > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.

    > [!WARNING]
    > Riattivare la verifica dei certificati SSL al termine del test del controller.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Testare PostTodoItem con Postman

1. Creare una nuova richiesta.
1. Impostare il metodo HTTP su `POST`.
1. Fare clic sulla scheda **Body** (Corpo).
1. Selezionare il pulsante di opzione **raw** (non elaborato).
1. Impostare il tipo su **JSON (application/json)** .
1. Nel corpo della richiesta immettere JSON per un elemento attività:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. Selezionare **Send** (Invia).

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a>Testare l'URI dell'intestazione della posizione

1. Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).
1. Copiare il valore dell'intestazione **Location** (Posizione):

    ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/create.png)

1. Impostare il metodo su GET.
1. Incollare l'URI, ad esempio `https://localhost:5001/api/TodoItems/1`.
1. Selezionare **Send** (Invia).

## <a name="examine-the-get-methods"></a>Esaminare i metodi GET

Questi metodi implementano due metodi GET:

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

Testare l'app chiamando i due endpoint da un browser o da Postman. Esempio:

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a>Testare Get con Postman

1. Creare una nuova richiesta.
1. Impostare il metodo HTTP su **GET**.
1. Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`. Ad esempio `https://localhost:5001/api/TodoItems`.
1. Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.
1. Selezionare **Send** (Invia).

Questa app usa un database in memoria. Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato. Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.

## <a name="routing-and-url-paths"></a>Routing e percorsi di URL

L'attributo [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) denota un metodo che risponde a una richiesta HTTP Get. Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:

* Iniziare con la stringa di modello nell'attributo `Route` del controller:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller". In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems". Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.
* Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso. In questo esempio non si usa un modello. Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività. Quando viene richiamato `GetTodoItem`, il valore di `"{id}"` nell'URL viene fornito al metodo nel parametro `id`.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Valori restituiti

Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta. Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite. Le eccezioni non gestite vengono convertite in errori 5xx.

I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP. Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:

* Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.
* In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON. La restituzione di `item` risulta in una risposta HTTP 200.

## <a name="the-puttodoitem-method"></a>Metodo PutTodoItem

Esaminare il metodo `PutTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche. Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.

### <a name="test-the-puttodoitem-method"></a>Testare il metodo PutTodoItem

Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata. Deve esistere un elemento nel database prima di eseguire una chiamata PUT. Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.

Aggiornare l'attività con ID 1. Impostarne il nome su "feed Fish":

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

L'immagine seguente visualizza l'aggiornamento di Postman:

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a>Metodo DeleteTodoItem

Esaminare il metodo `DeleteTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Testare il metodo DeleteTodoItem

Usare Postman per eliminare un elemento attività:

1. Impostare il metodo su `DELETE`.
1. Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`.
1. Selezionare **Send** (Invia).

## <a name="call-the-web-api-with-javascript"></a>Chiamare l'API Web con JavaScript

Vedere [esercitazione: chiamare un'API web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Aggiungere il supporto per l'autenticazione a un'API Web

Vedere l'esercitazione su [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .

## <a name="additional-resources"></a>Risorse aggiuntive

[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).

Per altre informazioni, vedere le seguenti risorse:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Versione YouTube dell'esercitazione](https://www.youtube.com/watch?v=TTkhEyGBfAk)
