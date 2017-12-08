---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Panoramica di Routing di ASP.NET MVC (VB) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther Mostra come framework di MVC ASP.NET esegue il mapping alle richieste del browser per le azioni del controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="9abc5-103">Panoramica di Routing di ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="9abc5-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="9abc5-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9abc5-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9abc5-105">In questa esercitazione, Stephen Walther Mostra come framework di MVC ASP.NET esegue il mapping alle richieste del browser per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9abc5-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="9abc5-106">In questa esercitazione vengono introdotte a una caratteristica importante di ogni applicazione MVC ASP.NET denominato *Routing ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="9abc5-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="9abc5-107">Il modulo di Routing di ASP.NET è responsabile per il mapping in ingresso alle richieste del browser per le azioni del controller MVC specifiche.</span><span class="sxs-lookup"><span data-stu-id="9abc5-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="9abc5-108">Al termine di questa esercitazione, è possibile comprendere come tabella di routing standard esegue il mapping delle richieste per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9abc5-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="9abc5-109">Utilizzando la tabella di Route predefinita</span><span class="sxs-lookup"><span data-stu-id="9abc5-109">Using the Default Route Table</span></span>

<span data-ttu-id="9abc5-110">Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurato per utilizzare il Routing di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abc5-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="9abc5-111">Routing ASP.NET sia configurato in due posizioni.</span><span class="sxs-lookup"><span data-stu-id="9abc5-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="9abc5-112">In primo luogo, il Routing di ASP.NET è abilitato nel file di configurazione dell'applicazione Web (file Web. config).</span><span class="sxs-lookup"><span data-stu-id="9abc5-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="9abc5-113">Sono presenti quattro sezioni nel file di configurazione rilevanti per il routing: la sezione system.web.httpModules, la sezione system.web.httpHandlers, la sezione system.webserver.modules e la sezione system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="9abc5-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="9abc5-114">Prestare attenzione a non eliminare queste sezioni perché senza queste sezioni routing non funzionerà più.</span><span class="sxs-lookup"><span data-stu-id="9abc5-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="9abc5-115">In secondo luogo e soprattutto, nel file Global. asax dell'applicazione viene creata una tabella di route.</span><span class="sxs-lookup"><span data-stu-id="9abc5-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="9abc5-116">Il file Global. asax è un file speciale che contiene i gestori eventi per eventi del ciclo di vita dell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abc5-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="9abc5-117">La tabella di route viene creata durante l'evento di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9abc5-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="9abc5-118">Il file di listato 1 contiene il file Global. asax predefinito per un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abc5-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="9abc5-119">**Elenco 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="9abc5-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="9abc5-120">Quando un'applicazione MVC prima avvia, l'applicazione\_viene chiamato il metodo Start ().</span><span class="sxs-lookup"><span data-stu-id="9abc5-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="9abc5-121">Questo metodo, a sua volta chiama il metodo di RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="9abc5-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="9abc5-122">Il metodo RegisterRoutes() crea la tabella di route.</span><span class="sxs-lookup"><span data-stu-id="9abc5-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="9abc5-123">La tabella di route predefinita contiene una singola route (denominata valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="9abc5-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="9abc5-124">La route predefinita esegue il mapping il primo segmento di un URL a un nome del controller, il secondo segmento di un URL per un'azione del controller e il terzo segmento in un parametro denominato **id**.</span><span class="sxs-lookup"><span data-stu-id="9abc5-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="9abc5-125">Si supponga che si immette l'URL seguente nella barra degli indirizzi del web browser:</span><span class="sxs-lookup"><span data-stu-id="9abc5-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="9abc5-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="9abc5-126">/Home/Index/3</span></span>

<span data-ttu-id="9abc5-127">La route predefinita esegue il mapping di questo URL per i parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="9abc5-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="9abc5-128">Controller Home =</span><span class="sxs-lookup"><span data-stu-id="9abc5-128">controller = Home</span></span>

- <span data-ttu-id="9abc5-129">Azione = indice</span><span class="sxs-lookup"><span data-stu-id="9abc5-129">action = Index</span></span>

- <span data-ttu-id="9abc5-130">ID = 3</span><span class="sxs-lookup"><span data-stu-id="9abc5-130">id = 3</span></span>

<span data-ttu-id="9abc5-131">Quando si richiede il 3/indice/URL /Home, viene eseguito il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9abc5-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="9abc5-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="9abc5-132">HomeController.Index(3)</span></span>

<span data-ttu-id="9abc5-133">La route predefinita include le impostazioni predefinite per tutti e tre i parametri.</span><span class="sxs-lookup"><span data-stu-id="9abc5-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="9abc5-134">Se non si specifica un controller, il parametro controller per impostazione predefinita sarà il valore **Home**.</span><span class="sxs-lookup"><span data-stu-id="9abc5-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="9abc5-135">Se non si specifica un'azione, il parametro di azione viene impostata sul valore **indice**.</span><span class="sxs-lookup"><span data-stu-id="9abc5-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="9abc5-136">Infine, se non si specifica un id, il parametro id per impostazione predefinita in una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="9abc5-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="9abc5-137">Ecco alcuni esempi di come la route predefinita esegue il mapping degli URL per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9abc5-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="9abc5-138">Si supponga che si immette l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="9abc5-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="9abc5-139">/ Home</span><span class="sxs-lookup"><span data-stu-id="9abc5-139">/Home</span></span>

<span data-ttu-id="9abc5-140">A causa di valori di parametro predefiniti della route predefinito, immettere l'URL verrà causa l'index () del metodo della classe HomeController listato 2 da chiamare.</span><span class="sxs-lookup"><span data-stu-id="9abc5-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="9abc5-141">**Elenco di 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="9abc5-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="9abc5-142">Nel listato 2, la classe HomeController include un metodo denominato Index () che accetta un singolo parametro denominato ID. /Home l'URL, il metodo Index () essere chiamato con il valore Nothing come valore del parametro Id.</span><span class="sxs-lookup"><span data-stu-id="9abc5-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="9abc5-143">A causa della modalità che il framework di MVC richiama le azioni del controller, l'URL di /Home corrisponde anche il metodo Index () della classe HomeController listato 3.</span><span class="sxs-lookup"><span data-stu-id="9abc5-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="9abc5-144">**Elenco di 3 - HomeController.vb (operazione sull'indice senza parametri)**</span><span class="sxs-lookup"><span data-stu-id="9abc5-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="9abc5-145">Il metodo Index () nel listato 3 non accetta parametri.</span><span class="sxs-lookup"><span data-stu-id="9abc5-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="9abc5-146">L'URL di /Home causerà questa chiamata al metodo Index ().</span><span class="sxs-lookup"><span data-stu-id="9abc5-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="9abc5-147">URL /Home/indice/3 richiama anche questo metodo (l'Id viene ignorato).</span><span class="sxs-lookup"><span data-stu-id="9abc5-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="9abc5-148">L'URL di /Home corrisponde anche il metodo Index () della classe HomeController listato 4.</span><span class="sxs-lookup"><span data-stu-id="9abc5-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="9abc5-149">**Elenco di 4 - HomeController.vb (operazione sull'indice con il parametro ammette valori null)**</span><span class="sxs-lookup"><span data-stu-id="9abc5-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="9abc5-150">Listato 4, il metodo Index () ha un parametro di tipo Integer.</span><span class="sxs-lookup"><span data-stu-id="9abc5-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="9abc5-151">Poiché il parametro è un parametro ammette valori null (può avere il valore Nothing), l'indice può essere chiamato senza che venga generato un errore.</span><span class="sxs-lookup"><span data-stu-id="9abc5-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="9abc5-152">Infine, richiamare il metodo Index () nel listato 5 con l'URL di /Home genera un'eccezione perché il parametro Id *non* un parametro ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="9abc5-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="9abc5-153">Se si tenta di richiamare il metodo Index () restituito l'errore visualizzato nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="9abc5-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="9abc5-154">**Elenco di 5 - HomeController.vb (operazione sull'indice con il parametro Id)**</span><span class="sxs-lookup"><span data-stu-id="9abc5-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="9abc5-155">[![Richiamare un'azione del controller che prevede un valore di parametro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9abc5-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="9abc5-156">**Figura 01**: richiamare un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine ingrandita](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9abc5-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="9abc5-157">URL /Home/indice/3, d'altra parte, funziona perfettamente con l'azione del controller di indice nel listato 5.</span><span class="sxs-lookup"><span data-stu-id="9abc5-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="9abc5-158">/Home/Index/3 la richiesta, il metodo Index () essere chiamato con un parametro Id con il valore 3.</span><span class="sxs-lookup"><span data-stu-id="9abc5-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="9abc5-159">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="9abc5-159">Summary</span></span>

<span data-ttu-id="9abc5-160">L'obiettivo di questa esercitazione è stata per fornire una breve introduzione al Routing di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abc5-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="9abc5-161">Abbiamo esaminato la tabella di route predefinita che ottiene con una nuova applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9abc5-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="9abc5-162">Si è appreso come la route predefinita esegue il mapping degli URL per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9abc5-162">You learned how the default route maps URLs to controller actions.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9abc5-163">[Precedente](creating-an-action-cs.md)
[Successivo](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9abc5-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
