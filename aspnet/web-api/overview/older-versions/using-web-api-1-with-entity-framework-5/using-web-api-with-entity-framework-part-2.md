---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Creazione di modelli di dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379725"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="88f9b-102">Parte 2: Creazione di modelli di dominio</span><span class="sxs-lookup"><span data-stu-id="88f9b-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="88f9b-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="88f9b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="88f9b-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="88f9b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="88f9b-105">Aggiungere modelli</span><span class="sxs-lookup"><span data-stu-id="88f9b-105">Add Models</span></span>

<span data-ttu-id="88f9b-106">Esistono tre modi per approccio Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="88f9b-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="88f9b-107">Database-first: iniziare con un database ed Entity Framework genera il codice.</span><span class="sxs-lookup"><span data-stu-id="88f9b-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="88f9b-108">Model-first: iniziare con un modello visivo ed Entity Framework genera il codice sia il database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="88f9b-109">Code first: iniziare con il codice ed Entity Framework genera il database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="88f9b-110">Viene usato l'approccio code first, iniziamo definendo gli oggetti di dominio come oggetti poco (plain-old CLR Object).</span><span class="sxs-lookup"><span data-stu-id="88f9b-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="88f9b-111">Con l'approccio code first, gli oggetti di dominio non necessario alcun codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o di persistenza.</span><span class="sxs-lookup"><span data-stu-id="88f9b-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="88f9b-112">(In particolare, non è necessario ereditare il [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) È comunque possibile usare le annotazioni dei dati per controllare come Entity Framework crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="88f9b-113">Perché non sono dotati di qualsiasi proprietà aggiuntiva che descrivono oggetti poco [stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), sono facilmente può essere serializzati come JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="88f9b-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="88f9b-114">Tuttavia, ciò non significa che è sempre opportuno esporre direttamente i modelli Entity Framework nel client, come vedremo più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="88f9b-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="88f9b-115">Si creerà gli oggetti poco seguenti:</span><span class="sxs-lookup"><span data-stu-id="88f9b-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="88f9b-116">Prodotto</span><span class="sxs-lookup"><span data-stu-id="88f9b-116">Product</span></span>
- <span data-ttu-id="88f9b-117">Ordinamento</span><span class="sxs-lookup"><span data-stu-id="88f9b-117">Order</span></span>
- <span data-ttu-id="88f9b-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="88f9b-118">OrderDetail</span></span>

<span data-ttu-id="88f9b-119">Per creare ogni classe, fare clic sulla cartella modelli in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="88f9b-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="88f9b-120">Dal menu di scelta rapida, selezionare **Add** e quindi selezionare **classe.**</span><span class="sxs-lookup"><span data-stu-id="88f9b-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="88f9b-121">Aggiungere un `Product` classe con l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="88f9b-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="88f9b-122">Per convenzione, Entity Framework Usa il `Id` proprietà come chiave primaria e ne esegue il mapping a una colonna identity nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="88f9b-123">Quando si crea una nuova `Product` istanza, si non imposta un valore per `Id`, perché il database genera il valore.</span><span class="sxs-lookup"><span data-stu-id="88f9b-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="88f9b-124">Il **ScaffoldColumn** attributo indica a MVC ASP.NET per ignorare il `Id` proprietà durante la generazione di un form dell'editor.</span><span class="sxs-lookup"><span data-stu-id="88f9b-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="88f9b-125">Il **necessari** attributo viene usato per convalidare il modello.</span><span class="sxs-lookup"><span data-stu-id="88f9b-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="88f9b-126">Specifica che il `Name` proprietà deve essere una stringa non vuota.</span><span class="sxs-lookup"><span data-stu-id="88f9b-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="88f9b-127">Aggiungere il `Order` classe:</span><span class="sxs-lookup"><span data-stu-id="88f9b-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="88f9b-128">Aggiungere il `OrderDetail` classe:</span><span class="sxs-lookup"><span data-stu-id="88f9b-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="88f9b-129">Relazioni di chiave esterne</span><span class="sxs-lookup"><span data-stu-id="88f9b-129">Foreign Key Relations</span></span>

<span data-ttu-id="88f9b-130">Un ordine contiene molti dettagli dell'ordine e i dettagli di ciascun ordine si riferisce a un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="88f9b-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="88f9b-131">Per rappresentare le relazioni e le `OrderDetail` classe definisce le proprietà denominate `OrderId` e `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="88f9b-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="88f9b-132">Entity Framework verrà dedotto che queste proprietà rappresentano chiavi esterne e aggiungerà i vincoli di chiave esterna nel database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="88f9b-133">Il `Order` e `OrderDetail` classi includono inoltre la proprietà "spostamento", che contenga riferimenti agli oggetti correlati.</span><span class="sxs-lookup"><span data-stu-id="88f9b-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="88f9b-134">Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="88f9b-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="88f9b-135">A questo punto, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="88f9b-135">Compile the project now.</span></span> <span data-ttu-id="88f9b-136">Entity Framework Usa la reflection per individuare le proprietà dei modelli, pertanto è necessario un assembly compilato creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="88f9b-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="88f9b-137">Configurare i formattatori di Media Type</span><span class="sxs-lookup"><span data-stu-id="88f9b-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="88f9b-138">Oggetto [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando si crea il corpo della risposta HTTP API Web.</span><span class="sxs-lookup"><span data-stu-id="88f9b-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="88f9b-139">I formattatori predefiniti supportano l'output JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="88f9b-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="88f9b-140">Per impostazione predefinita, entrambi i formattatori serializza tutti gli oggetti per valore.</span><span class="sxs-lookup"><span data-stu-id="88f9b-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="88f9b-141">Serializzazione dal valore crea un problema se un oggetto grafico contiene i riferimenti circolari.</span><span class="sxs-lookup"><span data-stu-id="88f9b-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="88f9b-142">Che avviene esattamente con le `Order` e `OrderDetail` classi, perché ognuno contiene un riferimento a altro.</span><span class="sxs-lookup"><span data-stu-id="88f9b-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="88f9b-143">Il formattatore verrà seguire i riferimenti, la scrittura di ogni oggetto per valore che nelle cerchi.</span><span class="sxs-lookup"><span data-stu-id="88f9b-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="88f9b-144">Pertanto, è necessario modificare il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="88f9b-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="88f9b-145">In Esplora soluzioni espandere l'App\_avviare cartella e aprire il file WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="88f9b-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="88f9b-146">Aggiungere il codice seguente alla classe `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="88f9b-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="88f9b-147">Questo codice imposta il formattatore JSON per conservare i riferimenti all'oggetto e rimuove completamente il formattatore XML dalla pipeline.</span><span class="sxs-lookup"><span data-stu-id="88f9b-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="88f9b-148">(È possibile configurare il formattatore XML per mantenere riferimenti a oggetti, ma è un po' più, e occorre solo JSON per questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="88f9b-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="88f9b-149">Per altre informazioni, vedere [gestisce i riferimenti all'oggetto circolare](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="88f9b-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88f9b-150">[Precedente](using-web-api-with-entity-framework-part-1.md)
> [Successivo](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="88f9b-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
