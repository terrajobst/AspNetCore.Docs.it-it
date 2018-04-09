---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Creazione dei modelli di dominio | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="255ed-102">Parte 2: Creazione dei modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="255ed-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="255ed-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="255ed-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="255ed-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="255ed-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="255ed-105">Aggiungere modelli</span><span class="sxs-lookup"><span data-stu-id="255ed-105">Add Models</span></span>

<span data-ttu-id="255ed-106">Esistono tre modi per l'approccio Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="255ed-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="255ed-107">Database-first: iniziare con un database e di Entity Framework genera il codice.</span><span class="sxs-lookup"><span data-stu-id="255ed-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="255ed-108">Model-first: iniziare con un modello visivo ed Entity Framework genera il codice sia il database.</span><span class="sxs-lookup"><span data-stu-id="255ed-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="255ed-109">Prima di codice: si inizia con il codice ed Entity Framework genera il database.</span><span class="sxs-lookup"><span data-stu-id="255ed-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="255ed-110">Viene usato l'approccio incentrato codice, in modo da iniziare definendo gli oggetti di dominio come POCOs (oggetti CLR normale precedente).</span><span class="sxs-lookup"><span data-stu-id="255ed-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="255ed-111">Con l'approccio incentrato codice, gli oggetti di dominio non necessario codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o di persistenza.</span><span class="sxs-lookup"><span data-stu-id="255ed-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="255ed-112">(In particolare, non è necessario ereditare la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) È comunque possibile utilizzare le annotazioni dei dati per controllare come Entity Framework crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="255ed-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="255ed-113">Poiché non sono dotati di qualsiasi proprietà aggiuntive che descrivono POCOs [stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), può facilmente essere serializzati in JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="255ed-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="255ed-114">Tuttavia, non implica che è sempre opportuno esporre i modelli di Entity Framework direttamente al client, come vedremo più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="255ed-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="255ed-115">Verrà creata la POCOs seguenti:</span><span class="sxs-lookup"><span data-stu-id="255ed-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="255ed-116">Prodotto</span><span class="sxs-lookup"><span data-stu-id="255ed-116">Product</span></span>
- <span data-ttu-id="255ed-117">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="255ed-117">Order</span></span>
- <span data-ttu-id="255ed-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="255ed-118">OrderDetail</span></span>

<span data-ttu-id="255ed-119">Per creare ogni classe, fare clic sulla cartella modelli in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="255ed-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="255ed-120">Dal menu di scelta rapida, selezionare **Add** e quindi selezionare **classe.**</span><span class="sxs-lookup"><span data-stu-id="255ed-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="255ed-121">Aggiungere un `Product` classe con l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="255ed-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="255ed-122">Per convenzione, Entity Framework Usa il `Id` proprietà come chiave primaria e ne esegue il mapping a una colonna identity nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="255ed-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="255ed-123">Quando si crea un nuovo `Product` istanza, non impostare un valore per `Id`, perché il database genera il valore.</span><span class="sxs-lookup"><span data-stu-id="255ed-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="255ed-124">Il **ScaffoldColumn** attributo indica a ASP.NET MVC per ignorare la `Id` proprietà durante la generazione di un editor form.</span><span class="sxs-lookup"><span data-stu-id="255ed-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="255ed-125">Il **necessari** attributo viene utilizzato per convalidare il modello.</span><span class="sxs-lookup"><span data-stu-id="255ed-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="255ed-126">Specifica che il `Name` proprietà deve essere una stringa non vuota.</span><span class="sxs-lookup"><span data-stu-id="255ed-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="255ed-127">Aggiungere la `Order` classe:</span><span class="sxs-lookup"><span data-stu-id="255ed-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="255ed-128">Aggiungere la `OrderDetail` classe:</span><span class="sxs-lookup"><span data-stu-id="255ed-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="255ed-129">Relazioni di chiave esterne</span><span class="sxs-lookup"><span data-stu-id="255ed-129">Foreign Key Relations</span></span>

<span data-ttu-id="255ed-130">Un ordine contiene molti dettagli dell'ordine e i dettagli di ciascun ordine si riferisce a un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="255ed-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="255ed-131">Per rappresentare queste relazioni, la `OrderDetail` classe definisce le proprietà denominate `OrderId` e `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="255ed-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="255ed-132">Entity Framework dedurrà queste proprietà rappresentano le chiavi esterne che aggiungerà i vincoli di chiave esterna nel database.</span><span class="sxs-lookup"><span data-stu-id="255ed-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="255ed-133">Il `Order` e `OrderDetail` classi includono inoltre la proprietà "spostamento", che contengono riferimenti agli oggetti correlati.</span><span class="sxs-lookup"><span data-stu-id="255ed-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="255ed-134">Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="255ed-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="255ed-135">Compilare il progetto ora.</span><span class="sxs-lookup"><span data-stu-id="255ed-135">Compile the project now.</span></span> <span data-ttu-id="255ed-136">Entity Framework utilizza la reflection per individuare le proprietà dei modelli, pertanto richiede un assembly compilato creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="255ed-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="255ed-137">Configurare i formattatori di Media Type</span><span class="sxs-lookup"><span data-stu-id="255ed-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="255ed-138">Oggetto [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando l'API Web scrive il corpo della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="255ed-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="255ed-139">I formattatori predefiniti supportano JSON e XML di output.</span><span class="sxs-lookup"><span data-stu-id="255ed-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="255ed-140">Per impostazione predefinita, entrambi i formattatori serializzare tutti gli oggetti per valore.</span><span class="sxs-lookup"><span data-stu-id="255ed-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="255ed-141">Serializzazione da valore costituisce un problema se un oggetto grafico contiene riferimenti circolari.</span><span class="sxs-lookup"><span data-stu-id="255ed-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="255ed-142">Che è esattamente il caso di `Order` e `OrderDetail` classi, perché ogni contiene un riferimento a altro.</span><span class="sxs-lookup"><span data-stu-id="255ed-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="255ed-143">Il formattatore verrà seguire i riferimenti, la scrittura di ogni oggetto per valore e anche in cerchi.</span><span class="sxs-lookup"><span data-stu-id="255ed-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="255ed-144">Pertanto, è necessario modificare il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="255ed-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="255ed-145">In Esplora soluzioni, espandere l'applicazione\_avviare cartella e aprire il file denominato WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="255ed-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="255ed-146">Aggiungere il codice seguente alla classe `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="255ed-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="255ed-147">Questo codice imposta il formattatore JSON per mantenere i riferimenti all'oggetto e rimuove il formattatore XML dalla pipeline completamente.</span><span class="sxs-lookup"><span data-stu-id="255ed-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="255ed-148">(È possibile configurare il formattatore XML per mantenere i riferimenti agli oggetti, ma è più lunga e dobbiamo solo JSON per questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="255ed-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="255ed-149">Per ulteriori informazioni, vedere [la gestione di riferimenti circolari oggetto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="255ed-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="255ed-150">[Precedente](using-web-api-with-entity-framework-part-1.md)
> [Successivo](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="255ed-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
