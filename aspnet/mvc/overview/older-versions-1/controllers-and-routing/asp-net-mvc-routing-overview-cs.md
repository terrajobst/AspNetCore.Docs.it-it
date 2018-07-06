---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Panoramica del Routing di ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come il framework ASP.NET MVC esegue il mapping alle richieste del browser per le azioni del controller.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819147"
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="65843-103">Panoramica del Routing di ASP.NET MVC (c#)</span><span class="sxs-lookup"><span data-stu-id="65843-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="65843-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="65843-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="65843-105">In questa esercitazione, Stephen Walther spiega come il framework ASP.NET MVC esegue il mapping alle richieste del browser per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="65843-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="65843-106">In questa esercitazione viene presentato una caratteristica importante di tutte le applicazioni ASP.NET MVC chiamata *Routing ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="65843-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="65843-107">Il modulo di Routing di ASP.NET è responsabile del mapping di richieste del browser in ingresso alle azioni del controller MVC specifiche.</span><span class="sxs-lookup"><span data-stu-id="65843-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="65843-108">Al termine di questa esercitazione, è possibile comprendere come la tabella di routing standard dei esegue il mapping delle richieste alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="65843-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="65843-109">Usando la tabella di Route predefinita</span><span class="sxs-lookup"><span data-stu-id="65843-109">Using the Default Route Table</span></span>

<span data-ttu-id="65843-110">Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurata per usare il Routing di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65843-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="65843-111">Il Routing ASP.NET è configurata in due posizioni.</span><span class="sxs-lookup"><span data-stu-id="65843-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="65843-112">In primo luogo, il Routing di ASP.NET è abilitato nel file di configurazione dell'applicazione Web (file Web. config).</span><span class="sxs-lookup"><span data-stu-id="65843-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="65843-113">Esistono quattro sezioni nel file di configurazione che sono rilevanti per il routing: la sezione system.web.httpModules, la sezione system.web.httpHandlers, la sezione system.webserver.modules e la sezione system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="65843-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="65843-114">Prestare attenzione a non eliminare queste sezioni, perché senza queste sezioni routing non funzionerà più.</span><span class="sxs-lookup"><span data-stu-id="65843-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="65843-115">In secondo luogo e ancora più importante, viene creata una tabella di route nel file Global. asax dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65843-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="65843-116">Il file Global. asax è un file speciale che contiene i gestori eventi per eventi del ciclo di vita dell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65843-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="65843-117">La tabella di route viene creata durante l'evento di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65843-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="65843-118">Il file nel listato 1 contiene il file Global. asax predefinito per un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="65843-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="65843-119">**Listato 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="65843-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="65843-120">Quando un'applicazione MVC primo avvio, l'applicazione\_viene chiamato il metodo Start ().</span><span class="sxs-lookup"><span data-stu-id="65843-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="65843-121">Questo metodo, a sua volta, chiama il metodo RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="65843-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="65843-122">Il metodo RegisterRoutes() crea la tabella di route.</span><span class="sxs-lookup"><span data-stu-id="65843-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="65843-123">La tabella di route predefinito contiene una singola route (denominata Default).</span><span class="sxs-lookup"><span data-stu-id="65843-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="65843-124">La route predefinita mappa il primo segmento di URL per un nome di controller, il secondo segmento di URL di un'azione del controller e il terzo segmento per un parametro denominato **id**.</span><span class="sxs-lookup"><span data-stu-id="65843-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="65843-125">Si supponga che si immette l'URL seguente nella barra degli indirizzi del web browser:</span><span class="sxs-lookup"><span data-stu-id="65843-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="65843-126">/ Home/indice/3</span><span class="sxs-lookup"><span data-stu-id="65843-126">/Home/Index/3</span></span>

<span data-ttu-id="65843-127">La route predefinita esegue il mapping di questo URL per i parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="65843-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="65843-128">controller = Home</span><span class="sxs-lookup"><span data-stu-id="65843-128">controller = Home</span></span>

- <span data-ttu-id="65843-129">azione = indice</span><span class="sxs-lookup"><span data-stu-id="65843-129">action = Index</span></span>

- <span data-ttu-id="65843-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="65843-130">id = 3</span></span>

<span data-ttu-id="65843-131">Quando si richiede l'URL avremo/indice/3, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="65843-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="65843-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="65843-132">HomeController.Index(3)</span></span>

<span data-ttu-id="65843-133">La route predefinita include le impostazioni predefinite per tutti i tre parametri.</span><span class="sxs-lookup"><span data-stu-id="65843-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="65843-134">Se non si fornisce un controller, quindi il parametro controller viene impostato sul valore **Home**.</span><span class="sxs-lookup"><span data-stu-id="65843-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="65843-135">Se non viene fornita un'azione, il parametro action viene impostata sul valore **indice**.</span><span class="sxs-lookup"><span data-stu-id="65843-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="65843-136">Infine, se non si fornisce un id, il parametro id viene impostato su una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="65843-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="65843-137">Esaminiamo alcuni esempi di come la route predefinita esegue il mapping degli URL alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="65843-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="65843-138">Si supponga che si immette l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="65843-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="65843-139">/Home</span><span class="sxs-lookup"><span data-stu-id="65843-139">/Home</span></span>

<span data-ttu-id="65843-140">A causa di valori di parametro predefiniti della route predefinita, immettere questo URL causerà il metodo Index () della classe HomeController nel listato 2 da chiamare.</span><span class="sxs-lookup"><span data-stu-id="65843-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="65843-141">**Listato 2 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="65843-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="65843-142">Nel listato 2, la classe HomeController include un metodo denominato Index () che accetta un singolo parametro denominato ID. Avremo l'URL, il metodo Index () essere chiamato con una stringa vuota come valore del parametro Id.</span><span class="sxs-lookup"><span data-stu-id="65843-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="65843-143">A causa della modalità che il framework di MVC richiama le azioni del controller, avremo l'URL corrisponda anche al metodo Index () della classe HomeController nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="65843-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="65843-144">**Listato 3 - HomeController.cs (operazione sull'indice senza parametri)**</span><span class="sxs-lookup"><span data-stu-id="65843-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="65843-145">Il metodo Index () nel listato 3 non accetta alcun parametro.</span><span class="sxs-lookup"><span data-stu-id="65843-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="65843-146">L'URL avremo causerà questa chiamata al metodo Index ().</span><span class="sxs-lookup"><span data-stu-id="65843-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="65843-147">Inoltre, l'URL avremo/indice/3 richiama questo metodo (l'Id viene ignorato).</span><span class="sxs-lookup"><span data-stu-id="65843-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="65843-148">Avremo l'URL corrisponda anche al metodo Index () della classe HomeController nel listato 4.</span><span class="sxs-lookup"><span data-stu-id="65843-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="65843-149">**Listato 4 - HomeController.cs (operazione sull'indice con il parametro ammette valori null)**</span><span class="sxs-lookup"><span data-stu-id="65843-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="65843-150">Nel listato 4, il metodo Index () ha un solo parametro Integer.</span><span class="sxs-lookup"><span data-stu-id="65843-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="65843-151">Poiché il parametro è un parametro nullable (può avere il valore Null), l'indice può essere chiamato senza generare un errore.</span><span class="sxs-lookup"><span data-stu-id="65843-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="65843-152">Infine, richiamare il metodo Index () nel listato 5 con l'URL avremo genera un'eccezione poiché il parametro Id *non è* parametro ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="65843-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="65843-153">Se si prova a richiamare il metodo Index () viene visualizzato l'errore visualizzato nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="65843-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="65843-154">**Listato 5 - HomeController.cs (operazione sull'indice con il parametro Id)**</span><span class="sxs-lookup"><span data-stu-id="65843-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="65843-155">[![Richiamo di un'azione del controller che prevede un valore di parametro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="65843-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="65843-156">**Figura 01**: richiamare un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni normali](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="65843-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="65843-157">L'URL avremo/indice/3, d'altra parte, funziona perfettamente con l'azione Index del controller nel listato 5.</span><span class="sxs-lookup"><span data-stu-id="65843-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="65843-158">La richiesta /Home/Index/3 induce il metodo Index () essere chiamato con un parametro di Id con il valore 3.</span><span class="sxs-lookup"><span data-stu-id="65843-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="65843-159">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="65843-159">Summary</span></span>

<span data-ttu-id="65843-160">L'obiettivo di questa esercitazione è stata per fornire una breve introduzione al Routing di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65843-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="65843-161">Abbiamo esaminato la tabella di route predefinito che si ottiene con una nuova applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65843-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="65843-162">Si è appreso come la route predefinita esegue il mapping degli URL alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="65843-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="65843-163">avanti</span><span class="sxs-lookup"><span data-stu-id="65843-163">Next</span></span>](understanding-action-filters-cs.md)
