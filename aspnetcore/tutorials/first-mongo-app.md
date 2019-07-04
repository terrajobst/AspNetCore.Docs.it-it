---
title: Creare un'API Web con ASP.NET Core e MongoDB
author: prkhandelwal
description: Questa esercitazione illustra come creare un'API Web ASP.NET Core con un database NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/10/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 426b4c0dee290153b9b1bf83deec14fa728183cb
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048087"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="1e6af-103">Creare un'API Web con ASP.NET Core e MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="1e6af-104">Di [Pratik Khandelwal](https://twitter.com/K2Prk) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1e6af-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1e6af-105">Questa esercitazione crea un'API Web che esegue operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) su un database NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="1e6af-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="1e6af-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="1e6af-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e6af-107">Configurare MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-107">Configure MongoDB</span></span>
> * <span data-ttu-id="1e6af-108">Creare un database MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="1e6af-109">Definire una raccolta e uno schema MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="1e6af-110">Eseguire operazioni CRUD di MongoDB da un'API Web</span><span class="sxs-lookup"><span data-stu-id="1e6af-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="1e6af-111">Personalizzare la serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="1e6af-111">Customize JSON serialization</span></span>

<span data-ttu-id="1e6af-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e6af-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e6af-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1e6af-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e6af-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e6af-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="1e6af-115">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1e6af-115">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="1e6af-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="1e6af-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1e6af-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e6af-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e6af-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="1e6af-119">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1e6af-119">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1e6af-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e6af-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="1e6af-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e6af-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="1e6af-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1e6af-123">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="1e6af-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="1e6af-124">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1e6af-124">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1e6af-125">Visual Studio per Mac versione 7.7 o successiva</span><span class="sxs-lookup"><span data-stu-id="1e6af-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="1e6af-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="1e6af-127">Configurare MongoDB</span><span class="sxs-lookup"><span data-stu-id="1e6af-127">Configure MongoDB</span></span>

<span data-ttu-id="1e6af-128">Se si usa Windows, MongoDB è installato in *C:\\Programmi\\MongoDB* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1e6af-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="1e6af-129">Aggiungere *C:\\Programmi\\MongoDB\\Server\\\<numero_versione>\\bin* alla variabile di ambiente `Path`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="1e6af-130">Questa modifica consente l'accesso MongoDB da qualsiasi posizione nel computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1e6af-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="1e6af-131">Usare la shell mongo nelle procedure seguenti per creare un database, creare le raccolte e archiviare i documenti.</span><span class="sxs-lookup"><span data-stu-id="1e6af-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="1e6af-132">Per altre informazioni sui comandi della shell mongo, vedere [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell) (Utilizzo della shell mongo).</span><span class="sxs-lookup"><span data-stu-id="1e6af-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="1e6af-133">Scegliere una directory nel computer di sviluppo per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="1e6af-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="1e6af-134">Ad esempio, *C:\\BooksData* in Windows.</span><span class="sxs-lookup"><span data-stu-id="1e6af-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="1e6af-135">Creare la directory se non esiste.</span><span class="sxs-lookup"><span data-stu-id="1e6af-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="1e6af-136">La shell mongo non consente di creare nuove directory.</span><span class="sxs-lookup"><span data-stu-id="1e6af-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="1e6af-137">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1e6af-137">Open a command shell.</span></span> <span data-ttu-id="1e6af-138">Eseguire il comando seguente per connettersi a MongoDB sulla porta predefinita 27017.</span><span class="sxs-lookup"><span data-stu-id="1e6af-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="1e6af-139">Ricordare di sostituire `<data_directory_path>` con la directory scelta nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="1e6af-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="1e6af-140">Aprire un'altra istanza della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1e6af-140">Open another command shell instance.</span></span> <span data-ttu-id="1e6af-141">Connettersi al database di test predefinito eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-141">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="1e6af-142">Eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1e6af-142">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="1e6af-143">Se non esiste già, viene creato un database denominato *BookstoreDb*.</span><span class="sxs-lookup"><span data-stu-id="1e6af-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="1e6af-144">Se il database esiste, la connessione viene aperta per le transazioni.</span><span class="sxs-lookup"><span data-stu-id="1e6af-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="1e6af-145">Creare una raccolta `Books` tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-145">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="1e6af-146">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-146">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="1e6af-147">Definire uno schema per la raccolta `Books` e inserire due documenti usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="1e6af-148">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-148">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="1e6af-149">Visualizzare i documenti nel database usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-149">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="1e6af-150">Viene visualizzato il risultato seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-150">The following result is displayed:</span></span>

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

    <span data-ttu-id="1e6af-151">Lo schema aggiunge una proprietà `_id` generata automaticamente di tipo `ObjectId` per ogni documento.</span><span class="sxs-lookup"><span data-stu-id="1e6af-151">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="1e6af-152">Il database è pronto.</span><span class="sxs-lookup"><span data-stu-id="1e6af-152">The database is ready.</span></span> <span data-ttu-id="1e6af-153">È possibile iniziare a creare l'API Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e6af-153">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="1e6af-154">Creare il progetto per l'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e6af-154">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e6af-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e6af-155">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1e6af-156">Passare a **File** > **Nuovo** > **progetto**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-156">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="1e6af-157">Selezionare il tipo di progetto **Applicazione Web ASP.NET Core** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-157">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="1e6af-158">Assegnare al progetto il nome *BooksApi* e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-158">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="1e6af-159">Selezionare il framework di destinazione **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-159">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="1e6af-160">Selezionare il modello di progetto **API** e scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-160">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="1e6af-161">Visitare la [Raccolta NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) per determinare la versione stabile più recente del driver .NET per MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1e6af-161">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="1e6af-162">Nella finestra **Console di Gestione pacchetti** passare alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e6af-162">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="1e6af-163">Eseguire il comando seguente per installare il driver .NET per MongoDB:</span><span class="sxs-lookup"><span data-stu-id="1e6af-163">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e6af-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e6af-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1e6af-165">Eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1e6af-165">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="1e6af-166">Un nuovo progetto API Web ASP.NET Core destinato a .NET Core viene generato e aperto in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1e6af-166">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="1e6af-167">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'BooksApi'. Add them?** (Risorse di compilazione e debug mancanti da "BooksApi". Aggiungerle?).</span><span class="sxs-lookup"><span data-stu-id="1e6af-167">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="1e6af-168">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-168">Select **Yes**.</span></span>
1. <span data-ttu-id="1e6af-169">Visitare la [Raccolta NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) per determinare la versione stabile più recente del driver .NET per MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1e6af-169">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="1e6af-170">Aprire **Terminale integrato** e passare alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e6af-170">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="1e6af-171">Eseguire il comando seguente per installare il driver .NET per MongoDB:</span><span class="sxs-lookup"><span data-stu-id="1e6af-171">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1e6af-172">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="1e6af-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="1e6af-173">Passare a **File** > **Nuova soluzione** >  **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-173">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="1e6af-174">Selezionare il modello di progetto C# **API Web ASP.NET Core** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-174">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="1e6af-175">Selezionare **.NET Core 2.2** nell'elenco a discesa **Framework di destinazione** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-175">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="1e6af-176">Immettere *BooksApi* per **Nome progetto** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-176">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="1e6af-177">Nel riquadro **Soluzione** fare clic con il pulsante destro del mouse sul nodo **Dipendenze** del progetto e scegliere **Aggiungi pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-177">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="1e6af-178">Immettere *MongoDB.Driver* nella casella di ricerca, selezionare il pacchetto *MongoDB.Driver* e quindi **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-178">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="1e6af-179">Selezionare il pulsante **Accetta** nella finestra di dialogo **Accettazione della licenza**.</span><span class="sxs-lookup"><span data-stu-id="1e6af-179">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="1e6af-180">Aggiungere un modello di entità</span><span class="sxs-lookup"><span data-stu-id="1e6af-180">Add an entity model</span></span>

1. <span data-ttu-id="1e6af-181">Aggiungere una directory *Models* alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e6af-181">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="1e6af-182">Aggiungere una classe `Book` alla directory *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-182">Add a `Book` class to the *Models* directory with the following code:</span></span>

    ```csharp
    using MongoDB.Bson;
    using MongoDB.Bson.Serialization.Attributes;
    
    namespace BooksApi.Models
    {
        public class Book
        {
            [BsonId]
            [BsonRepresentation(BsonType.ObjectId)]
            public string Id { get; set; }
    
            [BsonElement("Name")]
            public string BookName { get; set; }
    
            public decimal Price { get; set; }
    
            public string Category { get; set; }
    
            public string Author { get; set; }
        }
    }
    ```

    <span data-ttu-id="1e6af-183">Nella classe precedente, la proprietà `Id`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-183">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="1e6af-184">È obbligatoria per il mapping tra l'oggetto CLR (Common Language Runtime) e la raccolta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1e6af-184">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="1e6af-185">È annotata con [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) per definire questa proprietà come chiave primaria del documento.</span><span class="sxs-lookup"><span data-stu-id="1e6af-185">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="1e6af-186">È annotata con [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) per consentire il passaggio del parametro come tipo `string` invece di una struttura [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="1e6af-186">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="1e6af-187">Mongo gestisce la conversione da `string` a `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-187">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="1e6af-188">La proprietà `BookName` è annotata con l'attributo [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="1e6af-188">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="1e6af-189">Il valore dell'attributo `Name` rappresenta il nome della proprietà nella raccolta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1e6af-189">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="1e6af-190">Aggiungere un modello di configurazione</span><span class="sxs-lookup"><span data-stu-id="1e6af-190">Add a configuration model</span></span>

1. <span data-ttu-id="1e6af-191">Aggiungere i seguenti valori di configurazione del database a *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1e6af-191">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="1e6af-192">Aggiungere un file *BookstoreDatabaseSettings.cs* alla directory *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-192">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="1e6af-193">La classe `BookstoreDatabaseSettings` precedente viene utilizzata per archiviare i valori della proprietà `BookstoreDatabaseSettings` del file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1e6af-193">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="1e6af-194">I nomi delle proprietà JSON e C# sono identici per semplificare il processo di mapping.</span><span class="sxs-lookup"><span data-stu-id="1e6af-194">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="1e6af-195">Aggiungere il codice evidenziato seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-195">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

    <span data-ttu-id="1e6af-196">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-196">In the preceding code:</span></span>

    * <span data-ttu-id="1e6af-197">L'istanza di configurazione a cui si associa la sezione `BookstoreDatabaseSettings` del file *appsettings.json* viene registrata nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="1e6af-197">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="1e6af-198">Una proprietà `ConnectionString` di un oggetto `BookstoreDatabaseSettings` viene popolata con la proprietà `BookstoreDatabaseSettings:ConnectionString` in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1e6af-198">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="1e6af-199">L'interfaccia `IBookstoreDatabaseSettings` è registrata nell'inserimento di dipendenze con una [durata del servizio](xref:fundamentals/dependency-injection#service-lifetimes) singleton.</span><span class="sxs-lookup"><span data-stu-id="1e6af-199">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="1e6af-200">Quando avviene l'inserimento, l'istanza dell'interfaccia restituisce un oggetto `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-200">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="1e6af-201">Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere i riferimenti a `BookstoreDatabaseSettings` e `IBookstoreDatabaseSettings`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-201">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="1e6af-202">Aggiungere un servizio di operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="1e6af-202">Add a CRUD operations service</span></span>

1. <span data-ttu-id="1e6af-203">Aggiungere una directory *Services* alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e6af-203">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="1e6af-204">Aggiungere una classe `BookService` alla directory *Services* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-204">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="1e6af-205">Nel codice precedente un'istanza di `IBookstoreDatabaseSettings` viene recuperata dall'inserimento di dipendenze tramite l'inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="1e6af-205">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="1e6af-206">Questa tecnica fornisce accesso ai valori di configurazione di *appsettings.json* che sono stati aggiunti nella sezione [Aggiungere un modello di configurazione](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="1e6af-206">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="1e6af-207">Aggiungere il codice evidenziato seguente a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-207">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](first-mongo-app/sample_snapshot/BooksApi/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

    <span data-ttu-id="1e6af-208">Nel codice precedente la classe `BookService` è registrata con l'inserimento di dipendenze per supportare l'inserimento del costruttore nelle classi che la utilizzano.</span><span class="sxs-lookup"><span data-stu-id="1e6af-208">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="1e6af-209">La durata del servizio singleton è più appropriata perché `BookService` assume una dipendenza diretta a `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-209">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="1e6af-210">In base alle [linee guida per il riutilizzo di Mongo Client](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use) ufficiali, `MongoClient` deve essere registrato nell'inserimento di dipendenze con una durata del servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="1e6af-210">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="1e6af-211">Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `BookService`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-211">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="1e6af-212">La classe `BookService` usa i membri `MongoDB.Driver` seguenti per eseguire operazioni CRUD sul database:</span><span class="sxs-lookup"><span data-stu-id="1e6af-212">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="1e6af-213">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Legge l'istanza del server per l'esecuzione di operazioni di database.</span><span class="sxs-lookup"><span data-stu-id="1e6af-213">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="1e6af-214">Al costruttore di questa classe viene passata la stringa di connessione MongoDB:</span><span class="sxs-lookup"><span data-stu-id="1e6af-214">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="1e6af-215">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Rappresenta il database di Mongo per l'esecuzione delle operazioni.</span><span class="sxs-lookup"><span data-stu-id="1e6af-215">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="1e6af-216">Questa esercitazione usa il metodo [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) generico sull'interfaccia per ottenere l'accesso ai dati in una raccolta specifica.</span><span class="sxs-lookup"><span data-stu-id="1e6af-216">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="1e6af-217">Eseguire le operazioni CRUD sulla raccolta dopo la chiamata a questo metodo.</span><span class="sxs-lookup"><span data-stu-id="1e6af-217">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="1e6af-218">Nella chiamata del metodo `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-218">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="1e6af-219">`collection` rappresenta il nome della raccolta.</span><span class="sxs-lookup"><span data-stu-id="1e6af-219">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="1e6af-220">`TDocument` rappresenta il tipo di oggetto CLR archiviato nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="1e6af-220">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="1e6af-221">`GetCollection<TDocument>(collection)` restituisce un oggetto [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) che rappresenta la raccolta.</span><span class="sxs-lookup"><span data-stu-id="1e6af-221">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="1e6af-222">In questa esercitazione, vengono richiamati i metodi seguenti sulla raccolta:</span><span class="sxs-lookup"><span data-stu-id="1e6af-222">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="1e6af-223">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Elimina un singolo documento corrispondente ai criteri di ricerca specificati.</span><span class="sxs-lookup"><span data-stu-id="1e6af-223">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="1e6af-224">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Restituisce tutti i documenti nella raccolta corrispondenti ai criteri di ricerca specificati.</span><span class="sxs-lookup"><span data-stu-id="1e6af-224">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="1e6af-225">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserisce l'oggetto specificato come nuovo documento nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="1e6af-225">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="1e6af-226">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Sostituisce il singolo documento corrispondente ai criteri di ricerca specificati con l'oggetto specificato.</span><span class="sxs-lookup"><span data-stu-id="1e6af-226">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="1e6af-227">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="1e6af-227">Add a controller</span></span>

<span data-ttu-id="1e6af-228">Aggiungere una classe `BooksController` alla directory *Controllers* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-228">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="1e6af-229">Il controller dell'API Web precedente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-229">The preceding web API controller:</span></span>

* <span data-ttu-id="1e6af-230">Usa la classe `BookService` per eseguire operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="1e6af-230">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="1e6af-231">Contiene metodi di azione per supportare le richieste HTTP GET, POST, PUT e DELETE.</span><span class="sxs-lookup"><span data-stu-id="1e6af-231">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="1e6af-232">Chiama <xref:System.Web.Http.ApiController.CreatedAtRoute*> nel metodo dell'azione `Create` per restituire una risposta [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1e6af-232">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="1e6af-233">Il codice di stato 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="1e6af-233">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="1e6af-234">`Location` aggiunge anche un'intestazione `CreatedAtRoute` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="1e6af-234">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="1e6af-235">L'intestazione `Location` specifica l'URI del libro appena creato.</span><span class="sxs-lookup"><span data-stu-id="1e6af-235">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="1e6af-236">Testare l'API Web</span><span class="sxs-lookup"><span data-stu-id="1e6af-236">Test the web API</span></span>

1. <span data-ttu-id="1e6af-237">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1e6af-237">Build and run the app.</span></span>

1. <span data-ttu-id="1e6af-238">Passare a `http://localhost:<port>/api/books` per testare il metodo dell'azione `Get` senza parametri del controller.</span><span class="sxs-lookup"><span data-stu-id="1e6af-238">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="1e6af-239">Viene visualizzata la risposta JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-239">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="1e6af-240">Passare a `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` per testare il metodo dell'azione `Get` in overload del controller.</span><span class="sxs-lookup"><span data-stu-id="1e6af-240">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="1e6af-241">Viene visualizzata la risposta JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-241">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="1e6af-242">Configurare le opzioni di serializzazione JSON</span><span class="sxs-lookup"><span data-stu-id="1e6af-242">Configure JSON serialization options</span></span>

<span data-ttu-id="1e6af-243">Esistono due dettagli da modificare per le risposte JSON restituite nella sezione [Testare l'API Web](#test-the-web-api):</span><span class="sxs-lookup"><span data-stu-id="1e6af-243">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="1e6af-244">La notazione a cammello predefinita per i nomi di proprietà deve essere modificata in modo da adottare la convenzione Pascal dei nomi di proprietà dell'oggetto CLR.</span><span class="sxs-lookup"><span data-stu-id="1e6af-244">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="1e6af-245">La proprietà `bookName` deve essere restituita come `Name`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-245">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="1e6af-246">Per soddisfare i requisiti precedenti, apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="1e6af-246">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="1e6af-247">In `Startup.ConfigureServices` concatenare il codice evidenziato seguente alla chiamata del metodo `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-247">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

    <span data-ttu-id="1e6af-248">Con la modifica precedente, i nomi delle proprietà nella risposta JSON serializzata dell'API Web corrispondono ai nomi di proprietà corrispondenti nel tipo di oggetto CLR.</span><span class="sxs-lookup"><span data-stu-id="1e6af-248">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="1e6af-249">Ad esempio, la proprietà `Author` della classe `Book` viene serializzata come `Author`.</span><span class="sxs-lookup"><span data-stu-id="1e6af-249">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="1e6af-250">In *Models/Book.cs* annotare la proprietà `BookName` con l'attributo [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) seguente:</span><span class="sxs-lookup"><span data-stu-id="1e6af-250">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

    <span data-ttu-id="1e6af-251">Il valore `Name` dell'attributo `[JsonProperty]` rappresenta il nome della proprietà nella risposta JSON serializzata dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="1e6af-251">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="1e6af-252">Aggiungere il codice seguente all'inizio di *Models/Book.cs* per risolvere il riferimento all'attributo `[JsonProperty]`:</span><span class="sxs-lookup"><span data-stu-id="1e6af-252">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="1e6af-253">Ripetere i passaggi definiti nella sezione [Testare l'API Web](#test-the-web-api).</span><span class="sxs-lookup"><span data-stu-id="1e6af-253">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="1e6af-254">Si noti la differenza nei nomi di proprietà JSON.</span><span class="sxs-lookup"><span data-stu-id="1e6af-254">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e6af-255">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1e6af-255">Next steps</span></span>

<span data-ttu-id="1e6af-256">Per altre informazioni sulla creazione di API Web ASP.NET Core, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="1e6af-256">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="1e6af-257">Versione YouTube dell'articolo</span><span class="sxs-lookup"><span data-stu-id="1e6af-257">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
