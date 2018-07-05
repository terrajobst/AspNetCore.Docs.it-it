---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Creazione di un vincolo di Route personalizzato (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Abbiamo implementato una semplice personalizzato vincolo che impedisce a una route corrispondente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: cc30cfd49402b10274e22eb27abba73329811b7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385495"
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="e358f-104">Creazione di un vincolo di Route personalizzato (VB)</span><span class="sxs-lookup"><span data-stu-id="e358f-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="e358f-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e358f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e358f-106">Stephen Walther viene illustrato come creare un vincolo di route personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e358f-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="e358f-107">Si implementa un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.</span><span class="sxs-lookup"><span data-stu-id="e358f-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="e358f-108">L'obiettivo di questa esercitazione è dimostrare come è possibile creare un vincolo di route personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e358f-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="e358f-109">Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="e358f-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="e358f-110">In questa esercitazione viene creato un vincolo di route Localhost.</span><span class="sxs-lookup"><span data-stu-id="e358f-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="e358f-111">Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale.</span><span class="sxs-lookup"><span data-stu-id="e358f-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="e358f-112">Richieste remote da attraverso la rete Internet non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="e358f-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="e358f-113">Si implementa un vincolo di route personalizzate implementando l'interfaccia IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="e358f-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="e358f-114">Si tratta di un'interfaccia estremamente semplice che descrive un singolo metodo:</span><span class="sxs-lookup"><span data-stu-id="e358f-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="e358f-115">Il metodo restituisce un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="e358f-115">The method returns a Boolean value.</span></span> <span data-ttu-id="e358f-116">Se si restituisce False, la route associata con il vincolo non corrisponde alla richiesta del browser.</span><span class="sxs-lookup"><span data-stu-id="e358f-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="e358f-117">Il vincolo Localhost è contenuto nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="e358f-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="e358f-118">**Listato 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="e358f-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="e358f-119">Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="e358f-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="e358f-120">Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta è lo stesso indirizzo IP del server.</span><span class="sxs-lookup"><span data-stu-id="e358f-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="e358f-121">Si usa vincolo personalizzato all'interno di una route definita nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="e358f-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="e358f-122">Il file Global. asax nel listato 2 Usa il vincolo di Localhost per impedire agli utenti di richiedere una pagina di amministrazione, a meno che non effettuano la richiesta dal server locale.</span><span class="sxs-lookup"><span data-stu-id="e358f-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="e358f-123">Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.</span><span class="sxs-lookup"><span data-stu-id="e358f-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="e358f-124">**Listato 2 - Global. asax**</span><span class="sxs-lookup"><span data-stu-id="e358f-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="e358f-125">Il vincolo Localhost viene usato nella definizione della route Admin.</span><span class="sxs-lookup"><span data-stu-id="e358f-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="e358f-126">Questa route non corrispondere a una richiesta del browser remoto.</span><span class="sxs-lookup"><span data-stu-id="e358f-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="e358f-127">Tenere presente, tuttavia, che altri route definite in Global. asax potrebbero corrispondere alla stessa richiesta.</span><span class="sxs-lookup"><span data-stu-id="e358f-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="e358f-128">È importante comprendere che un vincolo impedisce a una particolare route di una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="e358f-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="e358f-129">Si noti che la route predefinita è stata commento dal file Global. asax nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="e358f-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="e358f-130">Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="e358f-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="e358f-131">In tal caso, gli utenti remoti potrebbe ancora richiamare azioni del controller di amministrazione anche se le richieste non corrispondono alla route di amministratore.</span><span class="sxs-lookup"><span data-stu-id="e358f-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e358f-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="e358f-132">Previous</span></span>](creating-a-route-constraint-vb.md)
