---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Supporto Entity Relations in OData v3 con API Web 2 | Microsoft Docs
author: MikeWasson
description: 'La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere gli autori di; i prodotti hanno fornitori. Utilizzo di OData, i client è possono navigare nei...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: f78b5cf36789032f90d3d073698f7a439507277f
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206861"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="6b561-104">Supporto Entity Relations in OData v3 con API Web 2</span><span class="sxs-lookup"><span data-stu-id="6b561-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="6b561-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6b561-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6b561-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="6b561-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="6b561-107">La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere gli autori di; i prodotti hanno fornitori.</span><span class="sxs-lookup"><span data-stu-id="6b561-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="6b561-108">Utilizzando OData, i client possono passare tramite relazioni tra entità.</span><span class="sxs-lookup"><span data-stu-id="6b561-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="6b561-109">Dato un prodotto, è possibile trovare il fornitore.</span><span class="sxs-lookup"><span data-stu-id="6b561-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="6b561-110">È anche possibile creare o rimuovere le relazioni.</span><span class="sxs-lookup"><span data-stu-id="6b561-110">You can also create or remove relationships.</span></span> <span data-ttu-id="6b561-111">Ad esempio, è possibile impostare il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="6b561-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="6b561-112">Questa esercitazione illustra come supportare queste operazioni nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b561-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="6b561-113">L'esercitazione si basa sull'esercitazione [creazione di un Endpoint OData v3 con API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6b561-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6b561-114">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6b561-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6b561-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="6b561-115">Web API 2</span></span>
> - <span data-ttu-id="6b561-116">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="6b561-116">OData Version 3</span></span>
> - <span data-ttu-id="6b561-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6b561-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="6b561-118">Aggiungere un'entità Supplier</span><span class="sxs-lookup"><span data-stu-id="6b561-118">Add a Supplier Entity</span></span>

<span data-ttu-id="6b561-119">Prima di tutto è necessario aggiungere un nuovo tipo di entità al nostro feed OData.</span><span class="sxs-lookup"><span data-stu-id="6b561-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="6b561-120">Si aggiungerà un `Supplier` classe.</span><span class="sxs-lookup"><span data-stu-id="6b561-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="6b561-121">Questa classe viene utilizzata una stringa per la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="6b561-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="6b561-122">In pratica, che potrebbero essere meno comune rispetto all'uso di una chiave integer.</span><span class="sxs-lookup"><span data-stu-id="6b561-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="6b561-123">Ma vale la pena visualizzati come OData gestisce altri tipi di chiave oltre ai numeri interi.</span><span class="sxs-lookup"><span data-stu-id="6b561-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="6b561-124">Successivamente, si creerà una relazione mediante l'aggiunta di un `Supplier` proprietà per il `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="6b561-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="6b561-125">Aggiungere un nuovo **DbSet** per il `ProductServiceContext` classe, in modo che Entity Framework includerà il `Supplier` tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="6b561-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="6b561-126">In WebApiConfig.cs aggiungere un'entità "Fornitori" al modello EDM:</span><span class="sxs-lookup"><span data-stu-id="6b561-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="6b561-127">Proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="6b561-127">Navigation Properties</span></span>

<span data-ttu-id="6b561-128">Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="6b561-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="6b561-129">Di seguito "Fornitore" è una proprietà di navigazione nel `Product` tipo.</span><span class="sxs-lookup"><span data-stu-id="6b561-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="6b561-130">In questo caso, `Supplier` fa riferimento a un singolo elemento, ma una navigazione può inoltre restituire una raccolta (relazione uno-a-molti o molti-a-molti).</span><span class="sxs-lookup"><span data-stu-id="6b561-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="6b561-131">Per supportare questa richiesta, aggiungere il metodo seguente al `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="6b561-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="6b561-132">Il *chiave* parametro è la chiave del prodotto.</span><span class="sxs-lookup"><span data-stu-id="6b561-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="6b561-133">Il metodo restituisce l'entità correlata&#8212;in questo caso, un `Supplier` istanza.</span><span class="sxs-lookup"><span data-stu-id="6b561-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="6b561-134">Il nome del metodo e il nome di parametro sono entrambi importanti.</span><span class="sxs-lookup"><span data-stu-id="6b561-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="6b561-135">In generale, se la proprietà di navigazione è denominata "X", è necessario aggiungere un metodo denominato "GetX".</span><span class="sxs-lookup"><span data-stu-id="6b561-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="6b561-136">Il metodo deve accettare un parametro denominato "*chiave*" che corrisponde al tipo di dati della chiave dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="6b561-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="6b561-137">È anche importante includere la **[FromOdataUri]** attributo il *chiave* parametro.</span><span class="sxs-lookup"><span data-stu-id="6b561-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="6b561-138">Questo attributo indica a Web API da usare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b561-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="6b561-139">Creazione ed eliminazione di collegamenti</span><span class="sxs-lookup"><span data-stu-id="6b561-139">Creating and Deleting Links</span></span>

<span data-ttu-id="6b561-140">OData supporta la creazione o la rimozione delle relazioni tra due entità.</span><span class="sxs-lookup"><span data-stu-id="6b561-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="6b561-141">Nella terminologia di OData, la relazione è un "collegamento".</span><span class="sxs-lookup"><span data-stu-id="6b561-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="6b561-142">Ogni collegamento dispone di un URI con il modulo *entity*/$links /*entità*.</span><span class="sxs-lookup"><span data-stu-id="6b561-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="6b561-143">Ad esempio, il collegamento dal prodotto al fornitore aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6b561-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="6b561-144">Per creare un nuovo collegamento, il client invia una richiesta POST per l'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="6b561-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="6b561-145">Il corpo della richiesta è l'URI dell'entità di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6b561-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="6b561-146">Si supponga, ad esempio, che è un fornitore con la chiave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="6b561-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="6b561-147">Per creare un collegamento da "Product(1)" a "Supplier('CTSO')", il client invia una richiesta simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6b561-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="6b561-148">Per eliminare un collegamento, il client invia una richiesta di eliminazione per l'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="6b561-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="6b561-149">**Creazione di collegamenti**</span><span class="sxs-lookup"><span data-stu-id="6b561-149">**Creating Links**</span></span>

<span data-ttu-id="6b561-150">Per consentire a un client creare collegamenti fornitore del prodotto, aggiungere il codice seguente per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="6b561-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="6b561-151">Questo metodo accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="6b561-151">This method takes three parameters:</span></span>

- <span data-ttu-id="6b561-152">*chiave*: la chiave per l'entità padre (prodotto)</span><span class="sxs-lookup"><span data-stu-id="6b561-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="6b561-153">*navigationProperty*: il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="6b561-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="6b561-154">In questo esempio, la proprietà di navigazione solo valido è "Fornitore a".</span><span class="sxs-lookup"><span data-stu-id="6b561-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="6b561-155">*collegamento*: l'URI di OData dell'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="6b561-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="6b561-156">Questo valore viene ricavato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b561-156">This value is taken from the request body.</span></span> <span data-ttu-id="6b561-157">Ad esempio, il collegamento URI potrebbe essere "`http://localhost/odata/Suppliers('CTSO')`, vale a dire il fornitore con ID = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="6b561-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="6b561-158">Il metodo Usa il collegamento per cercare il fornitore.</span><span class="sxs-lookup"><span data-stu-id="6b561-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="6b561-159">Se il fornitore corrisponda viene trovato, il metodo imposta il `Product.Supplier` proprietà e Salva il risultato nel database.</span><span class="sxs-lookup"><span data-stu-id="6b561-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="6b561-160">La parte più difficile consiste nell'analizzare l'URI del collegamento.</span><span class="sxs-lookup"><span data-stu-id="6b561-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="6b561-161">In pratica, è necessario simulare il risultato dell'invio di una richiesta GET all'URI.</span><span class="sxs-lookup"><span data-stu-id="6b561-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="6b561-162">Il metodo helper seguente viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="6b561-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="6b561-163">Il metodo richiama il processo di routing di API Web e consente di ottenere un **ODataPath** istanza che rappresenta il percorso OData analizzato.</span><span class="sxs-lookup"><span data-stu-id="6b561-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="6b561-164">Per un URI di collegamento, uno dei segmenti deve essere la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="6b561-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="6b561-165">(Caso contrario, il client ha inviato un URI non valido.)</span><span class="sxs-lookup"><span data-stu-id="6b561-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="6b561-166">**L'eliminazione di collegamenti**</span><span class="sxs-lookup"><span data-stu-id="6b561-166">**Deleting Links**</span></span>

<span data-ttu-id="6b561-167">Per eliminare un collegamento, aggiungere il codice seguente per il `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="6b561-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="6b561-168">In questo esempio, la proprietà di navigazione è un singolo `Supplier` entità.</span><span class="sxs-lookup"><span data-stu-id="6b561-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="6b561-169">Se la proprietà di navigazione è una raccolta, l'URI per eliminare un collegamento deve includere una chiave per l'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="6b561-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="6b561-170">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6b561-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="6b561-171">Questa richiesta rimuove ordine 1 dal cliente 1.</span><span class="sxs-lookup"><span data-stu-id="6b561-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="6b561-172">In questo caso, il metodo DeleteLink possono essere utilizzati avrà la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="6b561-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="6b561-173">Il *relatedKey* parametro fornisce la chiave per l'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="6b561-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="6b561-174">Pertanto nel `DeleteLink` metodo, per cercare l'entità primaria per il *chiave* parametro, trovare l'entità correlata dal *relatedKey* parametro e quindi rimuovere l'associazione.</span><span class="sxs-lookup"><span data-stu-id="6b561-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="6b561-175">A seconda del modello di dati, potrebbe essere necessario implementare entrambe le versioni di `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="6b561-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="6b561-176">API Web chiamerà la versione corretta in base all'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b561-176">Web API will call the correct version based on the request URI.</span></span>
