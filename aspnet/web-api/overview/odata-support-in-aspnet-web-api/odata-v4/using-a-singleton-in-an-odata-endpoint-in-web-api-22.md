---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creare un Singleton in OData v4 Using Web API 2.2 | Documenti Microsoft
author: rick-anderson
description: In questo argomento viene illustrato come definire un singleton in un endpoint OData in Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508210"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="d9618-103">Creare un Singleton in OData v4 Using Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="d9618-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="d9618-104">da Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="d9618-104">by Zoe Luo</span></span>

> <span data-ttu-id="d9618-105">In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="d9618-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="d9618-106">Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenuto, che supporta WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="d9618-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="d9618-107">In questo articolo viene illustrato come definire un singleton in un endpoint OData in Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="d9618-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="d9618-108">Per informazioni su quali un singleton e come è possibile trarre vantaggio dall'utilizzo, vedere [utilizzando un singleton per definire l'entità speciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9618-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="d9618-109">Per creare un endpoint OData V4 in Web API, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d9618-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="d9618-110">Si creerà un singleton nel progetto API Web con il modello di dati seguenti:</span><span class="sxs-lookup"><span data-stu-id="d9618-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modello dati](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="d9618-112">Un singleton denominato `Umbrella` verranno definiti in base al tipo `Company`e un set denominato di entità `Employees` verranno definiti in base al tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="d9618-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="d9618-113">La soluzione usata in questa esercitazione può essere scaricata da [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="d9618-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="d9618-114">Definizione del modello di dati</span><span class="sxs-lookup"><span data-stu-id="d9618-114">Define the data model</span></span>

1. <span data-ttu-id="d9618-115">Definire i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="d9618-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="d9618-116">Generare il modello EDM in base ai tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="d9618-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="d9618-117">In questo caso, `builder.Singleton<Company>("Umbrella")` indica il generatore di modelli per creare un singleton denominato `Umbrella` nel modello EDM.</span><span class="sxs-lookup"><span data-stu-id="d9618-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="d9618-118">Metadati generati avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d9618-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="d9618-119">Dai metadati possiamo vedere che la proprietà di navigazione `Company` nel `Employees` set di entità è associata a singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="d9618-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="d9618-120">L'associazione viene eseguita automaticamente da `ODataConventionModelBuilder`, poiché solo `Umbrella` ha il `Company` tipo.</span><span class="sxs-lookup"><span data-stu-id="d9618-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="d9618-121">Se è presente qualsiasi ambiguità nel modello, è possibile utilizzare `HasSingletonBinding` associare in modo esplicito una proprietà di navigazione in un singleton; `HasSingletonBinding` ha lo stesso effetto dell'utilizzo di `Singleton` attributo nella definizione del tipo CLR:</span><span class="sxs-lookup"><span data-stu-id="d9618-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="d9618-122">Definire il controller singleton</span><span class="sxs-lookup"><span data-stu-id="d9618-122">Define the singleton controller</span></span>

<span data-ttu-id="d9618-123">Ad esempio il controller di EntitySet, il controller singleton eredita da `ODataController`, e deve essere il nome del controller singleton `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="d9618-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="d9618-124">Per gestire diversi tipi di richieste, sono necessarie azioni per essere predefinite nel controller.</span><span class="sxs-lookup"><span data-stu-id="d9618-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="d9618-125">**Attributo routing** è abilitata per impostazione predefinita in WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="d9618-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="d9618-126">Ad esempio, per definire un'azione per gestire l'esecuzione di query `Revenue` da `Company` routing degli attributi, utilizzare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d9618-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="d9618-127">Se non si è disposti definire gli attributi per ogni azione, solo per definire le azioni seguenti [convenzioni di Routing OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="d9618-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="d9618-128">Poiché una chiave non è necessaria per l'esecuzione di query singleton, le azioni definite nel controller singleton sono leggermente diverse dalle azioni definite nel controller di entityset.</span><span class="sxs-lookup"><span data-stu-id="d9618-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="d9618-129">Per riferimento, di seguito sono elencate le firme del metodo per ogni definizione di azione nel controller singleton.</span><span class="sxs-lookup"><span data-stu-id="d9618-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="d9618-130">In pratica, è sufficiente eseguire sul lato del servizio.</span><span class="sxs-lookup"><span data-stu-id="d9618-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="d9618-131">Il [progetto di esempio](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene tutto il codice per la soluzione e il client OData in cui viene illustrato come utilizzare il singleton.</span><span class="sxs-lookup"><span data-stu-id="d9618-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="d9618-132">La compilazione del client seguendo i passaggi descritti in [creare un'App Client di OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="d9618-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="d9618-133">.</span><span class="sxs-lookup"><span data-stu-id="d9618-133">.</span></span> 

<span data-ttu-id="d9618-134">*Grazie a Leo Hu per il contenuto originale di questo articolo.*</span><span class="sxs-lookup"><span data-stu-id="d9618-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
