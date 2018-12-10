---
title: Creare API Web con ASP.NET Core e MongoDB
author: prkhandelwal
description: Questa esercitazione illustra come creare un'API Web ASP.NET Core con un database NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc,seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: df3b8656618c813838d6618efc9394f0ccb6e563
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121479"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="755c9-103">Creare un'API Web con ASP.NET Core e MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="755c9-104">Di [Pratik Khandelwal](https://twitter.com/K2Prk) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="755c9-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="755c9-105">Questa esercitazione crea un'API Web che esegue operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) su un database NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="755c9-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="755c9-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="755c9-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="755c9-107">Configurare MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-107">Configure MongoDB</span></span>
> * <span data-ttu-id="755c9-108">Creare un database MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="755c9-109">Definire una raccolta e uno schema MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="755c9-110">Eseguire operazioni CRUD di MongoDB da un'API Web</span><span class="sxs-lookup"><span data-stu-id="755c9-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="755c9-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="755c9-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="755c9-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="755c9-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="755c9-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="755c9-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="755c9-114">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="755c9-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="755c9-115">[Visual Studio 2017 versione 15.9 o successive](https://www.visualstudio.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="755c9-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="755c9-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="755c9-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="755c9-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="755c9-118">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="755c9-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="755c9-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="755c9-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="755c9-120">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="755c9-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="755c9-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="755c9-122">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="755c9-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="755c9-123">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="755c9-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="755c9-124">Visual Studio per Mac versione 7.7 o successiva</span><span class="sxs-lookup"><span data-stu-id="755c9-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="755c9-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="755c9-126">Configurare MongoDB</span><span class="sxs-lookup"><span data-stu-id="755c9-126">Configure MongoDB</span></span>

<span data-ttu-id="755c9-127">Se si usa Windows, MongoDB è installato in *C:\Programmi\MongoDB* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="755c9-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="755c9-128">Aggiungere *C:\Programmi\MongoDB\Server\<numero_versione>\bin* alla variabile di ambiente `Path`.</span><span class="sxs-lookup"><span data-stu-id="755c9-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="755c9-129">Questa modifica consente l'accesso MongoDB da qualsiasi posizione nel computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="755c9-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="755c9-130">Usare la shell mongo nelle procedure seguenti per creare un database, creare le raccolte e archiviare i documenti.</span><span class="sxs-lookup"><span data-stu-id="755c9-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="755c9-131">Per altre informazioni sui comandi della shell mongo, vedere [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Utilizzo della shell mongo).</span><span class="sxs-lookup"><span data-stu-id="755c9-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="755c9-132">Scegliere una directory nel computer di sviluppo per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="755c9-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="755c9-133">Ad esempio, *C:\BooksData* in Windows.</span><span class="sxs-lookup"><span data-stu-id="755c9-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="755c9-134">Creare la directory se non esiste.</span><span class="sxs-lookup"><span data-stu-id="755c9-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="755c9-135">La shell mongo non consente di creare nuove directory.</span><span class="sxs-lookup"><span data-stu-id="755c9-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="755c9-136">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="755c9-136">Open a command shell.</span></span> <span data-ttu-id="755c9-137">Eseguire il comando seguente per connettersi a MongoDB sulla porta predefinita 27017.</span><span class="sxs-lookup"><span data-stu-id="755c9-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="755c9-138">Ricordare di sostituire `<data_directory_path>` con la directory scelta nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="755c9-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="755c9-139">Aprire un'altra istanza della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="755c9-139">Open another command shell instance.</span></span> <span data-ttu-id="755c9-140">Connettersi al database di test predefinito eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="755c9-141">Eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="755c9-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="755c9-142">Se non esiste già, viene creato un database denominato *BookstoreDb*.</span><span class="sxs-lookup"><span data-stu-id="755c9-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="755c9-143">Se il database esiste, la connessione viene aperta per le transazioni.</span><span class="sxs-lookup"><span data-stu-id="755c9-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="755c9-144">Creare una raccolta `Books` tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="755c9-145">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="755c9-146">Definire uno schema per la raccolta `Books` e inserire due documenti usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="755c9-147">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="755c9-148">Visualizzare i documenti nel database usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="755c9-149">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="755c9-150">Lo schema aggiunge una proprietà `_id` generata automaticamente di tipo `ObjectId` per ogni documento.</span><span class="sxs-lookup"><span data-stu-id="755c9-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="755c9-151">Il database è pronto.</span><span class="sxs-lookup"><span data-stu-id="755c9-151">The database is ready.</span></span> <span data-ttu-id="755c9-152">È possibile iniziare a creare l'API Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="755c9-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="755c9-153">Creare il progetto per l'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="755c9-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="755c9-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="755c9-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="755c9-155">Passare a **File** > **Nuovo** > **progetto**.</span><span class="sxs-lookup"><span data-stu-id="755c9-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="755c9-156">Selezionare **Applicazione Web ASP.NET Core**, denominare il progetto *BooksApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="755c9-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="755c9-157">Selezionare il framework di destinazione **.NET Core** e **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="755c9-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="755c9-158">Selezionare il modello di progetto **API** e fare clic su **OK**:</span><span class="sxs-lookup"><span data-stu-id="755c9-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="755c9-159">Nella finestra **Console di Gestione pacchetti** passare alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="755c9-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="755c9-160">Eseguire il comando seguente per installare il driver .NET per MongoDB:</span><span class="sxs-lookup"><span data-stu-id="755c9-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="755c9-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="755c9-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="755c9-162">Eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="755c9-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="755c9-163">Un nuovo progetto API Web ASP.NET Core destinato a .NET Core viene generato e aperto in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="755c9-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="755c9-164">Fare clic su **Yes** quando viene visualizzata la notifica *Required assets to build and debug are missing from 'BooksApi'. Add them?* (Asset richiesti per la compilazione e il debug mancanti da 'BooksApi'. Aggiungerli?).</span><span class="sxs-lookup"><span data-stu-id="755c9-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="755c9-165">Aprire **Terminale integrato** e passare alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="755c9-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="755c9-166">Eseguire il comando seguente per installare il driver .NET per MongoDB:</span><span class="sxs-lookup"><span data-stu-id="755c9-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="755c9-167">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="755c9-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="755c9-168">Passare a **File** > **Nuova soluzione** > **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="755c9-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="755c9-169">Selezionare il modello di progetto C# **API Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="755c9-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="755c9-170">Selezionare **.NET Core 2.2** nell'elenco a discesa **Framework di destinazione** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="755c9-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="755c9-171">Immettere *BooksApi* per **Nome progetto** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="755c9-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="755c9-172">Nel riquadro **Soluzione** fare clic con il pulsante destro del mouse sul nodo **Dipendenze** del progetto e scegliere **Aggiungi pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="755c9-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="755c9-173">Immettere *MongoDB* nella casella di ricerca, selezionare il pacchetto *MongoDB* e fare clic su **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="755c9-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="755c9-174">Fare clic sul pulsante **Accetta** nella finestra di dialogo **Accettazione della licenza**.</span><span class="sxs-lookup"><span data-stu-id="755c9-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="755c9-175">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="755c9-175">Add a model</span></span>

1. <span data-ttu-id="755c9-176">Aggiungere una directory *Models* alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="755c9-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="755c9-177">Aggiungere una classe `Book` alla directory *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="755c9-178">Nella classe precedente, la proprietà `Id` è obbligatoria per il mapping tra l'oggetto CLR (Common Language Runtime) e la raccolta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="755c9-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="755c9-179">Le altre proprietà nella classe sono decorate con l'attributo `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="755c9-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="755c9-180">Il valore dell'attributo rappresenta il nome della proprietà nella raccolta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="755c9-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="755c9-181">Aggiungere una classe di operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="755c9-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="755c9-182">Aggiungere una directory *Services* alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="755c9-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="755c9-183">Aggiungere una classe `BookService` alla directory *Services* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="755c9-184">Aggiungere la stringa di connessione MongoDB ad *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="755c9-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="755c9-185">Si accede alla proprietà `BookstoreDb` precedente nel costruttore della classe `BookService`.</span><span class="sxs-lookup"><span data-stu-id="755c9-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="755c9-186">In `Startup.ConfigureServices` registrare la classe `BookService` nel sistema di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="755c9-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="755c9-187">La registrazione del servizio precedente è necessaria per supportare l'inserimento del costruttore nelle classi che lo utilizzano.</span><span class="sxs-lookup"><span data-stu-id="755c9-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="755c9-188">La classe `BookService` usa i membri `MongoDB.Driver` seguenti per eseguire operazioni CRUD sul database:</span><span class="sxs-lookup"><span data-stu-id="755c9-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="755c9-189">`MongoClient` &ndash; Legge l'istanza del server per l'esecuzione di operazioni di database.</span><span class="sxs-lookup"><span data-stu-id="755c9-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="755c9-190">Al costruttore di questa classe viene passata la stringa di connessione MongoDB:</span><span class="sxs-lookup"><span data-stu-id="755c9-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="755c9-191">`IMongoDatabase` &ndash; Rappresenta il database di Mongo per l'esecuzione delle operazioni.</span><span class="sxs-lookup"><span data-stu-id="755c9-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="755c9-192">Questa esercitazione usa il metodo `GetCollection<T>(collection)` generico sull'interfaccia per ottenere l'accesso ai dati in una raccolta specifica.</span><span class="sxs-lookup"><span data-stu-id="755c9-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="755c9-193">Le operazioni CRUD possono essere eseguite sulla raccolta dopo la chiamata di questo metodo.</span><span class="sxs-lookup"><span data-stu-id="755c9-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="755c9-194">Nella chiamata del metodo `GetCollection<T>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="755c9-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="755c9-195">`collection` rappresenta il nome della raccolta.</span><span class="sxs-lookup"><span data-stu-id="755c9-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="755c9-196">`T` rappresenta il tipo di oggetto CLR archiviato nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="755c9-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="755c9-197">`GetCollection<T>(collection)` restituisce un oggetto `MongoCollection` che rappresenta la raccolta.</span><span class="sxs-lookup"><span data-stu-id="755c9-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="755c9-198">In questa esercitazione, vengono richiamati i metodi seguenti sulla raccolta:</span><span class="sxs-lookup"><span data-stu-id="755c9-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="755c9-199">`Find<T>` &ndash; Restituisce tutti i documenti nella raccolta corrispondenti ai criteri di ricerca specificati.</span><span class="sxs-lookup"><span data-stu-id="755c9-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="755c9-200">`InsertOne` &ndash; Inserisce l'oggetto specificato come nuovo documento nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="755c9-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="755c9-201">`ReplaceOne` &ndash; Sostituisce il singolo documento corrispondente ai criteri di ricerca specificati con l'oggetto specificato.</span><span class="sxs-lookup"><span data-stu-id="755c9-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="755c9-202">`DeleteOne` &ndash; Elimina un singolo documento corrispondente ai criteri di ricerca specificati.</span><span class="sxs-lookup"><span data-stu-id="755c9-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="755c9-203">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="755c9-203">Add a controller</span></span>

1. <span data-ttu-id="755c9-204">Aggiungere una classe `BooksController` alla directory *Controllers* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="755c9-205">Il controller dell'API Web precedente:</span><span class="sxs-lookup"><span data-stu-id="755c9-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="755c9-206">Usa la classe `BookService` per eseguire operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="755c9-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="755c9-207">Contiene metodi di azione per supportare le richieste HTTP GET, POST, PUT e DELETE.</span><span class="sxs-lookup"><span data-stu-id="755c9-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="755c9-208">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="755c9-208">Build and run the app.</span></span>
1. <span data-ttu-id="755c9-209">Passare a `http://localhost:<port>/api/books` nel browser.</span><span class="sxs-lookup"><span data-stu-id="755c9-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="755c9-210">Viene visualizzata la risposta JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="755c9-210">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="755c9-211">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="755c9-211">Next steps</span></span>

<span data-ttu-id="755c9-212">Per altre informazioni sulla creazione di API Web ASP.NET Core, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="755c9-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
