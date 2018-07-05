---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usa $select, $expand e $value in ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f7e70bd76668f2af9215d57ee1cc9e7d41948c67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384764"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="98b02-102">Usa $select, $expand e $value in ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="98b02-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="98b02-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98b02-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="98b02-104">API Web 2 aggiunge il supporto per il $expand, $select $value opzioni e in OData.</span><span class="sxs-lookup"><span data-stu-id="98b02-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="98b02-105">Queste opzioni consentono a un client controllare la rappresentazione che ottiene risposta dal server.</span><span class="sxs-lookup"><span data-stu-id="98b02-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="98b02-106">**$expand** fa sì che le entità correlate possono essere incluse inline nella risposta.</span><span class="sxs-lookup"><span data-stu-id="98b02-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="98b02-107">**$select** seleziona un subset di proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="98b02-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="98b02-108">**$value** Ottiene il valore di una proprietà non elaborato.</span><span class="sxs-lookup"><span data-stu-id="98b02-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="98b02-109">Schema di esempio</span><span class="sxs-lookup"><span data-stu-id="98b02-109">Example Schema</span></span>

<span data-ttu-id="98b02-110">In questo articolo userà un servizio OData che definisce tre entità: prodotto, fornitore e categoria.</span><span class="sxs-lookup"><span data-stu-id="98b02-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="98b02-111">Ogni prodotto dispone di una categoria e un unico fornitore.</span><span class="sxs-lookup"><span data-stu-id="98b02-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="98b02-112">Di seguito sono le classi di c# che definiscono i modelli di entità:</span><span class="sxs-lookup"><span data-stu-id="98b02-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="98b02-113">Si noti che il `Product` classe definisce le proprietà di navigazione per il `Supplier` e `Category`.</span><span class="sxs-lookup"><span data-stu-id="98b02-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="98b02-114">Il `Category` classe definisce una proprietà di navigazione per i prodotti in ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="98b02-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="98b02-115">Per creare un endpoint OData per questo schema, usare lo scaffolding di Visual Studio 2013, come descritto in [creazione di un OData Endpoint nell'API Web ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="98b02-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="98b02-116">Aggiungere controller separato per prodotto, categoria e fornitore.</span><span class="sxs-lookup"><span data-stu-id="98b02-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="98b02-117">Abilitazione di $espandere e $select</span><span class="sxs-lookup"><span data-stu-id="98b02-117">Enabling $expand and $select</span></span>

<span data-ttu-id="98b02-118">In Visual Studio 2013, lo scaffolding API Web OData crea un controller che supporta automaticamente $expand e $select.</span><span class="sxs-lookup"><span data-stu-id="98b02-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="98b02-119">Per riferimento, ecco i requisiti per supportare $espandere e $select in un controller.</span><span class="sxs-lookup"><span data-stu-id="98b02-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="98b02-120">Per delle raccolte, il controller `Get` metodo deve restituire un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="98b02-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="98b02-121">Per le entità singole, restituiscono un **SingleResult&lt;T&gt;**, dove T è un **IQueryable** che contiene zero o un'entità.</span><span class="sxs-lookup"><span data-stu-id="98b02-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="98b02-122">Inoltre, decorare il `Get` metodi con il **[Queryable]** dell'attributo, come illustrato nei frammenti di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="98b02-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="98b02-123">In alternativa, chiamare **EnableQuerySupport** nel **HttpConfiguration** oggetto all'avvio.</span><span class="sxs-lookup"><span data-stu-id="98b02-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="98b02-124">(Per altre informazioni, vedere [attivazione delle opzioni di Query OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="98b02-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="98b02-125">Usa $espandere</span><span class="sxs-lookup"><span data-stu-id="98b02-125">Using $expand</span></span>

<span data-ttu-id="98b02-126">Quando si eseguono query un'entità OData o una raccolta, la risposta predefinita non include le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="98b02-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="98b02-127">Ad esempio, ecco la risposta predefinita per il set di entità categorie:</span><span class="sxs-lookup"><span data-stu-id="98b02-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="98b02-128">Come può notare, la risposta non include tutti i prodotti, anche se l'entità Category è un collegamento di navigazione di prodotti.</span><span class="sxs-lookup"><span data-stu-id="98b02-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="98b02-129">Tuttavia, il client può usare $espandere per visualizzare l'elenco dei prodotti per ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="98b02-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="98b02-130">Opzione $expand va inserito nella stringa di query della richiesta:</span><span class="sxs-lookup"><span data-stu-id="98b02-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="98b02-131">Ora il server includerà i prodotti per ogni categoria, in linea con le categorie.</span><span class="sxs-lookup"><span data-stu-id="98b02-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="98b02-132">Ecco il payload di risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="98b02-133">Si noti che ogni voce nella matrice "value" contiene un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="98b02-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="98b02-134">Il $expand opzione elenco accetta un delimitato da virgole delle proprietà di navigazione da espandere.</span><span class="sxs-lookup"><span data-stu-id="98b02-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="98b02-135">La richiesta seguente espande sia la categoria e il fornitore per un prodotto.</span><span class="sxs-lookup"><span data-stu-id="98b02-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="98b02-136">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="98b02-137">È possibile espandere più di un livello di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="98b02-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="98b02-138">L'esempio seguente include tutti i prodotti per una categoria e anche il fornitore per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="98b02-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="98b02-139">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="98b02-140">Per impostazione predefinita, API Web consente di limitare la profondità di espansione massima a 2.</span><span class="sxs-lookup"><span data-stu-id="98b02-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="98b02-141">Che impedisce il client di inviare le richieste complesse, ad esempio `$expand=Orders/OrderDetails/Product/Supplier/Region`, che potrebbe risultare inefficace per eseguire query e creare le risposte di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="98b02-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="98b02-142">Per sostituire il valore predefinito, impostare il **MaxExpansionDepth** proprietà di **[Queryable]** attributo.</span><span class="sxs-lookup"><span data-stu-id="98b02-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="98b02-143">Per altre informazioni sulla $expand opzione, vedere [Query opzione di sistema Expand ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) nella documentazione ufficiale relativa a OData.</span><span class="sxs-lookup"><span data-stu-id="98b02-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="98b02-144">Usando $select</span><span class="sxs-lookup"><span data-stu-id="98b02-144">Using $select</span></span>

<span data-ttu-id="98b02-145">L'opzione $select specifica un subset delle proprietà da includere nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="98b02-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="98b02-146">Ad esempio, per ottenere solo il nome e il prezzo di ogni prodotto, usare la query seguente:</span><span class="sxs-lookup"><span data-stu-id="98b02-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="98b02-147">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="98b02-148">È possibile combinare $select e $expand nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="98b02-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="98b02-149">Assicurarsi di includere la proprietà estesa nell'opzione $select.</span><span class="sxs-lookup"><span data-stu-id="98b02-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="98b02-150">Ad esempio, la richiesta seguente ottiene il nome del prodotto e il fornitore.</span><span class="sxs-lookup"><span data-stu-id="98b02-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="98b02-151">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="98b02-152">È anche possibile selezionare le proprietà all'interno di una proprietà espansa.</span><span class="sxs-lookup"><span data-stu-id="98b02-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="98b02-153">La richiesta seguente espande i prodotti e consente di selezionare il nome di categoria e il nome del prodotto.</span><span class="sxs-lookup"><span data-stu-id="98b02-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="98b02-154">Di seguito è riportato il corpo della risposta:</span><span class="sxs-lookup"><span data-stu-id="98b02-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="98b02-155">Per altre informazioni sull'opzione $select, vedere [selezionare l'opzione di Query di sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) nella documentazione ufficiale relativa a OData.</span><span class="sxs-lookup"><span data-stu-id="98b02-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="98b02-156">Recupero di singole proprietà di un'entità ($value)</span><span class="sxs-lookup"><span data-stu-id="98b02-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="98b02-157">Esistono due modi per ottenere una singola proprietà da un'entità di un client OData.</span><span class="sxs-lookup"><span data-stu-id="98b02-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="98b02-158">Il client può ottenere il valore in formato OData oppure ottenere il valore non elaborato della proprietà.</span><span class="sxs-lookup"><span data-stu-id="98b02-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="98b02-159">La richiesta seguente ottiene una proprietà nel formato OData.</span><span class="sxs-lookup"><span data-stu-id="98b02-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="98b02-160">Ecco un esempio di risposta in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="98b02-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="98b02-161">Per ottenere il valore non elaborato della proprietà, aggiungere $value all'URI:</span><span class="sxs-lookup"><span data-stu-id="98b02-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="98b02-162">Ecco la risposta.</span><span class="sxs-lookup"><span data-stu-id="98b02-162">Here is the response.</span></span> <span data-ttu-id="98b02-163">Si noti che il tipo di contenuto "text/plain", non è JSON.</span><span class="sxs-lookup"><span data-stu-id="98b02-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="98b02-164">Per supportare queste query in controller di OData, aggiungere un metodo denominato `GetProperty`, dove `Property` è il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="98b02-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="98b02-165">Ad esempio, il metodo da ottenere la proprietà Name deve essere denominato `GetName`.</span><span class="sxs-lookup"><span data-stu-id="98b02-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="98b02-166">Il metodo deve restituire il valore di tale proprietà:</span><span class="sxs-lookup"><span data-stu-id="98b02-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
