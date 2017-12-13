---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Il contenimento OData v4 Using Web API 2.2 | Documenti Microsoft
author: rick-anderson
description: "In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="89bd0-104">Contenimento OData v4 Using Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="89bd0-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="89bd0-105">da Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="89bd0-105">by Jinfu Tan</span></span>

> <span data-ttu-id="89bd0-106">In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità.</span><span class="sxs-lookup"><span data-stu-id="89bd0-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="89bd0-107">Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenuto, che supporta WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="89bd0-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="89bd0-108">In questo argomento viene illustrato come definire un contenuto in un endpoint OData WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="89bd0-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="89bd0-109">Per ulteriori informazioni sul contenuto, vedere [contenimento proviene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="89bd0-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="89bd0-110">Per creare un endpoint OData V4 in Web API, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="89bd0-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="89bd0-111">In primo luogo, si creerà un modello di dominio di contenimento del servizio OData, utilizzando questo modello di dati:</span><span class="sxs-lookup"><span data-stu-id="89bd0-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="89bd0-113">Un account contiene molti PaymentInstruments (PI), ma non è possibile definire una set di entità per un PI.</span><span class="sxs-lookup"><span data-stu-id="89bd0-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="89bd0-114">Al contrario, l'elaborazione solo accessibili tramite un Account.</span><span class="sxs-lookup"><span data-stu-id="89bd0-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="89bd0-115">È possibile scaricare la soluzione usata in questo argomento da [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="89bd0-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="89bd0-116">La definizione del modello di dati</span><span class="sxs-lookup"><span data-stu-id="89bd0-116">Defining the data model</span></span>

1. <span data-ttu-id="89bd0-117">Definire i tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="89bd0-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="89bd0-118">Il `Contained` attributo viene utilizzato per le proprietà di navigazione di contenimento.</span><span class="sxs-lookup"><span data-stu-id="89bd0-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="89bd0-119">Generare il modello EDM in base ai tipi CLR.</span><span class="sxs-lookup"><span data-stu-id="89bd0-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="89bd0-120">Il `ODataConventionModelBuilder` gestirà la compilazione del modello EDM se il `Contained` attributo viene aggiunto alla proprietà di navigazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="89bd0-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="89bd0-121">Se la proprietà è un tipo di raccolta, un `GetCount(string NameContains)` funzione verrà inoltre creata.</span><span class="sxs-lookup"><span data-stu-id="89bd0-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="89bd0-122">Metadati generati avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="89bd0-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="89bd0-123">Il `ContainsTarget` attributo indica che la proprietà di navigazione è un contenimento.</span><span class="sxs-lookup"><span data-stu-id="89bd0-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="89bd0-124">Definire il controller di set di entità che lo contiene</span><span class="sxs-lookup"><span data-stu-id="89bd0-124">Define the containing entity set controller</span></span>

<span data-ttu-id="89bd0-125">Entità indipendenti non dispone di propri controller. l'azione è definita nel controller di set di entità che lo contiene.</span><span class="sxs-lookup"><span data-stu-id="89bd0-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="89bd0-126">In questo esempio, è un AccountsController, ma non PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="89bd0-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="89bd0-127">Se il percorso OData è 4 o più segmenti, solo attributo funzionamento del routing, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente.</span><span class="sxs-lookup"><span data-stu-id="89bd0-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="89bd0-128">In caso contrario, attributo e il routing convenzionale funziona: ad esempio, `GetPayInPIs(int key)` corrisponde `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="89bd0-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="89bd0-129">*Grazie a Leo Hu per il contenuto originale di questo articolo.*</span><span class="sxs-lookup"><span data-stu-id="89bd0-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
