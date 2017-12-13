---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: V dinamico. Fortemente tipizzate viste | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="93878-103">V dinamico.</span><span class="sxs-lookup"><span data-stu-id="93878-103">Dynamic v.</span></span> <span data-ttu-id="93878-104">Visualizzazioni fortemente tipizzate</span><span class="sxs-lookup"><span data-stu-id="93878-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="93878-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="93878-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="93878-106">Esistono tre modi per passare informazioni da un controller a una vista in ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="93878-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="93878-107">Come un oggetto modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="93878-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="93878-108">Come un tipo dinamico (con @model dinamica)</span><span class="sxs-lookup"><span data-stu-id="93878-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="93878-109">Utilizzando ViewBag</span><span class="sxs-lookup"><span data-stu-id="93878-109">Using the ViewBag</span></span>

<span data-ttu-id="93878-110">Ho scritto una semplice applicazione MVC 3 Top Blog per mettere a confronto le visualizzazioni fortemente tipizzate e dinamiche.</span><span class="sxs-lookup"><span data-stu-id="93878-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="93878-111">Il controller viene avviata con un semplice elenco di blog:</span><span class="sxs-lookup"><span data-stu-id="93878-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="93878-112">Fare clic con il metodo IndexNotStonglyTyped() e aggiungere una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="93878-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="93878-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="93878-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="93878-114">Verificare che il **creare una visualizzazione fortemente tipizzata** non sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="93878-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="93878-115">La visualizzazione risultante non contenga molte:</span><span class="sxs-lookup"><span data-stu-id="93878-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="93878-116">Poiché si sta usando un dinamico e non una visualizzazione fortemente tipizzata, non Aiutaci a intellisense.</span><span class="sxs-lookup"><span data-stu-id="93878-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="93878-117">Il codice completo è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="93878-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="93878-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="93878-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="93878-119">A questo punto verrà aggiunto una visualizzazione fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="93878-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="93878-120">Aggiungere il codice seguente al controller:</span><span class="sxs-lookup"><span data-stu-id="93878-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="93878-121">Si noti che è esattamente la stessa View(topBlogs) restituito; chiamare della vista non fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="93878-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="93878-122">Fare clic con il pulsante destro all'interno di *StonglyTypedIndex()* e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="93878-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="93878-123">Questa volta selezionare il **Blog** classe del modello e selezionare **elenco** come il modello di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="93878-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="93878-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="93878-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="93878-125">All'interno del nuovo modello di visualizzazione è il supporto intellisense.</span><span class="sxs-lookup"><span data-stu-id="93878-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="93878-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="93878-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="93878-127">Può essere scaricato il progetto c# [qui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="93878-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
