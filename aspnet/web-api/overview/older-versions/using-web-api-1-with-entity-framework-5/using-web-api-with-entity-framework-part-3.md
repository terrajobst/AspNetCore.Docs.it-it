---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Creazione di un Controller di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826274"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="0b288-102">Parte 3: Creazione di un Controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="0b288-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="0b288-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0b288-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0b288-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="0b288-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="0b288-105">Aggiungere un Controller di amministrazione</span><span class="sxs-lookup"><span data-stu-id="0b288-105">Add an Admin Controller</span></span>

<span data-ttu-id="0b288-106">In questa sezione si aggiungerà un controller API Web che supporta CRUD (creare, leggere, aggiornare ed eliminare) le operazioni sui prodotti.</span><span class="sxs-lookup"><span data-stu-id="0b288-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="0b288-107">Il controller userà Entity Framework per comunicare con il livello di database.</span><span class="sxs-lookup"><span data-stu-id="0b288-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="0b288-108">Solo gli amministratori saranno in grado di utilizzare il controller.</span><span class="sxs-lookup"><span data-stu-id="0b288-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="0b288-109">I clienti potranno accedere i prodotti tramite un altro controller.</span><span class="sxs-lookup"><span data-stu-id="0b288-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="0b288-110">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="0b288-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0b288-111">Selezionare **aggiungere** e quindi **Controller**.</span><span class="sxs-lookup"><span data-stu-id="0b288-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="0b288-112">Nel **Aggiungi Controller** finestra di dialogo nome al controller `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="0b288-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="0b288-113">Sotto **modello**, selezionare &quot;controller API con azioni di lettura/scrittura, che usa Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="0b288-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="0b288-114">Sotto **classe modello**, selezionare "Prodotto (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="0b288-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="0b288-115">Sotto **DataContext**, selezionare "&lt;nuovo contesto dati&gt;".</span><span class="sxs-lookup"><span data-stu-id="0b288-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="0b288-116">Se il **classe modello** elenco a discesa non include tutte le classi modello, assicurarsi che è stato compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="0b288-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="0b288-117">Entity Framework Usa la reflection, pertanto è necessario l'assembly compilato.</span><span class="sxs-lookup"><span data-stu-id="0b288-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="0b288-118">Selezione di "&lt;nuovo contesto dati&gt;" verrà aperta la **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="0b288-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="0b288-119">Nome del contesto dati `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="0b288-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="0b288-120">Fare clic su **OK** per chiudere la **nuovo contesto dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="0b288-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="0b288-121">Nel **Aggiungi Controller** finestra di dialogo, fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="0b288-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="0b288-122">Ecco cosa è stato aggiunto al progetto:</span><span class="sxs-lookup"><span data-stu-id="0b288-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="0b288-123">Una classe denominata `OrdersContext` che deriva da **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="0b288-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="0b288-124">Questa classe fornisce l'associazione tra i modelli POCO e il database.</span><span class="sxs-lookup"><span data-stu-id="0b288-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="0b288-125">Controller Web API denominato `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="0b288-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="0b288-126">Questo controller supporta operazioni CRUD su `Product` istanze.</span><span class="sxs-lookup"><span data-stu-id="0b288-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="0b288-127">Usa il `OrdersContext` classi per comunicare con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b288-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="0b288-128">Una nuova stringa di connessione del database nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="0b288-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="0b288-129">Aprire il file OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="0b288-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="0b288-130">Si noti che il costruttore viene specificato il nome della stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="0b288-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="0b288-131">Questo nome fa riferimento alla stringa di connessione che è stato aggiunto al file Web. config.</span><span class="sxs-lookup"><span data-stu-id="0b288-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="0b288-132">Aggiungere le proprietà seguenti alla classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="0b288-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="0b288-133">Oggetto **DbSet** rappresenta un set di entità che è possibile eseguire query.</span><span class="sxs-lookup"><span data-stu-id="0b288-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="0b288-134">Di seguito è riportato il listato completo per il `OrdersContext` classe:</span><span class="sxs-lookup"><span data-stu-id="0b288-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="0b288-135">Il `AdminController` classe definisce cinque metodi che implementano la funzionalità CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="0b288-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="0b288-136">Ogni metodo corrisponde a un URI che il client può richiamare:</span><span class="sxs-lookup"><span data-stu-id="0b288-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="0b288-137">Metodo del controller</span><span class="sxs-lookup"><span data-stu-id="0b288-137">Controller Method</span></span> | <span data-ttu-id="0b288-138">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0b288-138">Description</span></span> | <span data-ttu-id="0b288-139">URI</span><span class="sxs-lookup"><span data-stu-id="0b288-139">URI</span></span> | <span data-ttu-id="0b288-140">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="0b288-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0b288-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="0b288-141">GetProducts</span></span> | <span data-ttu-id="0b288-142">Ottiene tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="0b288-142">Gets all products.</span></span> | <span data-ttu-id="0b288-143">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="0b288-143">api/products</span></span> | <span data-ttu-id="0b288-144">GET</span><span class="sxs-lookup"><span data-stu-id="0b288-144">GET</span></span> |
| <span data-ttu-id="0b288-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="0b288-145">GetProduct</span></span> | <span data-ttu-id="0b288-146">Trova un prodotto base all'ID.</span><span class="sxs-lookup"><span data-stu-id="0b288-146">Finds a product by ID.</span></span> | <span data-ttu-id="0b288-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="0b288-147">api/products/*id*</span></span> | <span data-ttu-id="0b288-148">GET</span><span class="sxs-lookup"><span data-stu-id="0b288-148">GET</span></span> |
| <span data-ttu-id="0b288-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="0b288-149">PutProduct</span></span> | <span data-ttu-id="0b288-150">Consente di aggiornare un prodotto.</span><span class="sxs-lookup"><span data-stu-id="0b288-150">Updates a product.</span></span> | <span data-ttu-id="0b288-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="0b288-151">api/products/*id*</span></span> | <span data-ttu-id="0b288-152">PUT</span><span class="sxs-lookup"><span data-stu-id="0b288-152">PUT</span></span> |
| <span data-ttu-id="0b288-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="0b288-153">PostProduct</span></span> | <span data-ttu-id="0b288-154">Crea un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="0b288-154">Creates a new product.</span></span> | <span data-ttu-id="0b288-155">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="0b288-155">api/products</span></span> | <span data-ttu-id="0b288-156">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="0b288-156">POST</span></span> |
| <span data-ttu-id="0b288-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="0b288-157">DeleteProduct</span></span> | <span data-ttu-id="0b288-158">Elimina un prodotto.</span><span class="sxs-lookup"><span data-stu-id="0b288-158">Deletes a product.</span></span> | <span data-ttu-id="0b288-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="0b288-159">api/products/*id*</span></span> | <span data-ttu-id="0b288-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="0b288-160">DELETE</span></span> |

<span data-ttu-id="0b288-161">Chiama ogni metodo `OrdersContext` eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="0b288-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="0b288-162">Chiamano i metodi che modificano la raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanenti le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="0b288-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="0b288-163">I controller sono creati per ogni richiesta HTTP e quindi eliminati, pertanto è necessario rendere persistenti le modifiche prima che restituisca un metodo.</span><span class="sxs-lookup"><span data-stu-id="0b288-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="0b288-164">Aggiungere un inizializzatore di Database</span><span class="sxs-lookup"><span data-stu-id="0b288-164">Add a Database Initializer</span></span>

<span data-ttu-id="0b288-165">Entity Framework è una funzionalità interessante che consente di popolare il database all'avvio, quindi di nuovo automaticamente il database ogni volta che i modelli di modifica.</span><span class="sxs-lookup"><span data-stu-id="0b288-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="0b288-166">Questa funzionalità è utile durante lo sviluppo, perché hai sempre alcuni dati di test, anche se si modificano i modelli.</span><span class="sxs-lookup"><span data-stu-id="0b288-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="0b288-167">In Esplora soluzioni fare doppio clic su cartella Models e creare una nuova classe denominata `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="0b288-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="0b288-168">Incollare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="0b288-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="0b288-169">Ereditando dal **DropCreateDatabaseIfModelChanges** (classe), si definiranno Entity Framework per eliminare il database ogni volta che si modifica classi del modello.</span><span class="sxs-lookup"><span data-stu-id="0b288-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="0b288-170">Quando Entity Framework consente di creare o ricrea il database, chiama il **Seed** metodo per popolare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="0b288-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="0b288-171">Usiamo il **Seed** metodo per aggiungere alcuni prodotti di esempio oltre a un ordine di esempio.</span><span class="sxs-lookup"><span data-stu-id="0b288-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="0b288-172">Questa funzionalità è molto utile per i test, ma non usare la **DropCreateDatabaseIfModelChanges** classe nell'ambiente di produzione, perché si potrebbero perdere i dati se vengono apportate modifiche a una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="0b288-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="0b288-173">Successivamente, aprire Global. asax e aggiungere il codice seguente per il **Application\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="0b288-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="0b288-174">Inviare una richiesta al Controller</span><span class="sxs-lookup"><span data-stu-id="0b288-174">Send a Request to the Controller</span></span>

<span data-ttu-id="0b288-175">A questo punto, è ancora stato scritto alcun codice client, ma è possibile richiamare API usando un web browser o un debug HTTP strumento, ad esempio web [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="0b288-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="0b288-176">In Visual Studio, premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="0b288-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="0b288-177">Web browser aprirà `http://localhost:*portnum*/`, dove *portnum* è un numero di porta.</span><span class="sxs-lookup"><span data-stu-id="0b288-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="0b288-178">Inviare una richiesta HTTP per "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="0b288-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="0b288-179">La prima richiesta potrebbe essere lenta per il completamento, poiché Entify Framework è necessaria creare ed effettuare il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="0b288-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="0b288-180">La risposta deve eseguire un'operazione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="0b288-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="0b288-181">[Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0b288-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
