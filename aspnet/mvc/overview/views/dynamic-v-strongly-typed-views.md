---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Confronto tra visualizzazioni dinamiche. Fortemente tipizzate viste | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: bd00fccc44019b2e401247de1532d2abcb5dd396
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578503"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="d0035-103">Confronto tra visualizzazioni dinamiche.</span><span class="sxs-lookup"><span data-stu-id="d0035-103">Dynamic v.</span></span> <span data-ttu-id="d0035-104">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="d0035-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="d0035-105">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="d0035-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="d0035-106">Esistono tre modi per passare le informazioni da un controller di visualizzazione ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="d0035-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="d0035-107">Come un oggetto modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="d0035-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="d0035-108">Come un tipo dinamico (con @model dinamica)</span><span class="sxs-lookup"><span data-stu-id="d0035-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="d0035-109">Usando ViewBag</span><span class="sxs-lookup"><span data-stu-id="d0035-109">Using the ViewBag</span></span>

<span data-ttu-id="d0035-110">Ho scritto un'applicazione MVC 3 Top Blog semplice per confrontare e contrapporre le visualizzazioni fortemente tipizzate e dinamiche.</span><span class="sxs-lookup"><span data-stu-id="d0035-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="d0035-111">Il controller inizia con un semplice elenco di blog:</span><span class="sxs-lookup"><span data-stu-id="d0035-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="d0035-112">Fare clic con il pulsante destro del metodo IndexNotStonglyTyped() e aggiungere una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="d0035-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="d0035-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0035-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="d0035-114">Assicurarsi che il **creare una visualizzazione fortemente tipizzata** non sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="d0035-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="d0035-115">La visualizzazione risultante non contenga molte:</span><span class="sxs-lookup"><span data-stu-id="d0035-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="d0035-116">Poiché utilizziamo un dinamico e non una visualizzazione fortemente tipizzata, non consentono di intellisense.</span><span class="sxs-lookup"><span data-stu-id="d0035-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="d0035-117">Il codice completo è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d0035-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="d0035-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d0035-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="d0035-119">Ora si aggiungerà una visualizzazione fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="d0035-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="d0035-120">Aggiungere il codice seguente al controller:</span><span class="sxs-lookup"><span data-stu-id="d0035-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="d0035-121">Si noti che è esattamente la stessa View(topBlogs) restituito; chiamare la visualizzazione non fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="d0035-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="d0035-122">Fare clic con il pulsante destro all'interno di *StonglyTypedIndex()* e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="d0035-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="d0035-123">Questa volta selezionare il **Blog** classe del modello e selezionare **elenco** come modello di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="d0035-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="d0035-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d0035-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="d0035-125">Supporto intellisense è disponibile all'interno del nuovo modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d0035-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="d0035-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d0035-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="d0035-127">Il progetto c# può essere scaricato [qui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="d0035-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
