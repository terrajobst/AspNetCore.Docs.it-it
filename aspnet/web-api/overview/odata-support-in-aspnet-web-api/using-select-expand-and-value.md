---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Utilizza $select, $expand e in ASP.NET Web API 2 OData $value | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="cc185-102">Utilizza $select, $expand e $value in ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="cc185-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="cc185-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cc185-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cc185-104">Web API 2 aggiunge il supporto per i $expand, $select e opzioni $value OData.</span><span class="sxs-lookup"><span data-stu-id="cc185-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="cc185-105">Queste opzioni consentono di controllare la rappresentazione che ottiene dal server di un client.</span><span class="sxs-lookup"><span data-stu-id="cc185-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="cc185-106">**$expand** fa sì che le entità correlate essere incluse inline nella risposta.</span><span class="sxs-lookup"><span data-stu-id="cc185-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="cc185-107">**$select** seleziona un subset di proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="cc185-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="cc185-108">**$value** Ottiene il valore di una proprietà non elaborato.</span><span class="sxs-lookup"><span data-stu-id="cc185-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="cc185-109">Nello Schema di esempio</span><span class="sxs-lookup"><span data-stu-id="cc185-109">Example Schema</span></span>

<span data-ttu-id="cc185-110">Per questo articolo, si utilizzerà un servizio OData che definisce tre entità: prodotto, fornitore e la categoria.</span><span class="sxs-lookup"><span data-stu-id="cc185-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="cc185-111">Ogni prodotto dispone di una categoria e un fornitore.</span><span class="sxs-lookup"><span data-stu-id="cc185-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="cc185-112">Di seguito sono le classi c# che definiscono i modelli di entità:</span><span class="sxs-lookup"><span data-stu-id="cc185-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="cc185-113">Si noti che il `Product` classe definisce le proprietà di navigazione per il `Supplier` e `Category`.</span><span class="sxs-lookup"><span data-stu-id="cc185-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="cc185-114">La `Category` classe definisce una proprietà di navigazione per i prodotti in ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="cc185-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="cc185-115">Per creare un endpoint OData per questo schema, usare lo scaffolding di Visual Studio 2013, come descritto in [creazione di un OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="cc185-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="cc185-116">Aggiungere controller separato per prodotto, categoria e fornitore.</span><span class="sxs-lookup"><span data-stu-id="cc185-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="cc185-117">Abilitazione di $espandere e $select</span><span class="sxs-lookup"><span data-stu-id="cc185-117">Enabling $expand and $select</span></span>

<span data-ttu-id="cc185-118">In Visual Studio 2013, lo scaffolding di Web API OData viene creato un controller che automaticamente supporta $expand e $select.</span><span class="sxs-lookup"><span data-stu-id="cc185-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="cc185-119">Per riferimento, di seguito sono i requisiti per supportare $espandere e $select in un controller.</span><span class="sxs-lookup"><span data-stu-id="cc185-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="cc185-120">Per delle raccolte, il controller `Get` metodo deve restituire un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="cc185-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="cc185-121">Per le entità singole, restituiscono un **SingleResult&lt;T&gt;**, dove T è un **IQueryable** che contiene zero o un'entità.</span><span class="sxs-lookup"><span data-stu-id="cc185-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="cc185-122">Anche decorare il `Get` metodi con il **[Queryable]** attributo, come illustrato in frammenti di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="cc185-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="cc185-123">In alternativa, chiamare **EnableQuerySupport** sul **HttpConfiguration** oggetto all'avvio.</span><span class="sxs-lookup"><span data-stu-id="cc185-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="cc185-124">(Per ulteriori informazioni, vedere [attivazione delle opzioni di Query OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="cc185-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="cc185-125">Usando $espandere</span><span class="sxs-lookup"><span data-stu-id="cc185-125">Using $expand</span></span>

<span data-ttu-id="cc185-126">Quando si eseguono query un'entità di OData o una raccolta, la risposta predefinita non include le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="cc185-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="cc185-127">Ad esempio, ecco la risposta predefinita per il set di entità categorie:</span><span class="sxs-lookup"><span data-stu-id="cc185-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="cc185-128">Come si può notare, la risposta non include tutti i prodotti, anche se l'entità Category è un collegamento di navigazione di prodotti.</span><span class="sxs-lookup"><span data-stu-id="cc185-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="cc185-129">Tuttavia, il client può usare $espandere per ottenere l'elenco dei prodotti per ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="cc185-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="cc185-130">Opzione $expand va inserito nella stringa di query della richiesta:</span><span class="sxs-lookup"><span data-stu-id="cc185-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="cc185-131">Ora il server includerà i prodotti per ogni categoria, in linea con le categorie.</span><span class="sxs-lookup"><span data-stu-id="cc185-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="cc185-132">Di seguito è riportato il payload di risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="cc185-133">Si noti che ogni voce nella matrice di "value" contiene un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="cc185-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="cc185-134">I $expand opzione accetta separati da virgola elenco di proprietà di navigazione da espandere.</span><span class="sxs-lookup"><span data-stu-id="cc185-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="cc185-135">La richiesta seguente si espande la categoria e il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="cc185-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="cc185-136">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="cc185-137">È possibile espandere più di un livello di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cc185-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="cc185-138">Nell'esempio seguente include tutti i prodotti per una categoria e il fornitore per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="cc185-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="cc185-139">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="cc185-140">Per impostazione predefinita, l'API Web limita la profondità di espansione massima a 2.</span><span class="sxs-lookup"><span data-stu-id="cc185-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="cc185-141">Che impedisce al client di inviare richieste complesse, quali `$expand=Orders/OrderDetails/Product/Supplier/Region`, che potrebbe risultare inefficace per eseguire query e creare le risposte di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="cc185-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="cc185-142">Per ignorare il valore predefinito, impostare il **MaxExpansionDepth** proprietà il **[Queryable]** attributo.</span><span class="sxs-lookup"><span data-stu-id="cc185-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="cc185-143">Per ulteriori informazioni sulla $expand opzione, vedere [sistema opzione Query Expand ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) nella documentazione ufficiale di OData.</span><span class="sxs-lookup"><span data-stu-id="cc185-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="cc185-144">Utilizzando $select</span><span class="sxs-lookup"><span data-stu-id="cc185-144">Using $select</span></span>

<span data-ttu-id="cc185-145">L'opzione $select specifica un subset di proprietà da includere nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="cc185-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="cc185-146">Ad esempio, per ottenere solo il nome e il prezzo di ogni prodotto, usare la query seguente:</span><span class="sxs-lookup"><span data-stu-id="cc185-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="cc185-147">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="cc185-148">È possibile combinare $select e $expand nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="cc185-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="cc185-149">Assicurarsi di includere la proprietà estesa nell'opzione $select.</span><span class="sxs-lookup"><span data-stu-id="cc185-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="cc185-150">Ad esempio, la richiesta seguente ottiene il nome del prodotto e il fornitore.</span><span class="sxs-lookup"><span data-stu-id="cc185-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="cc185-151">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="cc185-152">È anche possibile selezionare le proprietà all'interno di una proprietà espansa.</span><span class="sxs-lookup"><span data-stu-id="cc185-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="cc185-153">La richiesta seguente espande i prodotti e seleziona il nome di categoria e il nome di prodotto.</span><span class="sxs-lookup"><span data-stu-id="cc185-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="cc185-154">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="cc185-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="cc185-155">Per ulteriori informazioni sull'opzione $select, vedere [selezionare l'opzione di Query di sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) nella documentazione ufficiale di OData.</span><span class="sxs-lookup"><span data-stu-id="cc185-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="cc185-156">Recupero di singole proprietà di un'entità ($value)</span><span class="sxs-lookup"><span data-stu-id="cc185-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="cc185-157">Esistono due modi per un client OData per ottenere una singola proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="cc185-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="cc185-158">Il client può ottenere il valore in formato OData, oppure ottenere il valore della proprietà non elaborato.</span><span class="sxs-lookup"><span data-stu-id="cc185-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="cc185-159">La richiesta seguente ottiene una proprietà in formato OData.</span><span class="sxs-lookup"><span data-stu-id="cc185-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="cc185-160">Ecco un esempio di risposta in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="cc185-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="cc185-161">Per ottenere il valore della proprietà non elaborato, aggiungere all'URI $value:</span><span class="sxs-lookup"><span data-stu-id="cc185-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="cc185-162">Di seguito è riportata la risposta.</span><span class="sxs-lookup"><span data-stu-id="cc185-162">Here is the response.</span></span> <span data-ttu-id="cc185-163">Si noti che il tipo di contenuto "text/plain", non è JSON.</span><span class="sxs-lookup"><span data-stu-id="cc185-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="cc185-164">Per supportare le query nel controller OData, aggiungere un metodo denominato `GetProperty`, dove `Property` è il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cc185-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="cc185-165">Ad esempio, verrebbe denominato il metodo per ottenere la proprietà nome `GetName`.</span><span class="sxs-lookup"><span data-stu-id="cc185-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="cc185-166">Il metodo deve restituire il valore della proprietà:</span><span class="sxs-lookup"><span data-stu-id="cc185-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
