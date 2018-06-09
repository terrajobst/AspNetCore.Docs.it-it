---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con l'attributo Routing in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30223262"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1694c-102">Creare un'API REST con attributo Routing in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1694c-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1694c-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1694c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1694c-104">Web API 2 supporta un nuovo tipo di routing, denominata *attributo routing*.</span><span class="sxs-lookup"><span data-stu-id="1694c-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1694c-105">Per una panoramica generale di routing degli attributi, vedere [Routing degli attributi in Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="1694c-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="1694c-106">In questa esercitazione si utilizzerà il routing di attributo per creare un'API REST per una raccolta di documentazione.</span><span class="sxs-lookup"><span data-stu-id="1694c-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="1694c-107">L'API offrirà supporto per le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1694c-107">The API will support the following actions:</span></span>

| <span data-ttu-id="1694c-108">Operazione</span><span class="sxs-lookup"><span data-stu-id="1694c-108">Action</span></span> | <span data-ttu-id="1694c-109">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="1694c-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="1694c-110">Ottenere un elenco di tutti i libri.</span><span class="sxs-lookup"><span data-stu-id="1694c-110">Get a list of all books.</span></span> | <span data-ttu-id="1694c-111">documentazione/api /</span><span class="sxs-lookup"><span data-stu-id="1694c-111">/api/books</span></span> |
| <span data-ttu-id="1694c-112">Ottenere un libro di ID.</span><span class="sxs-lookup"><span data-stu-id="1694c-112">Get a book by ID.</span></span> | <span data-ttu-id="1694c-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="1694c-113">/api/books/1</span></span> |
| <span data-ttu-id="1694c-114">Ottenere i dettagli di un libro.</span><span class="sxs-lookup"><span data-stu-id="1694c-114">Get the details of a book.</span></span> | <span data-ttu-id="1694c-115">/API/books/1/Details</span><span class="sxs-lookup"><span data-stu-id="1694c-115">/api/books/1/details</span></span> |
| <span data-ttu-id="1694c-116">Ottenere un elenco di libri per genere.</span><span class="sxs-lookup"><span data-stu-id="1694c-116">Get a list of books by genre.</span></span> | <span data-ttu-id="1694c-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="1694c-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="1694c-118">Ottenere un elenco di libri per data di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1694c-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="1694c-119">/API/books/date/2013-02-16 /api/books/date/2013/02/16 (modulo alternativo)</span><span class="sxs-lookup"><span data-stu-id="1694c-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="1694c-120">Ottenere un elenco di libri da un particolare autore.</span><span class="sxs-lookup"><span data-stu-id="1694c-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="1694c-121">/API/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="1694c-121">/api/authors/1/books</span></span> |

<span data-ttu-id="1694c-122">Tutti i metodi sono di sola lettura (richieste HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="1694c-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="1694c-123">Per il livello dati, si userà Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1694c-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="1694c-124">Record della Rubrica saranno disponibili i seguenti campi:</span><span class="sxs-lookup"><span data-stu-id="1694c-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="1694c-125">Id</span><span class="sxs-lookup"><span data-stu-id="1694c-125">ID</span></span>
- <span data-ttu-id="1694c-126">Titolo</span><span class="sxs-lookup"><span data-stu-id="1694c-126">Title</span></span>
- <span data-ttu-id="1694c-127">Genere</span><span class="sxs-lookup"><span data-stu-id="1694c-127">Genre</span></span>
- <span data-ttu-id="1694c-128">Data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="1694c-128">Publication date</span></span>
- <span data-ttu-id="1694c-129">Prezzo</span><span class="sxs-lookup"><span data-stu-id="1694c-129">Price</span></span>
- <span data-ttu-id="1694c-130">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1694c-130">Description</span></span>
- <span data-ttu-id="1694c-131">AuthorID (chiave esterna a una tabella di autori)</span><span class="sxs-lookup"><span data-stu-id="1694c-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="1694c-132">Per la maggior parte delle richieste, tuttavia, l'API restituirà un sottoinsieme di dati (titolo, autore e genere).</span><span class="sxs-lookup"><span data-stu-id="1694c-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="1694c-133">Per ottenere il record, il client richieste `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="1694c-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1694c-134">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1694c-134">Prerequisites</span></span>

<span data-ttu-id="1694c-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="1694c-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1694c-136">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1694c-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="1694c-137">Iniziare eseguendo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1694c-137">Start by running Visual Studio.</span></span> <span data-ttu-id="1694c-138">Dal **File** dal menu **New** e quindi selezionare **progetto**.</span><span class="sxs-lookup"><span data-stu-id="1694c-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="1694c-139">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="1694c-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1694c-140">In **Visual c#** selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="1694c-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1694c-141">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="1694c-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1694c-142">Denominare il progetto &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="1694c-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="1694c-143">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello.</span><span class="sxs-lookup"><span data-stu-id="1694c-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="1694c-144">In "Aggiungere cartelle e i riferimenti per core", selezionare il **API Web** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="1694c-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1694c-145">Fare clic su **creare progetto**.</span><span class="sxs-lookup"><span data-stu-id="1694c-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="1694c-146">Verrà creato un progetto di base che è configurato per la funzionalità API Web.</span><span class="sxs-lookup"><span data-stu-id="1694c-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="1694c-147">Modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="1694c-147">Domain Models</span></span>

<span data-ttu-id="1694c-148">Successivamente, aggiungere le classi per i modelli di dominio.</span><span class="sxs-lookup"><span data-stu-id="1694c-148">Next, add classes for domain models.</span></span> <span data-ttu-id="1694c-149">In Esplora soluzioni, fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="1694c-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="1694c-150">Selezionare **Aggiungi**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="1694c-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="1694c-151">Assegnare alla classe il nome `Author`.</span><span class="sxs-lookup"><span data-stu-id="1694c-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="1694c-152">Sostituire il codice in Author.cs con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1694c-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="1694c-153">A questo punto aggiungere un'altra classe denominata `Book`.</span><span class="sxs-lookup"><span data-stu-id="1694c-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="1694c-154">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="1694c-154">Add a Web API Controller</span></span>

<span data-ttu-id="1694c-155">In questo passaggio verrà aggiunto un controller API Web che usa Entity Framework del livello dati.</span><span class="sxs-lookup"><span data-stu-id="1694c-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="1694c-156">Premere CTRL+MAIUSC+B per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="1694c-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="1694c-157">Entity Framework utilizza la reflection per individuare le proprietà dei modelli, pertanto richiede un assembly compilato creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="1694c-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="1694c-158">In Esplora soluzioni, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="1694c-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1694c-159">Selezionare **Aggiungi**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="1694c-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="1694c-160">Nel **aggiungere lo scaffolding** finestra di dialogo, seleziona "Web API 2 Controller con azioni di lettura/scrittura, mediante Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="1694c-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="1694c-161">Nel **Aggiungi Controller** finestra di dialogo per **nome Controller**, immettere &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1694c-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="1694c-162">Selezionare il &quot;utilizzare azioni asincrone del controller&quot; casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="1694c-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="1694c-163">Per **classe modello**selezionare &quot;Book&quot;.</span><span class="sxs-lookup"><span data-stu-id="1694c-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="1694c-164">(Se non viene visualizzato il `Book` classe elencati nella casella di riepilogo, verificare che il progetto è stato generato.) Fare clic sul pulsante "+".</span><span class="sxs-lookup"><span data-stu-id="1694c-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="1694c-165">Fare clic su **Aggiungi** nel **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="1694c-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="1694c-166">Fare clic su **Aggiungi** nel **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="1694c-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="1694c-167">Lo scaffolding consente di aggiungere una classe denominata `BooksController` che definisce il controller API.</span><span class="sxs-lookup"><span data-stu-id="1694c-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="1694c-168">Aggiunge inoltre una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1694c-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="1694c-169">Valore di inizializzazione del Database</span><span class="sxs-lookup"><span data-stu-id="1694c-169">Seed the Database</span></span>

<span data-ttu-id="1694c-170">Dal menu Strumenti, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1694c-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="1694c-171">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1694c-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="1694c-172">Questo comando crea una cartella di migrazioni e aggiunge un nuovo file di codice denominato Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1694c-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="1694c-173">Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="1694c-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="1694c-174">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="1694c-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="1694c-175">Questi comandi creano un database locale e richiamano il metodo di inizializzazione per popolare il database.</span><span class="sxs-lookup"><span data-stu-id="1694c-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="1694c-176">Aggiungere le classi DTO</span><span class="sxs-lookup"><span data-stu-id="1694c-176">Add DTO Classes</span></span>

<span data-ttu-id="1694c-177">Se si esegue ora l'applicazione e inviarla una richiesta GET a /api/books/1, la risposta è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="1694c-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="1694c-178">(Aggiunto rientro per migliorare la leggibilità).</span><span class="sxs-lookup"><span data-stu-id="1694c-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="1694c-179">Al contrario, si desidera la richiesta per restituire un subset dei campi.</span><span class="sxs-lookup"><span data-stu-id="1694c-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="1694c-180">Inoltre, si desidera restituire il nome dell'autore, anziché l'ID autore.</span><span class="sxs-lookup"><span data-stu-id="1694c-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="1694c-181">A tale scopo, verrà modificata i metodi di controller per restituire un *oggetto di trasferimento dati* (DTO) anziché il modello di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1694c-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="1694c-182">Un DTO è un oggetto che può trasportare solo dati.</span><span class="sxs-lookup"><span data-stu-id="1694c-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="1694c-183">In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi** | **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="1694c-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="1694c-184">Denominare la cartella &quot;DTO&quot;.</span><span class="sxs-lookup"><span data-stu-id="1694c-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="1694c-185">Aggiungere una classe denominata `BookDto` nella cartella DTO, con la definizione seguente:</span><span class="sxs-lookup"><span data-stu-id="1694c-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="1694c-186">Aggiungere un'altra classe denominata `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="1694c-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="1694c-187">Aggiornare quindi il `BooksController` classe per restituire `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="1694c-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="1694c-188">Si userà il [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metodo progetto `Book` istanze per `BookDto` istanze.</span><span class="sxs-lookup"><span data-stu-id="1694c-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="1694c-189">Di seguito è riportato il codice aggiornato per la classe controller.</span><span class="sxs-lookup"><span data-stu-id="1694c-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="1694c-190">È stato eliminato il `PutBook`, `PostBook`, e `DeleteBook` metodi, perché non sono necessari per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1694c-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="1694c-191">Se si esegue l'applicazione e richiedere /api/books/1, il corpo della risposta deve essere simile a questo:</span><span class="sxs-lookup"><span data-stu-id="1694c-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="1694c-192">Aggiungere gli attributi delle Route</span><span class="sxs-lookup"><span data-stu-id="1694c-192">Add Route Attributes</span></span>

<span data-ttu-id="1694c-193">Successivamente, verrà convertito il controller per l'utilizzo di routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="1694c-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="1694c-194">Aggiungere innanzitutto un **RoutePrefix** attributo nel controller.</span><span class="sxs-lookup"><span data-stu-id="1694c-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="1694c-195">Questo attributo definisce i segmenti URI iniziali per tutti i metodi in questo controller.</span><span class="sxs-lookup"><span data-stu-id="1694c-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1694c-196">Aggiungere quindi **[Route]** attributi per le azioni del controller, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1694c-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="1694c-197">Il modello di route per ogni metodo del controller è il prefisso più la stringa specificata nel **Route** attributo.</span><span class="sxs-lookup"><span data-stu-id="1694c-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="1694c-198">Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot;{id: int}&quot;, che corrisponde se il segmento URI contiene un valore intero.</span><span class="sxs-lookup"><span data-stu-id="1694c-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="1694c-199">Metodo</span><span class="sxs-lookup"><span data-stu-id="1694c-199">Method</span></span> | <span data-ttu-id="1694c-200">Modello di route</span><span class="sxs-lookup"><span data-stu-id="1694c-200">Route Template</span></span> | <span data-ttu-id="1694c-201">URI di esempio</span><span class="sxs-lookup"><span data-stu-id="1694c-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="1694c-202">"documentazione/api"</span><span class="sxs-lookup"><span data-stu-id="1694c-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="1694c-203">"api/books/{id:int}"</span><span class="sxs-lookup"><span data-stu-id="1694c-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="1694c-204">Ottenere i dettagli della Rubrica</span><span class="sxs-lookup"><span data-stu-id="1694c-204">Get Book Details</span></span>

<span data-ttu-id="1694c-205">Per ottenere i dettagli di libro, il client invia una richiesta GET al `/api/books/{id}/details`, dove *{id}* è l'ID del libro.</span><span class="sxs-lookup"><span data-stu-id="1694c-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="1694c-206">Aggiungere il metodo seguente alla classe `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1694c-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="1694c-207">Se si richiede `/api/books/1/details`, la risposta è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1694c-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="1694c-208">Ottenere documentazione per genere</span><span class="sxs-lookup"><span data-stu-id="1694c-208">Get Books By Genre</span></span>

<span data-ttu-id="1694c-209">Per ottenere un elenco di libri un genere specifico, il client invia una richiesta GET al `/api/books/genre`, dove *genre* è il nome del genere.</span><span class="sxs-lookup"><span data-stu-id="1694c-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="1694c-210">ad esempio `/api/books/fantasy`.</span><span class="sxs-lookup"><span data-stu-id="1694c-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="1694c-211">Aggiungere il seguente metodo alla `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1694c-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="1694c-212">Di seguito viene definita la una route contenente un parametro di {genre} nel modello URI.</span><span class="sxs-lookup"><span data-stu-id="1694c-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="1694c-213">Si noti che l'API Web è in grado di distinguere questi due URI e li indirizzano a metodi diversi:</span><span class="sxs-lookup"><span data-stu-id="1694c-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="1694c-214">Ciò accade perché il `GetBook` metodo include un vincolo che il segmento "id" deve essere un valore integer:</span><span class="sxs-lookup"><span data-stu-id="1694c-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="1694c-215">Se si richiede /api/books/fantasy, la risposta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="1694c-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="1694c-216">Ottenere i libri di autore</span><span class="sxs-lookup"><span data-stu-id="1694c-216">Get Books By Author</span></span>

<span data-ttu-id="1694c-217">Per ottenere un elenco di una documentazione per un determinato autore, il client invia una richiesta GET al `/api/authors/id/books`, dove *id* è l'ID dell'autore.</span><span class="sxs-lookup"><span data-stu-id="1694c-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="1694c-218">Aggiungere il seguente metodo alla `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1694c-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="1694c-219">In questo esempio è interessante poiché &quot;documentazione&quot; è considerato una risorsa figlio di &quot;autori&quot;.</span><span class="sxs-lookup"><span data-stu-id="1694c-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="1694c-220">Questo modello è piuttosto comune nelle API REST.</span><span class="sxs-lookup"><span data-stu-id="1694c-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="1694c-221">Tilde (~) nel modello di route esegue l'override di prefisso della route nel **RoutePrefix** attributo.</span><span class="sxs-lookup"><span data-stu-id="1694c-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="1694c-222">Ottenere documentazione per data di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="1694c-222">Get Books By Publication Date</span></span>

<span data-ttu-id="1694c-223">Per ottenere un elenco di libri per data di pubblicazione, il client invia una richiesta GET al `/api/books/date/yyyy-mm-dd`, dove *aaaa-mm-gg* Data.</span><span class="sxs-lookup"><span data-stu-id="1694c-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="1694c-224">Ecco un modo per eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="1694c-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="1694c-225">Il `{pubdate:datetime}` parametro è vincolato in modo che corrisponda un **DateTime** valore.</span><span class="sxs-lookup"><span data-stu-id="1694c-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="1694c-226">Questo procedimento funziona, ma è molto più permissivo di Microsoft è interessata.</span><span class="sxs-lookup"><span data-stu-id="1694c-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="1694c-227">Ad esempio, gli URI corrisponderà anche la route:</span><span class="sxs-lookup"><span data-stu-id="1694c-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="1694c-228">Non è verificato un errore con consentire gli URI.</span><span class="sxs-lookup"><span data-stu-id="1694c-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="1694c-229">Tuttavia, è possibile limitare la route in un formato specifico, aggiungere un vincolo di espressione regolare per il modello di route:</span><span class="sxs-lookup"><span data-stu-id="1694c-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="1694c-230">Solo le date nel formato &quot;aaaa-mm-gg&quot; corrisponderanno.</span><span class="sxs-lookup"><span data-stu-id="1694c-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="1694c-231">Si noti che non viene utilizzata l'espressione regolare per convalidare che si è arrivati a una data reale.</span><span class="sxs-lookup"><span data-stu-id="1694c-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="1694c-232">Che avviene quando l'API Web tenta di convertire il segmento URI in un **DateTime** istanza.</span><span class="sxs-lookup"><span data-stu-id="1694c-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="1694c-233">Una data non valida, ad esempio "2012-47-99 avrà esito negativo da convertire e il client riceverà un errore 404.</span><span class="sxs-lookup"><span data-stu-id="1694c-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="1694c-234">È anche possibile supportare un separatore di barra (`/api/books/date/yyyy/mm/dd`) mediante l'aggiunta di un altro **[Route]** attributo con un'altra espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="1694c-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="1694c-235">È un dettaglio sottile ma importante qui.</span><span class="sxs-lookup"><span data-stu-id="1694c-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="1694c-236">Il secondo modello di route è un carattere jolly (\*) all'inizio del parametro {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="1694c-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="1694c-237">In questo modo il motore di routing che {pubdate} deve corrispondere il resto dell'URI.</span><span class="sxs-lookup"><span data-stu-id="1694c-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="1694c-238">Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI.</span><span class="sxs-lookup"><span data-stu-id="1694c-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="1694c-239">In questo caso, si vuole {pubdate} estendersi su più segmenti URI:</span><span class="sxs-lookup"><span data-stu-id="1694c-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="1694c-240">Codice del controller</span><span class="sxs-lookup"><span data-stu-id="1694c-240">Controller Code</span></span>

<span data-ttu-id="1694c-241">Di seguito è riportato il codice completo per la classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="1694c-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="1694c-242">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1694c-242">Summary</span></span>

<span data-ttu-id="1694c-243">Attributo di routing consente che un maggiore controllo e una maggiore flessibilità quando si progetta l'URI per l'API.</span><span class="sxs-lookup"><span data-stu-id="1694c-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
