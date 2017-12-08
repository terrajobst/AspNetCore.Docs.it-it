---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Utilizzare migrazioni Code First per il seeding del Database | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="f5c03-102">Utilizzare migrazioni Code First per il seeding del Database</span><span class="sxs-lookup"><span data-stu-id="f5c03-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="f5c03-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5c03-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f5c03-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="f5c03-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="f5c03-105">In questa sezione, si utilizzerà [migrazioni Code First](https://msdn.microsoft.com/en-us/data/jj591621) in Entity Framework per il seeding del database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="f5c03-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="f5c03-106">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f5c03-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f5c03-107">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f5c03-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="f5c03-108">Questo comando aggiunge una cartella denominata migrazioni al progetto, oltre a un file di codice denominato Configuration.cs nella cartella migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f5c03-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="f5c03-109">Aprire il file Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="f5c03-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="f5c03-110">Aggiungere il seguente **utilizzando** istruzione.</span><span class="sxs-lookup"><span data-stu-id="f5c03-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="f5c03-111">Quindi aggiungere il codice seguente per il **Configuration.Seed** metodo:</span><span class="sxs-lookup"><span data-stu-id="f5c03-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="f5c03-112">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5c03-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="f5c03-113">Il primo comando genera codice che crea il database e il secondo comando esegue il codice.</span><span class="sxs-lookup"><span data-stu-id="f5c03-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="f5c03-114">Il database viene creato localmente, utilizzando [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="f5c03-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="f5c03-115">Esplorare l'API (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="f5c03-115">Explore the API (Optional)</span></span>

<span data-ttu-id="f5c03-116">Premere F5 per eseguire l'applicazione in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="f5c03-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="f5c03-117">Visual Studio avvia IIS Express ed esegue l'app web.</span><span class="sxs-lookup"><span data-stu-id="f5c03-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="f5c03-118">Visual Studio, quindi, viene avviato un browser e verrà visualizzata la home page dell'app.</span><span class="sxs-lookup"><span data-stu-id="f5c03-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="f5c03-119">Quando Visual Studio viene eseguito un progetto web, viene assegnato un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="f5c03-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="f5c03-120">Nell'immagine seguente, il numero di porta è 50524.</span><span class="sxs-lookup"><span data-stu-id="f5c03-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="f5c03-121">Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="f5c03-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="f5c03-122">Home page viene implementata utilizzando ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f5c03-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="f5c03-123">Nella parte superiore della pagina, è presente un collegamento che afferma "API".</span><span class="sxs-lookup"><span data-stu-id="f5c03-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="f5c03-124">Questo collegamento consente di visualizzare una pagina della Guida generato automaticamente per l'API web.</span><span class="sxs-lookup"><span data-stu-id="f5c03-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="f5c03-125">(Per informazioni su come aggiungere la propria documentazione per la pagina e la modalità di generazione di questa pagina della Guida, vedere [la creazione di pagine della Guida per ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) È possibile fare clic su Guida di collegamenti della pagina per visualizzare i dettagli sull'API, incluso il formato di richiesta e risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c03-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="f5c03-126">L'API consente le operazioni CRUD nel database.</span><span class="sxs-lookup"><span data-stu-id="f5c03-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="f5c03-127">Di seguito viene riepilogata l'API.</span><span class="sxs-lookup"><span data-stu-id="f5c03-127">The following summarizes the API.</span></span>

| <span data-ttu-id="f5c03-128">Autori</span><span class="sxs-lookup"><span data-stu-id="f5c03-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="f5c03-129">OTTENERE l'api o gli autori</span><span class="sxs-lookup"><span data-stu-id="f5c03-129">GET api/authors</span></span> | <span data-ttu-id="f5c03-130">Ottenere tutti gli autori.</span><span class="sxs-lookup"><span data-stu-id="f5c03-130">Get all authors.</span></span> |
| <span data-ttu-id="f5c03-131">Api GET/autori / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-131">GET api/authors/{id}</span></span> | <span data-ttu-id="f5c03-132">Ottenere un autore dall'ID.</span><span class="sxs-lookup"><span data-stu-id="f5c03-132">Get an author by ID.</span></span> |
| <span data-ttu-id="f5c03-133">Autori di POST/api /</span><span class="sxs-lookup"><span data-stu-id="f5c03-133">POST /api/authors</span></span> | <span data-ttu-id="f5c03-134">Creare un nuovo autore.</span><span class="sxs-lookup"><span data-stu-id="f5c03-134">Create a new author.</span></span> |
| <span data-ttu-id="f5c03-135">Inserire /api autori / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="f5c03-136">Aggiornare un autore esistente.</span><span class="sxs-lookup"><span data-stu-id="f5c03-136">Update an existing author.</span></span> |
| <span data-ttu-id="f5c03-137">ELIMINARE /api autori / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="f5c03-138">Eliminare un autore.</span><span class="sxs-lookup"><span data-stu-id="f5c03-138">Delete an author.</span></span> |

| <span data-ttu-id="f5c03-139">Documentazione</span><span class="sxs-lookup"><span data-stu-id="f5c03-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="f5c03-140">OTTENERE /api/books</span><span class="sxs-lookup"><span data-stu-id="f5c03-140">GET /api/books</span></span> | <span data-ttu-id="f5c03-141">Ottenere tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="f5c03-141">Get all books.</span></span> |
| <span data-ttu-id="f5c03-142">OTTENERE /api documentazione / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-142">GET /api/books/{id}</span></span> | <span data-ttu-id="f5c03-143">Ottenere un libro di ID.</span><span class="sxs-lookup"><span data-stu-id="f5c03-143">Get a book by ID.</span></span> |
| <span data-ttu-id="f5c03-144">REGISTRARE/api/documentazione</span><span class="sxs-lookup"><span data-stu-id="f5c03-144">POST /api/books</span></span> | <span data-ttu-id="f5c03-145">Creare una nuova rubrica.</span><span class="sxs-lookup"><span data-stu-id="f5c03-145">Create a new book.</span></span> |
| <span data-ttu-id="f5c03-146">Inserire /api documentazione / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="f5c03-147">Aggiornare un libro esistente.</span><span class="sxs-lookup"><span data-stu-id="f5c03-147">Update an existing book.</span></span> |
| <span data-ttu-id="f5c03-148">ELIMINARE /api documentazione / {id}</span><span class="sxs-lookup"><span data-stu-id="f5c03-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="f5c03-149">Eliminare un libro.</span><span class="sxs-lookup"><span data-stu-id="f5c03-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="f5c03-150">Visualizzare il Database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="f5c03-150">View the Database (Optional)</span></span>

<span data-ttu-id="f5c03-151">Quando è stato eseguito il comando Update-Database, EF creato il database e chiamare il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="f5c03-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="f5c03-152">Quando si esegue l'applicazione localmente, Entity Framework Usa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="f5c03-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="f5c03-153">È possibile visualizzare il database in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f5c03-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="f5c03-154">Dal **vista** dal menu **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f5c03-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="f5c03-155">Nel **Connetti al Server** finestra di dialogo, nel **nome Server** casella di modifica, digitare "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="f5c03-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="f5c03-156">Lasciare il **autenticazione** opzione come "Autenticazione di Windows".</span><span class="sxs-lookup"><span data-stu-id="f5c03-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="f5c03-157">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="f5c03-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="f5c03-158">Visual Studio si connette al database locale e Mostra i database esistenti nella finestra Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f5c03-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="f5c03-159">È possibile espandere i nodi per visualizzare le tabelle create Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f5c03-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="f5c03-160">Per visualizzare i dati, fare doppio clic su una tabella e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="f5c03-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="f5c03-161">Nella schermata seguente mostra i risultati per la tabella di documentazione.</span><span class="sxs-lookup"><span data-stu-id="f5c03-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="f5c03-162">Si noti che EF popolato il database con i dati del valore di inizializzazione e la tabella contiene la chiave esterna alla tabella degli autori.</span><span class="sxs-lookup"><span data-stu-id="f5c03-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="f5c03-163">[Precedente](part-2.md)
[Successivo](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="f5c03-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
