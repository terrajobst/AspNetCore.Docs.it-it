---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Creazione di un vincolo di Route (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: f0dbbb880bc679da934444b87ef24d040b69e2f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828254"
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="2e9b1-103">Creazione di un vincolo di Route (c#)</span><span class="sxs-lookup"><span data-stu-id="2e9b1-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="2e9b1-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2e9b1-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2e9b1-105">In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="2e9b1-106">Vincoli di route vengono utilizzati per limitare le richieste del browser che corrispondono a una particolare route.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="2e9b1-107">È possibile usare un'espressione regolare per specificare un vincolo di route.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="2e9b1-108">Si supponga, ad esempio, che è stata definita la route nel listato 1 nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="2e9b1-109">**Listato 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="2e9b1-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="2e9b1-110">L'elenco 1 contiene una route denominata Product.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="2e9b1-111">È possibile usare la route di prodotto per eseguire il mapping alle richieste del browser il ProductController contenuta nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="2e9b1-112">**Listato 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="2e9b1-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="2e9b1-113">Si noti che l'azione Details() esposto dal controller di prodotto accetta un singolo parametro denominato productId.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="2e9b1-114">Questo parametro è un parametro integer.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="2e9b1-115">La route definita nel listato 1 corrisponderà a uno degli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e9b1-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="2e9b1-116">/ Prodotti/23</span><span class="sxs-lookup"><span data-stu-id="2e9b1-116">/Product/23</span></span>
- <span data-ttu-id="2e9b1-117">/ Prodotti/7</span><span class="sxs-lookup"><span data-stu-id="2e9b1-117">/Product/7</span></span>

<span data-ttu-id="2e9b1-118">Sfortunatamente, la route corrisponde anche agli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e9b1-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="2e9b1-119">/ Prodotti/bLa</span><span class="sxs-lookup"><span data-stu-id="2e9b1-119">/Product/blah</span></span>
- <span data-ttu-id="2e9b1-120">/ Prodotti/apple</span><span class="sxs-lookup"><span data-stu-id="2e9b1-120">/Product/apple</span></span>

<span data-ttu-id="2e9b1-121">Poiché l'azione Details() prevede un parametro intero, una richiesta che contiene un valore diverso da un valore intero verrà generato un errore.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="2e9b1-122">Ad esempio, se si digita il /Product/apple URL nel browser quindi si otterrà la pagina di errore nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="2e9b1-123">[![La finestra di dialogo Nuovo progetto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2e9b1-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="2e9b1-124">**Figura 01**: viene visualizzata una pagina explode ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2e9b1-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="2e9b1-125">Che cosa si vuole eseguire è trovare solo gli URL contenenti un productId integer appropriata.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="2e9b1-126">È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="2e9b1-127">La route di prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo numeri interi.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="2e9b1-128">**Listato 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="2e9b1-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="2e9b1-129">L'espressione regolare \d+ corrisponde a uno o più numeri interi.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="2e9b1-130">Questo vincolo fa sì che la route di prodotto in modo che corrispondano gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e9b1-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="2e9b1-131">/ Prodotto/3</span><span class="sxs-lookup"><span data-stu-id="2e9b1-131">/Product/3</span></span>
- <span data-ttu-id="2e9b1-132">/ Prodotti/8999</span><span class="sxs-lookup"><span data-stu-id="2e9b1-132">/Product/8999</span></span>

<span data-ttu-id="2e9b1-133">Ma non gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e9b1-133">But not the following URLs:</span></span>

- <span data-ttu-id="2e9b1-134">/ Prodotti/apple</span><span class="sxs-lookup"><span data-stu-id="2e9b1-134">/Product/apple</span></span>
- <span data-ttu-id="2e9b1-135">/ Prodotto</span><span class="sxs-lookup"><span data-stu-id="2e9b1-135">/Product</span></span>

- <span data-ttu-id="2e9b1-136">Queste richieste del browser verranno gestite da un'altra route o, se non esistono alcuna route corrispondente, un *la risorsa non è stata trovata* viene restituito l'errore.</span><span class="sxs-lookup"><span data-stu-id="2e9b1-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e9b1-137">[Precedente](creating-custom-routes-cs.md)
> [Successivo](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2e9b1-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
