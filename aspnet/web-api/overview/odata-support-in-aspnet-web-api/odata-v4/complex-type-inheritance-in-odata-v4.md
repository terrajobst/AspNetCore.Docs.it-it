---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Ereditarietà del tipo complesso in OData v4 con l'API Web ASP.NET | Microsoft Docs
author: microsoft
description: In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave). API Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 84a887b445959c4aa6d1ee372f067f93cd725d77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372057"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="ab137-104">Ereditarietà del tipo complesso in OData v4 con l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ab137-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="ab137-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ab137-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ab137-106">In base a OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), un tipo complesso può ereditare da un altro tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="ab137-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="ab137-107">(Un *complessi* tipo è un tipo strutturato senza una chiave.) API Web OData 5.3 supporta l'ereditarietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="ab137-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="ab137-108">Questo argomento illustra come creare un entity data model (EDM) con i tipi di eredità complessa.</span><span class="sxs-lookup"><span data-stu-id="ab137-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="ab137-109">Per il codice sorgente completo, vedere [esempio di ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="ab137-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ab137-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="ab137-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ab137-111">API Web OData 5.3</span><span class="sxs-lookup"><span data-stu-id="ab137-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="ab137-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="ab137-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="ab137-113">Gerarchia del modello</span><span class="sxs-lookup"><span data-stu-id="ab137-113">Model Hierarchy</span></span>

<span data-ttu-id="ab137-114">Per illustrare l'ereditarietà del tipo complesso, si userà la seguente gerarchia di classi.</span><span class="sxs-lookup"><span data-stu-id="ab137-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="ab137-115">`Shape` è un tipo complesso astratto.</span><span class="sxs-lookup"><span data-stu-id="ab137-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="ab137-116">`Rectangle`, `Triangle`, e `Circle` sono tipi complessi derivati dal `Shape`, e `RoundRectangle` deriva da `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ab137-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="ab137-117">`Window` è un tipo di entità e contiene un `Shape` istanza.</span><span class="sxs-lookup"><span data-stu-id="ab137-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="ab137-118">Di seguito sono le classi CLR che definiscono questi tipi.</span><span class="sxs-lookup"><span data-stu-id="ab137-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="ab137-119">Compilare il modello EDM</span><span class="sxs-lookup"><span data-stu-id="ab137-119">Build the EDM Model</span></span>

<span data-ttu-id="ab137-120">Per creare il modello EDM, è possibile usare **ODataConventionModelBuilder**, che viene dedotto le relazioni di ereditarietà tra i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="ab137-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="ab137-121">È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="ab137-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="ab137-122">Questo passa altro codice, ma ti offre maggiore controllo sul modello EDM.</span><span class="sxs-lookup"><span data-stu-id="ab137-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="ab137-123">I due esempi seguenti creano lo stesso schema EDM.</span><span class="sxs-lookup"><span data-stu-id="ab137-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="ab137-124">Documento di metadati</span><span class="sxs-lookup"><span data-stu-id="ab137-124">Metadata Document</span></span>

<span data-ttu-id="ab137-125">Ecco il documento di metadati OData, che mostra l'ereditarietà di tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="ab137-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="ab137-126">Il documento di metadati, è possibile osservare che:</span><span class="sxs-lookup"><span data-stu-id="ab137-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="ab137-127">Il `Shape` tipo complesso è astratto.</span><span class="sxs-lookup"><span data-stu-id="ab137-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="ab137-128">Il `Rectangle`, `Triangle`, e `Circle` tipo complesso hanno il tipo di base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="ab137-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="ab137-129">Il `RoundRectangle` tipo con il tipo di base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ab137-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="ab137-130">Il cast dei tipi complessi</span><span class="sxs-lookup"><span data-stu-id="ab137-130">Casting Complex Types</span></span>

<span data-ttu-id="ab137-131">È ora supportata l'esecuzione del cast per i tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="ab137-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="ab137-132">Ad esempio, la query seguente cast di un `Shape` a un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="ab137-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="ab137-133">Ecco il payload di risposta:</span><span class="sxs-lookup"><span data-stu-id="ab137-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
