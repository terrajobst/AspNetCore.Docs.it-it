---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Creazione di un Controller di amministrazione | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="10add-102">Parte 3: Creazione di un Controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="10add-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="10add-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="10add-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="10add-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="10add-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="10add-105">Aggiungere un Controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="10add-105">Add an Admin Controller</span></span>

<span data-ttu-id="10add-106">In questa sezione verrà aggiunto un controller API Web che supporta CRUD (creare, leggere, aggiornare ed eliminare) operazioni sui prodotti.</span><span class="sxs-lookup"><span data-stu-id="10add-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="10add-107">Entity Framework verrà utilizzata dal controller per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="10add-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="10add-108">Solo gli amministratori potranno usare questo controller.</span><span class="sxs-lookup"><span data-stu-id="10add-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="10add-109">I clienti potranno accedere i prodotti tramite un altro controller.</span><span class="sxs-lookup"><span data-stu-id="10add-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="10add-110">In Esplora soluzioni, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="10add-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="10add-111">Selezionare **aggiungere** e quindi **Controller**.</span><span class="sxs-lookup"><span data-stu-id="10add-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="10add-112">Nel **Aggiungi Controller** finestra di dialogo, nome del controller `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="10add-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="10add-113">In **modello**selezionare &quot;controller API con azioni di lettura/scrittura, mediante Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="10add-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="10add-114">In **classe modello**, selezionare "Prodotto (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="10add-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="10add-115">In **contesto dati**, selezionare "&lt;nuovo contesto dati&gt;".</span><span class="sxs-lookup"><span data-stu-id="10add-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="10add-116">Se il **classe modello** elenco a discesa non mostra tutte le classi modello, assicurarsi che è stato compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="10add-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="10add-117">Entity Framework Usa la reflection, pertanto è necessario l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="10add-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="10add-118">Selezione di "&lt;nuovo contesto dati&gt;" verrà aperta la **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="10add-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="10add-119">Nome del contesto dati `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="10add-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="10add-120">Fare clic su **OK** per chiudere la **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="10add-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="10add-121">Nel **Aggiungi Controller** finestra di dialogo, fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="10add-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="10add-122">Ecco cosa è stata aggiunta al progetto:</span><span class="sxs-lookup"><span data-stu-id="10add-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="10add-123">Una classe denominata `OrdersContext` che deriva da **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="10add-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="10add-124">Questa classe fornisce l'associazione tra i modelli POCO e il database.</span><span class="sxs-lookup"><span data-stu-id="10add-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="10add-125">Controller API Web denominato `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="10add-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="10add-126">Il controller supporta operazioni CRUD su `Product` istanze.</span><span class="sxs-lookup"><span data-stu-id="10add-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="10add-127">Usa il `OrdersContext` classe per comunicare con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="10add-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="10add-128">Nuova stringa di connessione del database nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="10add-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="10add-129">Aprire il file OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="10add-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="10add-130">Si noti che il costruttore viene specificato il nome della stringa di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="10add-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="10add-131">Questo nome fa riferimento alla stringa di connessione che è stato aggiunto al file Web. config.</span><span class="sxs-lookup"><span data-stu-id="10add-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="10add-132">Aggiungere le proprietà seguenti alla classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="10add-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="10add-133">Oggetto **DbSet** rappresenta un set di entità che è possibile eseguire query.</span><span class="sxs-lookup"><span data-stu-id="10add-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="10add-134">Di seguito è riportato il listato completo per la `OrdersContext` classe:</span><span class="sxs-lookup"><span data-stu-id="10add-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="10add-135">La `AdminController` classe definisce i cinque metodi che implementano funzionalità CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="10add-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="10add-136">Ogni metodo corrisponde a un URI che il client può richiamare:</span><span class="sxs-lookup"><span data-stu-id="10add-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="10add-137">Metodo del controller</span><span class="sxs-lookup"><span data-stu-id="10add-137">Controller Method</span></span> | <span data-ttu-id="10add-138">Descrizione</span><span class="sxs-lookup"><span data-stu-id="10add-138">Description</span></span> | <span data-ttu-id="10add-139">URI</span><span class="sxs-lookup"><span data-stu-id="10add-139">URI</span></span> | <span data-ttu-id="10add-140">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="10add-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10add-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="10add-141">GetProducts</span></span> | <span data-ttu-id="10add-142">Ottiene tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="10add-142">Gets all products.</span></span> | <span data-ttu-id="10add-143">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="10add-143">api/products</span></span> | <span data-ttu-id="10add-144">GET</span><span class="sxs-lookup"><span data-stu-id="10add-144">GET</span></span> |
| <span data-ttu-id="10add-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="10add-145">GetProduct</span></span> | <span data-ttu-id="10add-146">Trova un prodotto in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="10add-146">Finds a product by ID.</span></span> | <span data-ttu-id="10add-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="10add-147">api/products/*id*</span></span> | <span data-ttu-id="10add-148">GET</span><span class="sxs-lookup"><span data-stu-id="10add-148">GET</span></span> |
| <span data-ttu-id="10add-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="10add-149">PutProduct</span></span> | <span data-ttu-id="10add-150">Aggiorna un prodotto.</span><span class="sxs-lookup"><span data-stu-id="10add-150">Updates a product.</span></span> | <span data-ttu-id="10add-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="10add-151">api/products/*id*</span></span> | <span data-ttu-id="10add-152">PUT</span><span class="sxs-lookup"><span data-stu-id="10add-152">PUT</span></span> |
| <span data-ttu-id="10add-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="10add-153">PostProduct</span></span> | <span data-ttu-id="10add-154">Crea un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="10add-154">Creates a new product.</span></span> | <span data-ttu-id="10add-155">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="10add-155">api/products</span></span> | <span data-ttu-id="10add-156">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="10add-156">POST</span></span> |
| <span data-ttu-id="10add-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="10add-157">DeleteProduct</span></span> | <span data-ttu-id="10add-158">Elimina un prodotto.</span><span class="sxs-lookup"><span data-stu-id="10add-158">Deletes a product.</span></span> | <span data-ttu-id="10add-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="10add-159">api/products/*id*</span></span> | <span data-ttu-id="10add-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="10add-160">DELETE</span></span> |

<span data-ttu-id="10add-161">Ogni metodo chiama `OrdersContext` per eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="10add-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="10add-162">Chiamano i metodi che modificano la raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanenti le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="10add-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="10add-163">I controller vengono creati per ogni richiesta HTTP e quindi eliminati, pertanto è necessario rendere permanenti le modifiche prima che venga restituito un metodo.</span><span class="sxs-lookup"><span data-stu-id="10add-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="10add-164">Aggiungere un inizializzatore del Database</span><span class="sxs-lookup"><span data-stu-id="10add-164">Add a Database Initializer</span></span>

<span data-ttu-id="10add-165">Entity Framework è una funzionalità interessante che consente di popolare il database all'avvio e ricreare automaticamente il database ogni volta che i modelli di modifica.</span><span class="sxs-lookup"><span data-stu-id="10add-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="10add-166">Questa funzionalità è utile durante lo sviluppo, poiché è sempre alcuni dati di test, anche se si modificano i modelli.</span><span class="sxs-lookup"><span data-stu-id="10add-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="10add-167">In Esplora soluzioni, fare clic sulla cartella di modelli e creare una nuova classe denominata `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="10add-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="10add-168">Incollare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="10add-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="10add-169">Tramite l'eredità dal **DropCreateDatabaseIfModelChanges** (classe), si definiranno Entity Framework per eliminare il database ogni volta che si modifica classi del modello.</span><span class="sxs-lookup"><span data-stu-id="10add-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="10add-170">Quando Entity Framework consente di creare o ricrea il database, chiama il **valore di inizializzazione** metodo per popolare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="10add-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="10add-171">Utilizziamo la **valore di inizializzazione** metodo per aggiungere alcuni prodotti di esempio più di un ordine di esempio.</span><span class="sxs-lookup"><span data-stu-id="10add-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="10add-172">Questa funzionalità è molto utile per il test, ma non usano il **DropCreateDatabaseIfModelChanges** classe nell'ambiente di produzione, perché si potrebbe perdere i dati se vengono apportate modifiche a una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="10add-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="10add-173">Successivamente, aprire Global. asax e aggiungere il codice seguente per il **applicazione\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="10add-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="10add-174">Inviare una richiesta per il Controller</span><span class="sxs-lookup"><span data-stu-id="10add-174">Send a Request to the Controller</span></span>

<span data-ttu-id="10add-175">A questo punto, è non scrivere alcun codice client, ma è possibile richiamare web API usando un web browser o per il debug di un HTTP strumento, ad esempio [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="10add-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="10add-176">In Visual Studio, premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="10add-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="10add-177">Aprire il browser web per `http://localhost:*portnum*/`, dove *portnum* rappresenta un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="10add-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="10add-178">Inviare una richiesta HTTP per "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="10add-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="10add-179">La prima richiesta potrebbe essere lenta da completare, Entify Framework in quanto deve creare e inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="10add-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="10add-180">La risposta è necessario un codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="10add-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="10add-181">[Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="10add-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
