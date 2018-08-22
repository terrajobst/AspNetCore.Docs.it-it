---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creazione di prodotti e i controller di ordine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826312"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="53b71-102">Parte 6: Creazione di prodotto e i controller di ordine</span><span class="sxs-lookup"><span data-stu-id="53b71-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="53b71-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="53b71-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="53b71-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="53b71-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="53b71-105">Aggiungere un Controller di prodotti</span><span class="sxs-lookup"><span data-stu-id="53b71-105">Add a Products Controller</span></span>

<span data-ttu-id="53b71-106">Il controller di amministrazione è per gli utenti che dispongono dei privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="53b71-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="53b71-107">I clienti, d'altra parte, possono visualizzare i prodotti ma non è possibile creare, aggiornare o eliminarli.</span><span class="sxs-lookup"><span data-stu-id="53b71-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="53b71-108">È possibile limitare facilmente l'accesso ai metodi Post, Put e Delete, lasciando i metodi Get aperto.</span><span class="sxs-lookup"><span data-stu-id="53b71-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="53b71-109">Tuttavia, esaminare i dati che viene restituiti per un prodotto:</span><span class="sxs-lookup"><span data-stu-id="53b71-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="53b71-110">Il `ActualCost` proprietà non deve essere visibile ai clienti.</span><span class="sxs-lookup"><span data-stu-id="53b71-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="53b71-111">La soluzione consiste nel definire un *oggetto di trasferimento dati* (DTO) che include un subset di proprietà che devono essere visibili ai clienti.</span><span class="sxs-lookup"><span data-stu-id="53b71-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="53b71-112">Si userà LINQ al progetto `Product` alle istanze `ProductDTO` istanze.</span><span class="sxs-lookup"><span data-stu-id="53b71-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="53b71-113">Aggiungere una classe denominata `ProductDTO` nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="53b71-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="53b71-114">A questo punto aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="53b71-114">Now add the controller.</span></span> <span data-ttu-id="53b71-115">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="53b71-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="53b71-116">Selezionare **Add**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="53b71-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="53b71-117">Nel **Aggiungi Controller** finestra di dialogo nome al controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="53b71-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="53b71-118">Sotto **modello**, selezionare **controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="53b71-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="53b71-119">Sostituire tutto il contenuto nel file di origine con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53b71-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="53b71-120">Il controller Usa ancora il `OrdersContext` eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="53b71-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="53b71-121">Ma anziché restituire `Product` istanze direttamente, chiamiamo `MapProducts` proiettare in `ProductDTO` istanze:</span><span class="sxs-lookup"><span data-stu-id="53b71-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="53b71-122">Il `MapProducts` metodo restituisce un **IQueryable**, pertanto è possibile comporre i risultati con altri parametri di query.</span><span class="sxs-lookup"><span data-stu-id="53b71-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="53b71-123">È possibile verificarlo nel `GetProduct` metodo, che aggiunge una **dove** clausola per la query:</span><span class="sxs-lookup"><span data-stu-id="53b71-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="53b71-124">Aggiungere un Controller degli ordini</span><span class="sxs-lookup"><span data-stu-id="53b71-124">Add an Orders Controller</span></span>

<span data-ttu-id="53b71-125">Successivamente, aggiungere un controller che consente agli utenti di creare e visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="53b71-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="53b71-126">Si inizierà con un altro oggetto DTO.</span><span class="sxs-lookup"><span data-stu-id="53b71-126">We'll start with another DTO.</span></span> <span data-ttu-id="53b71-127">In Esplora soluzioni fare doppio clic su cartella Models e aggiungere una classe denominata `OrderDTO` usare l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="53b71-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="53b71-128">A questo punto aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="53b71-128">Now add the controller.</span></span> <span data-ttu-id="53b71-129">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="53b71-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="53b71-130">Selezionare **Add**, quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="53b71-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="53b71-131">Nel **Aggiungi Controller** finestra di dialogo impostare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="53b71-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="53b71-132">Sotto **nome del Controller**, immettere "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="53b71-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="53b71-133">Sotto **modello**, selezionare "Controller API con azioni di lettura/scrittura, che usa Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="53b71-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="53b71-134">Sotto **classe modello**, selezionare &quot;ordine (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="53b71-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="53b71-135">Sotto **alla classe contesto dati**, selezionare &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="53b71-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="53b71-136">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="53b71-136">Click **Add**.</span></span> <span data-ttu-id="53b71-137">Verrà aggiunto un file denominato OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="53b71-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="53b71-138">Successivamente, è necessario modificare l'implementazione predefinita del controller.</span><span class="sxs-lookup"><span data-stu-id="53b71-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="53b71-139">Eliminare innanzitutto le `PutOrder` e `DeleteOrder` metodi.</span><span class="sxs-lookup"><span data-stu-id="53b71-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="53b71-140">Per questo esempio, i clienti non possono modificare o eliminare gli ordini esistenti.</span><span class="sxs-lookup"><span data-stu-id="53b71-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="53b71-141">In un'applicazione reale, è necessario un numero elevato di logica di back-end per gestire questi casi.</span><span class="sxs-lookup"><span data-stu-id="53b71-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="53b71-142">(Ad esempio, è stato eseguito l'ordine già?)</span><span class="sxs-lookup"><span data-stu-id="53b71-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="53b71-143">Modifica il `GetOrders` per restituire solo gli ordini che appartengono all'utente:</span><span class="sxs-lookup"><span data-stu-id="53b71-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="53b71-144">Modifica il `GetOrder` metodo come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="53b71-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="53b71-145">Ecco le modifiche apportate al metodo:</span><span class="sxs-lookup"><span data-stu-id="53b71-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="53b71-146">Il valore restituito è un `OrderDTO` istanza, invece di un `Order`.</span><span class="sxs-lookup"><span data-stu-id="53b71-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="53b71-147">Quando si esegue una query del database per l'ordine, usiamo il [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metodo per recuperare i relativi `OrderDetail` e `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="53b71-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="53b71-148">È rendere flat il risultato usando una proiezione.</span><span class="sxs-lookup"><span data-stu-id="53b71-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="53b71-149">La risposta HTTP contiene una matrice di prodotti con quantità:</span><span class="sxs-lookup"><span data-stu-id="53b71-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="53b71-150">Questo formato è più semplice per i client da usare rispetto l'originale oggetto grafico, che contiene entità annidate (ordine, dettagli e i prodotti).</span><span class="sxs-lookup"><span data-stu-id="53b71-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="53b71-151">Quest'ultimo metodo prenderla in considerazione `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="53b71-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="53b71-152">Al momento, questo metodo accetta un `Order` istanza.</span><span class="sxs-lookup"><span data-stu-id="53b71-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="53b71-153">Ma si consideri che cosa accade se un client invia un corpo della richiesta simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="53b71-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="53b71-154">Si tratta di un ordine ben strutturato ed Entity Framework verrà Fortunatamente inserirlo nel database.</span><span class="sxs-lookup"><span data-stu-id="53b71-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="53b71-155">Ma contiene un'entità Product che non esisteva in precedenza.</span><span class="sxs-lookup"><span data-stu-id="53b71-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="53b71-156">Il client appena creato un nuovo prodotto nel nostro database!</span><span class="sxs-lookup"><span data-stu-id="53b71-156">The client just created a new product in our database!</span></span> <span data-ttu-id="53b71-157">Questo sarà una grande sorpresa al reparto di produzione di ordine, quando viene visualizzato un ordine per orsi koala.</span><span class="sxs-lookup"><span data-stu-id="53b71-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="53b71-158">È la morale, essere realmente necessario prestare particolare attenzione i dati che si accetta una richiesta POST o PUT.</span><span class="sxs-lookup"><span data-stu-id="53b71-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="53b71-159">Per evitare questo problema, modificare il `PostOrder` metodo per richiedere un `OrderDTO` istanza.</span><span class="sxs-lookup"><span data-stu-id="53b71-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="53b71-160">Usare la `OrderDTO` per creare il `Order`.</span><span class="sxs-lookup"><span data-stu-id="53b71-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="53b71-161">Si noti che viene usato il `ProductID` e `Quantity` proprietà e ignorare tutti i valori che il client ha inviato per il nome del prodotto o prezzo.</span><span class="sxs-lookup"><span data-stu-id="53b71-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="53b71-162">Se l'ID prodotto non è valido, violeranno il vincolo di chiave esterna nel database e l'inserimento avrà esito negativo, come previsto.</span><span class="sxs-lookup"><span data-stu-id="53b71-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="53b71-163">Di seguito è riportato l'intero `PostOrder` metodo:</span><span class="sxs-lookup"><span data-stu-id="53b71-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="53b71-164">Infine, aggiungere il **Authorize** attributo al controller:</span><span class="sxs-lookup"><span data-stu-id="53b71-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="53b71-165">Solo gli utenti registrati a questo punto possono creare o visualizzare gli ordini.</span><span class="sxs-lookup"><span data-stu-id="53b71-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53b71-166">[Precedente](using-web-api-with-entity-framework-part-5.md)
> [Successivo](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="53b71-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
