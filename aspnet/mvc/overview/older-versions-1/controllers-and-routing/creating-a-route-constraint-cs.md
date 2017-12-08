---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Creazione di un vincolo di Route (c#) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther viene illustrato come sia possibile controllare il browser richiede route corrispondenza creando vincoli della route con espressioni regolari.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: ee83a134dcbdd1abfb296f3126a64c7d4ebab7f5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="4e8ce-103">Creazione di un vincolo di Route (c#)</span><span class="sxs-lookup"><span data-stu-id="4e8ce-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="4e8ce-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4e8ce-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4e8ce-105">In questa esercitazione, Stephen Walther viene illustrato come sia possibile controllare il browser richiede route corrispondenza creando vincoli della route con espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="4e8ce-106">Utilizzare i vincoli della route per limitare le richieste del browser che corrispondono a una route specifica.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="4e8ce-107">È possibile utilizzare un'espressione regolare per specificare un vincolo di route.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="4e8ce-108">Ad esempio, si supponga che sia stata definita la route nel listato 1 nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="4e8ce-109">**Elenco 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="4e8ce-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="4e8ce-110">Elenco 1 contiene una route denominata Product.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="4e8ce-111">È possibile utilizzare la route di prodotto per eseguire il mapping alle richieste del browser il ProductController contenuti nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="4e8ce-112">**Elenco di 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="4e8ce-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="4e8ce-113">Si noti che l'azione Details() esposto dal controller di prodotto accetta un singolo parametro denominato productId.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="4e8ce-114">Questo parametro è un parametro di tipo integer.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="4e8ce-115">La route definita nel listato 1 corrisponderà a uno degli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="4e8ce-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="4e8ce-116">/ Prodotto/23</span><span class="sxs-lookup"><span data-stu-id="4e8ce-116">/Product/23</span></span>
- <span data-ttu-id="4e8ce-117">/ Prodotto/7</span><span class="sxs-lookup"><span data-stu-id="4e8ce-117">/Product/7</span></span>

<span data-ttu-id="4e8ce-118">Purtroppo, la route corrisponde anche i seguenti URL:</span><span class="sxs-lookup"><span data-stu-id="4e8ce-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="4e8ce-119">/ Prodotti/bLa</span><span class="sxs-lookup"><span data-stu-id="4e8ce-119">/Product/blah</span></span>
- <span data-ttu-id="4e8ce-120">/ Prodotti/apple</span><span class="sxs-lookup"><span data-stu-id="4e8ce-120">/Product/apple</span></span>

<span data-ttu-id="4e8ce-121">Poiché l'azione Details() prevede un parametro intero, effettua una richiesta che contiene un valore diverso da un valore intero verrà generato un errore.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="4e8ce-122">Ad esempio, se si digita il /Product/apple URL nel browser si otterranno la pagina di errore nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="4e8ce-123">[![La finestra di dialogo Nuovo progetto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4e8ce-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="4e8ce-124">**Figura 01**: visualizzare una pagina esplosi ([fare clic per visualizzare l'immagine ingrandita](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4e8ce-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="4e8ce-125">Cosa si vuole eseguire è trovare solo gli URL che contengono un productId intero appropriato.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="4e8ce-126">È possibile utilizzare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="4e8ce-127">La route di prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo numeri interi.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="4e8ce-128">**Elenco di 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="4e8ce-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="4e8ce-129">\D+ l'espressione regolare corrisponde a uno o più numeri interi.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="4e8ce-130">Questo vincolo fa sì che la route prodotto corrispondere i seguenti URL:</span><span class="sxs-lookup"><span data-stu-id="4e8ce-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="4e8ce-131">/ Prodotto/3</span><span class="sxs-lookup"><span data-stu-id="4e8ce-131">/Product/3</span></span>
- <span data-ttu-id="4e8ce-132">/ Prodotti/8999</span><span class="sxs-lookup"><span data-stu-id="4e8ce-132">/Product/8999</span></span>

<span data-ttu-id="4e8ce-133">Ma non gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="4e8ce-133">But not the following URLs:</span></span>

- <span data-ttu-id="4e8ce-134">/ Prodotti/apple</span><span class="sxs-lookup"><span data-stu-id="4e8ce-134">/Product/apple</span></span>
- <span data-ttu-id="4e8ce-135">/ Prodotto</span><span class="sxs-lookup"><span data-stu-id="4e8ce-135">/Product</span></span>

- <span data-ttu-id="4e8ce-136">Queste richieste del browser verranno gestite da un'altra route o, se non esistono route corrispondente, un *non è stata trovata la risorsa* viene restituito l'errore.</span><span class="sxs-lookup"><span data-stu-id="4e8ce-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4e8ce-137">[Precedente](creating-custom-routes-cs.md)
[Successivo](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4e8ce-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
