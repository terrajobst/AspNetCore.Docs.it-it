---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "Ereditarietà del tipo complesso in OData v4 con ASP.NET Web API | Documenti Microsoft"
author: microsoft
description: "In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave). API Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="edea4-104">Ereditarietà del tipo complesso in OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="edea4-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="edea4-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="edea4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="edea4-106">In base a OData v4 [specifica](http://www.odata.org/documentation/odata-version-4-0/), un tipo complesso può ereditare da un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="edea4-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="edea4-107">(A *complesso* tipo è un tipo strutturato senza una chiave.) Web API OData 5.3 supporta l'ereditarietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="edea4-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="edea4-108">In questo argomento viene illustrato come compilare un entity data model (EDM) con tipi complessi di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="edea4-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="edea4-109">Per il codice sorgente completo, vedere [esempio ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="edea4-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="edea4-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="edea4-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="edea4-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="edea4-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="edea4-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="edea4-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="edea4-113">Gerarchia del modello</span><span class="sxs-lookup"><span data-stu-id="edea4-113">Model Hierarchy</span></span>

<span data-ttu-id="edea4-114">Per illustrare l'ereditarietà del tipo complesso, verrà utilizzata la seguente gerarchia di classi.</span><span class="sxs-lookup"><span data-stu-id="edea4-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="edea4-115">`Shape`è un tipo complesso astratto.</span><span class="sxs-lookup"><span data-stu-id="edea4-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="edea4-116">`Rectangle`, `Triangle`, e `Circle` sono tipi complessi derivati da `Shape`, e `RoundRectangle` deriva da `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="edea4-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="edea4-117">`Window`è un tipo di entità e contiene un `Shape` istanza.</span><span class="sxs-lookup"><span data-stu-id="edea4-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="edea4-118">Di seguito sono le classi CLR che definiscono questi tipi.</span><span class="sxs-lookup"><span data-stu-id="edea4-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="edea4-119">Compilare il modello EDM</span><span class="sxs-lookup"><span data-stu-id="edea4-119">Build the EDM Model</span></span>

<span data-ttu-id="edea4-120">Per creare il modello EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deriva le relazioni di ereditarietà tra i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="edea4-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="edea4-121">È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="edea4-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="edea4-122">Accetta più codice, ma garantisce un maggiore controllo su EDM.</span><span class="sxs-lookup"><span data-stu-id="edea4-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="edea4-123">I due esempi seguenti creano lo stesso schema EDM.</span><span class="sxs-lookup"><span data-stu-id="edea4-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="edea4-124">Documento di metadati</span><span class="sxs-lookup"><span data-stu-id="edea4-124">Metadata Document</span></span>

<span data-ttu-id="edea4-125">Di seguito è riportato il documento di metadati OData, che mostra ereditarietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="edea4-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="edea4-126">Nel documento di metadati, è possibile vedere che:</span><span class="sxs-lookup"><span data-stu-id="edea4-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="edea4-127">Il `Shape` tipo complesso è astratta.</span><span class="sxs-lookup"><span data-stu-id="edea4-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="edea4-128">Il `Rectangle`, `Triangle`, e `Circle` tipo complesso avere il tipo di base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="edea4-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="edea4-129">Il `RoundRectangle` tipo è di tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="edea4-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="edea4-130">Cast dei tipi complessi</span><span class="sxs-lookup"><span data-stu-id="edea4-130">Casting Complex Types</span></span>

<span data-ttu-id="edea4-131">È ora supportata l'esecuzione del cast per i tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="edea4-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="edea4-132">Ad esempio, la query seguente cast di un `Shape` per un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="edea4-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="edea4-133">Di seguito è riportato il payload di risposta:</span><span class="sxs-lookup"><span data-stu-id="edea4-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
