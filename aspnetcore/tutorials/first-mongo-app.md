---
title: Creare API Web con ASP.NET Core e MongoDB
author: prkhandelwal
description: Questa esercitazione illustra come creare un'API Web ASP.NET Core con un database NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5b8a0c963940d65545579b7120edac3571e4ad2a
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750684"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Creare un'API Web con ASP.NET Core e MongoDB

Di [Pratik Khandelwal](https://twitter.com/K2Prk) e [Scott Addie](https://twitter.com/Scott_Addie)

Questa esercitazione crea un'API Web che esegue operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) su un database NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Configurare MongoDB
> * Creare un database MongoDB
> * Definire una raccolta e uno schema MongoDB
> * Eseguire operazioni CRUD di MongoDB da un'API Web

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.2 o versione successiva](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017 versione 15.9 o successive](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) con il carico di lavoro **Sviluppo ASP.NET e Web**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.2 o versione successiva](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# per Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* [.NET Core SDK 2.2 o versione successiva](https://www.microsoft.com/net/download/all)
* [Visual Studio per Mac versione 7.7 o successiva](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Configurare MongoDB

Se si usa Windows, MongoDB è installato in *C:\\Programmi\\MongoDB* per impostazione predefinita. Aggiungere *C:\\Programmi\\MongoDB\\Server\\\<numero_versione>\\bin* alla variabile di ambiente `Path`. Questa modifica consente l'accesso MongoDB da qualsiasi posizione nel computer di sviluppo.

Usare la shell mongo nelle procedure seguenti per creare un database, creare le raccolte e archiviare i documenti. Per altre informazioni sui comandi della shell mongo, vedere [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Utilizzo della shell mongo).

1. Scegliere una directory nel computer di sviluppo per archiviare i dati. Ad esempio, *C:\\BooksData* in Windows. Creare la directory se non esiste. La shell mongo non consente di creare nuove directory.
1. Aprire una shell dei comandi. Eseguire il comando seguente per connettersi a MongoDB sulla porta predefinita 27017. Ricordare di sostituire `<data_directory_path>` con la directory scelta nel passaggio precedente.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Aprire un'altra istanza della shell dei comandi. Connettersi al database di test predefinito eseguendo il comando seguente:

    ```console
    mongo
    ```

1. Eseguire il comando seguente in una shell dei comandi:

    ```console
    use BookstoreDb
    ```

    Se non esiste già, viene creato un database denominato *BookstoreDb*. Se il database esiste, la connessione viene aperta per le transazioni.

1. Creare una raccolta `Books` tramite il comando seguente:

    ```console
    db.createCollection('Books')
    ```

    Viene visualizzato il risultato seguente:

    ```console
    { "ok" : 1 }
    ```

1. Definire uno schema per la raccolta `Books` e inserire due documenti usando il comando seguente:

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Viene visualizzato il risultato seguente:

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. Visualizzare i documenti nel database usando il comando seguente:

    ```console
    db.Books.find({}).pretty()
    ```

    Viene visualizzato il risultato seguente:

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    Lo schema aggiunge una proprietà `_id` generata automaticamente di tipo `ObjectId` per ogni documento.

Il database è pronto. È possibile iniziare a creare l'API Web ASP.NET Core.

## <a name="create-the-aspnet-core-web-api-project"></a>Creare il progetto per l'API Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Passare a **File** > **Nuovo** > **progetto**.
1. Selezionare **Applicazione Web ASP.NET Core**, denominare il progetto *BooksApi* e fare clic su **OK**.
1. Selezionare il framework di destinazione **.NET Core** e **ASP.NET Core 2.2**. Selezionare il modello di progetto **API** e fare clic su **OK**:
1. Visitare la [Raccolta NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) per determinare la versione stabile più recente del driver .NET per MongoDB. Nella finestra **Console di Gestione pacchetti** passare alla radice del progetto. Eseguire il comando seguente per installare il driver .NET per MongoDB:

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Eseguire i comandi seguenti in una shell dei comandi:

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    Un nuovo progetto API Web ASP.NET Core destinato a .NET Core viene generato e aperto in Visual Studio Code.

1. Fare clic su **Yes** quando viene visualizzata la notifica *Required assets to build and debug are missing from 'BooksApi'. Add them?* (Asset richiesti per la compilazione e il debug mancanti da 'BooksApi'. Aggiungerli?).
1. Visitare la [Raccolta NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) per determinare la versione stabile più recente del driver .NET per MongoDB. Aprire **Terminale integrato** e passare alla radice del progetto. Eseguire il comando seguente per installare il driver .NET per MongoDB:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

1. Passare a **File** > **Nuova soluzione** > **.NET Core** > **App**.
1. Selezionare il modello di progetto C# **API Web ASP.NET Core** e fare clic su **Avanti**.
1. Selezionare **.NET Core 2.2** nell'elenco a discesa **Framework di destinazione** e fare clic su **Avanti**.
1. Immettere *BooksApi* per **Nome progetto** e fare clic su **Crea**.
1. Nel riquadro **Soluzione** fare clic con il pulsante destro del mouse sul nodo **Dipendenze** del progetto e scegliere **Aggiungi pacchetti**.
1. Immettere *MongoDB* nella casella di ricerca, selezionare il pacchetto *MongoDB* e fare clic su **Aggiungi pacchetto**.
1. Fare clic sul pulsante **Accetta** nella finestra di dialogo **Accettazione della licenza**.

---

## <a name="add-a-model"></a>Aggiungere un modello

1. Aggiungere una directory *Models* alla radice del progetto.
1. Aggiungere una classe `Book` alla directory *Models* con il codice seguente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

Nella classe precedente, la proprietà `Id`:

* È obbligatoria per il mapping tra l'oggetto CLR (Common Language Runtime) e la raccolta MongoDB.
* È annotata con `[BsonId]` per definire questa proprietà come chiave primaria del documento.
* È annotata con `[BsonRepresentation(BsonType.ObjectId)]` per consentire il passaggio del parametro come tipo `string` invece di `ObjectId`. Mongo gestisce la conversione da `string` a `ObjectId`.

Le altre proprietà nella classe sono annotate con l'attributo `[BsonElement]`. Il valore dell'attributo rappresenta il nome della proprietà nella raccolta MongoDB.

## <a name="add-a-crud-operations-class"></a>Aggiungere una classe di operazioni CRUD

1. Aggiungere una directory *Services* alla radice del progetto.
1. Aggiungere una classe `BookService` alla directory *Services* con il codice seguente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Aggiungere la stringa di connessione MongoDB ad *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    Si accede alla proprietà `BookstoreDb` precedente nel costruttore della classe `BookService`.

1. In `Startup.ConfigureServices` registrare la classe `BookService` nel sistema di inserimento delle dipendenze:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    La registrazione del servizio precedente è necessaria per supportare l'inserimento del costruttore nelle classi che lo utilizzano.

La classe `BookService` usa i membri `MongoDB.Driver` seguenti per eseguire operazioni CRUD sul database:

* `MongoClient` &ndash; Legge l'istanza del server per l'esecuzione di operazioni di database. Al costruttore di questa classe viene passata la stringa di connessione MongoDB:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Rappresenta il database di Mongo per l'esecuzione delle operazioni. Questa esercitazione usa il metodo `GetCollection<T>(collection)` generico sull'interfaccia per ottenere l'accesso ai dati in una raccolta specifica. Le operazioni CRUD possono essere eseguite sulla raccolta dopo la chiamata di questo metodo. Nella chiamata del metodo `GetCollection<T>(collection)`:
  * `collection` rappresenta il nome della raccolta.
  * `T` rappresenta il tipo di oggetto CLR archiviato nella raccolta.

`GetCollection<T>(collection)` restituisce un oggetto `MongoCollection` che rappresenta la raccolta. In questa esercitazione, vengono richiamati i metodi seguenti sulla raccolta:

* `Find<T>` &ndash; Restituisce tutti i documenti nella raccolta corrispondenti ai criteri di ricerca specificati.
* `InsertOne` &ndash; Inserisce l'oggetto specificato come nuovo documento nella raccolta.
* `ReplaceOne` &ndash; Sostituisce il singolo documento corrispondente ai criteri di ricerca specificati con l'oggetto specificato.
* `DeleteOne` &ndash; Elimina un singolo documento corrispondente ai criteri di ricerca specificati.

## <a name="add-a-controller"></a>Aggiungere un controller

1. Aggiungere una classe `BooksController` alla directory *Controllers* con il codice seguente:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    Il controller dell'API Web precedente:

    * Usa la classe `BookService` per eseguire operazioni CRUD.
    * Contiene metodi di azione per supportare le richieste HTTP GET, POST, PUT e DELETE.
    * Il metodo <xref:System.Web.Http.ApiController.CreatedAtRoute*> restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server. `CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
1. Compilare ed eseguire l'app.
1. Passare a `http://localhost:<port>/api/books` nel browser. Viene visualizzata la risposta JSON seguente:

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla creazione di API Web ASP.NET Core, vedere le risorse seguenti:

* <xref:web-api/index>
* <xref:web-api/action-return-types>
