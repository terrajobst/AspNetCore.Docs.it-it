---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creazione di prodotto e ordine controller | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="1b0f4-102">Parte 6: Creazione di prodotto e il controller di ordine</span><span class="sxs-lookup"><span data-stu-id="1b0f4-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="1b0f4-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1b0f4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1b0f4-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="1b0f4-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="1b0f4-105">Aggiungere un Controller di prodotti</span><span class="sxs-lookup"><span data-stu-id="1b0f4-105">Add a Products Controller</span></span>

<span data-ttu-id="1b0f4-106">Il controller di amministratore sia per gli utenti che dispongono dei privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="1b0f4-107">I clienti, d'altra parte, possono visualizzare i prodotti ma non è possibile creare, aggiornare o eliminarli.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="1b0f4-108">È possibile limitare facilmente l'accesso ai metodi Post, Put e Delete, lasciando aperto i metodi Get.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="1b0f4-109">Tuttavia, esaminare i dati che viene restituiti per un prodotto:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="1b0f4-110">Il `ActualCost` proprietà non deve essere visibile ai clienti.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="1b0f4-111">La soluzione consiste nel definire un *oggetto di trasferimento dati* (DTO) che include un subset di proprietà che devono essere visibili ai clienti.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="1b0f4-112">Si utilizzerà LINQ al progetto `Product` istanze per `ProductDTO` istanze.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="1b0f4-113">Aggiungere una classe denominata `ProductDTO` nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="1b0f4-114">Aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-114">Now add the controller.</span></span> <span data-ttu-id="1b0f4-115">In Esplora soluzioni, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1b0f4-116">Selezionare **Aggiungi**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="1b0f4-117">Nel **Aggiungi Controller** finestra di dialogo, nome del controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="1b0f4-118">In **modello**selezionare **controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="1b0f4-119">Sostituire tutto il contenuto nel file di origine con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="1b0f4-120">Il controller utilizza ancora la `OrdersContext` per eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="1b0f4-121">Ma anziché restituire `Product` istanze direttamente, definiamo `MapProducts` al progetto in `ProductDTO` istanze:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="1b0f4-122">Il `MapProducts` metodo restituisce un **IQueryable**, pertanto è possibile comporre il risultato con altri parametri di query.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="1b0f4-123">È possibile visualizzare nel `GetProduct` metodo che aggiunge un **dove** clausola alla query:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="1b0f4-124">Aggiungere un Controller di ordini</span><span class="sxs-lookup"><span data-stu-id="1b0f4-124">Add an Orders Controller</span></span>

<span data-ttu-id="1b0f4-125">Successivamente, aggiungere un controller che consente agli utenti di creare e visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="1b0f4-126">Si inizierà con un altro DTO.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-126">We'll start with another DTO.</span></span> <span data-ttu-id="1b0f4-127">In Esplora soluzioni, fare clic sulla cartella di modelli e aggiungere una classe denominata `OrderDTO` usare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="1b0f4-128">Aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-128">Now add the controller.</span></span> <span data-ttu-id="1b0f4-129">In Esplora soluzioni, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1b0f4-130">Selezionare **Aggiungi**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="1b0f4-131">Nel **Aggiungi Controller** finestra di dialogo, impostare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="1b0f4-132">In **nome Controller**, immettere "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="1b0f4-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="1b0f4-133">In **modello**, selezionare "Controller API con azioni di lettura/scrittura, mediante Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="1b0f4-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="1b0f4-134">In **classe modello**selezionare &quot;ordine (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="1b0f4-135">In **classe contesto dati**selezionare &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="1b0f4-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-136">Click **Add**.</span></span> <span data-ttu-id="1b0f4-137">Consente di aggiungere un file denominato OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="1b0f4-138">Successivamente, è necessario modificare l'implementazione predefinita del controller.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="1b0f4-139">Eliminare prima il `PutOrder` e `DeleteOrder` metodi.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="1b0f4-140">Per questo esempio, i clienti non è possibile modificare o eliminare gli ordini esistenti.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="1b0f4-141">In un'applicazione reale, è necessario un numero elevato di logica di back-end per gestire questi casi.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="1b0f4-142">(Ad esempio, è stato eseguito l'ordine già?)</span><span class="sxs-lookup"><span data-stu-id="1b0f4-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="1b0f4-143">Modifica il `GetOrders` per restituire solo gli ordini che appartengono all'utente:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="1b0f4-144">Modifica il `GetOrder` metodo come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="1b0f4-145">Di seguito sono le modifiche apportate al metodo:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="1b0f4-146">Il valore restituito è un `OrderDTO` istanza, invece di un `Order`.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="1b0f4-147">Quando si esegue una query del database per l'ordine, utilizziamo la [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) metodo per recuperare l'oggetto correlato `OrderDetail` e `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="1b0f4-148">Il risultato è bidimensionale utilizzando una proiezione.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="1b0f4-149">La risposta HTTP conterrà una matrice di prodotti con quantità:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="1b0f4-150">Questo formato è più semplice per i client di utilizzare il grafico di oggetto originale, che contiene le entità annidate (ordine, dettagli e prodotti).</span><span class="sxs-lookup"><span data-stu-id="1b0f4-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="1b0f4-151">L'ultimo metodo prenderla in considerazione `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="1b0f4-152">Attualmente, questo metodo accetta un `Order` istanza.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="1b0f4-153">Ma si consideri cosa accade se un client invia un corpo della richiesta simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="1b0f4-154">Si tratta di un ordine ben strutturato ed Entity Framework verrà tranquillamente inserirlo nel database.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="1b0f4-155">Ma contiene un'entità Product che non esisteva in precedenza.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="1b0f4-156">Il client appena creato un nuovo prodotto nel database.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-156">The client just created a new product in our database!</span></span> <span data-ttu-id="1b0f4-157">Questo valore sarà una novità per il reparto fullfilment ordine, quando viene visualizzato un ordine per koala orsi.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="1b0f4-158">È il morale, effettivamente attenzione i dati che si accetta una richiesta POST o PUT.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="1b0f4-159">Per evitare questo problema, modificare il `PostOrder` metodo per richiedere un `OrderDTO` istanza.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="1b0f4-160">Utilizzare il `OrderDTO` per creare il `Order`.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="1b0f4-161">Si noti che viene usato il `ProductID` e `Quantity` proprietà e se ignorano i valori che il client ha inviato per il nome del prodotto o prezzo.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="1b0f4-162">Se l'ID prodotto non è valido, violeranno il vincolo di chiave esterno nel database e l'inserimento avrà esito negativo, come previsto.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="1b0f4-163">Ecco l'intero `PostOrder` metodo:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="1b0f4-164">Aggiungere infine il **Authorize** attributo al controller:</span><span class="sxs-lookup"><span data-stu-id="1b0f4-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="1b0f4-165">Solo gli utenti registrati possono ora creare o visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="1b0f4-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1b0f4-166">[Precedente](using-web-api-with-entity-framework-part-5.md)
[Successivo](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="1b0f4-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
