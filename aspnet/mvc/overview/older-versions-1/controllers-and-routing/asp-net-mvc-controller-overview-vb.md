---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Panoramica del Controller ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther presenta controller ASP.NET MVC. Descrive come creare nuovi controller e di restituire tipi diversi di res azione...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809223"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="f2dda-104">Panoramica del Controller ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="f2dda-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="f2dda-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f2dda-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f2dda-106">In questa esercitazione, Stephen Walther presenta controller ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="f2dda-107">Descrive come creare nuovi controller e restituire diversi tipi di risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f2dda-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="f2dda-108">Questa esercitazione illustra l'argomento del controller ASP.NET MVC, le azioni del controller e i risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f2dda-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="f2dda-109">Dopo aver completato questa esercitazione, è possibile comprendere come i controller vengono usati per controllare il modo in cui che un visitatore interagisce con un sito Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="f2dda-110">Informazioni sui controller</span><span class="sxs-lookup"><span data-stu-id="f2dda-110">Understanding Controllers</span></span>

<span data-ttu-id="f2dda-111">Controller MVC sono responsabili per rispondere alle richieste effettuate per un sito Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="f2dda-112">Ogni richiesta del browser viene eseguito il mapping a un controller specifico.</span><span class="sxs-lookup"><span data-stu-id="f2dda-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="f2dda-113">Si supponga, ad esempio, che si immette l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="f2dda-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="f2dda-114">In questo caso, viene richiamato un controller denominato ProductController.</span><span class="sxs-lookup"><span data-stu-id="f2dda-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="f2dda-115">Il ProductController è responsabile della generazione la risposta alla richiesta del browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="f2dda-116">Ad esempio, il controller potrebbe restituire una visualizzazione specifica al browser o il controller potrebbe reindirizzare l'utente a un altro controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="f2dda-117">L'elenco 1 contiene un controller semplice denominato ProductController.</span><span class="sxs-lookup"><span data-stu-id="f2dda-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="f2dda-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="f2dda-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="f2dda-119">Come può notare da listato 1, un controller è semplicemente una classe (una classe Visual Basic .NET o c#).</span><span class="sxs-lookup"><span data-stu-id="f2dda-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="f2dda-120">Un controller è una classe che deriva dalla classe di base MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="f2dda-121">Poiché un controller eredita da questa classe di base, un controller eredita diversi metodi utili gratuitamente (questi metodi sono illustrati di seguito).</span><span class="sxs-lookup"><span data-stu-id="f2dda-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="f2dda-122">Informazioni sulle azioni del Controller</span><span class="sxs-lookup"><span data-stu-id="f2dda-122">Understanding Controller Actions</span></span>

<span data-ttu-id="f2dda-123">Un controller espone le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-123">A controller exposes controller actions.</span></span> <span data-ttu-id="f2dda-124">Un'azione è un metodo in un controller che viene chiamato quando si immette un URL specifico nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="f2dda-125">Si supponga, ad esempio, si effettua una richiesta per l'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="f2dda-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="f2dda-126">In questo caso, viene chiamato il metodo Index () sulla classe ProductController.</span><span class="sxs-lookup"><span data-stu-id="f2dda-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="f2dda-127">Il metodo Index () è un esempio di un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="f2dda-128">Un'azione del controller deve essere un metodo pubblico di una classe controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="f2dda-129">Metodi di Visual Basic.NET, per impostazione predefinita, sono metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="f2dda-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="f2dda-130">Tenere presente che qualsiasi metodo pubblico che aggiunta a una classe controller viene esposta come un'azione del controller automaticamente (è necessario prestare attenzione su questo perché un'azione del controller può essere richiamata da chiunque nell'universo semplicemente digitando l'URL corretto in una barra degli indirizzi del browser).</span><span class="sxs-lookup"><span data-stu-id="f2dda-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="f2dda-131">Esistono alcuni requisiti aggiuntivi che devono essere soddisfatti da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="f2dda-132">Un metodo usato come un'azione del controller non possa essere sottoposti a overload.</span><span class="sxs-lookup"><span data-stu-id="f2dda-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="f2dda-133">Inoltre, un'azione del controller non può essere un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="f2dda-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="f2dda-134">A parte questo, è possibile usare qualsiasi metodo come un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="f2dda-135">Informazioni sui risultati delle azioni</span><span class="sxs-lookup"><span data-stu-id="f2dda-135">Understanding Action Results</span></span>

<span data-ttu-id="f2dda-136">Un'azione del controller restituisce un valore chiamato un' *risultato dell'azione*.</span><span class="sxs-lookup"><span data-stu-id="f2dda-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="f2dda-137">Risultato di un'azione è ciò che restituisce un'azione del controller in risposta a una richiesta del browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="f2dda-138">Il framework ASP.NET MVC supporta diversi tipi di risultati delle azioni inclusi:</span><span class="sxs-lookup"><span data-stu-id="f2dda-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="f2dda-139">ViewResult - rappresenta HTML e il markup.</span><span class="sxs-lookup"><span data-stu-id="f2dda-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="f2dda-140">EmptyResult - non rappresenta alcun risultato.</span><span class="sxs-lookup"><span data-stu-id="f2dda-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="f2dda-141">RedirectResult - rappresenta un reindirizzamento a un nuovo URL.</span><span class="sxs-lookup"><span data-stu-id="f2dda-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="f2dda-142">JsonResult - rappresenta un risultato di JavaScript Object Notation che può essere utilizzato in un'applicazione AJAX.</span><span class="sxs-lookup"><span data-stu-id="f2dda-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="f2dda-143">JavaScriptResult - rappresenta uno script JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2dda-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="f2dda-144">ContentResult - rappresenta un risultato di testo.</span><span class="sxs-lookup"><span data-stu-id="f2dda-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="f2dda-145">FileContentResult - rappresenta un file scaricabile (con il contenuto binario).</span><span class="sxs-lookup"><span data-stu-id="f2dda-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="f2dda-146">FilePathResult - rappresenta un file scaricabile (con un percorso).</span><span class="sxs-lookup"><span data-stu-id="f2dda-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="f2dda-147">FileStreamResult - rappresenta un file scaricabile (con un flusso di file).</span><span class="sxs-lookup"><span data-stu-id="f2dda-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="f2dda-148">Tutti questi risultati azione ereditano dalla classe ActionResult di base.</span><span class="sxs-lookup"><span data-stu-id="f2dda-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="f2dda-149">Nella maggior parte dei casi, un'azione del controller restituisce l'elemento ViewResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="f2dda-150">Ad esempio, l'azione Index del controller nel listato 2 restituisce l'elemento ViewResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="f2dda-151">**Listato 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="f2dda-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="f2dda-152">Quando un'azione restituisce l'elemento ViewResult, HTML viene restituito al browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="f2dda-153">Il metodo Index () nel listato 2 restituisce una visualizzazione denominata indice al browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="f2dda-154">Si noti che l'azione Index () nel listato 2 non viene restituito un ViewResult().</span><span class="sxs-lookup"><span data-stu-id="f2dda-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="f2dda-155">Al contrario, il metodo View() della classe Controller è chiamato.</span><span class="sxs-lookup"><span data-stu-id="f2dda-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="f2dda-156">In genere, non restituiscono direttamente un risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f2dda-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="f2dda-157">Al contrario, si chiama uno dei seguenti metodi della classe di base di Controller:</span><span class="sxs-lookup"><span data-stu-id="f2dda-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="f2dda-158">Vista - restituisce un risultato dell'azione ViewResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="f2dda-159">Reindirizzare - restituisce un risultato dell'azione RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="f2dda-160">RedirectToAction - restituisce un risultato dell'azione RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="f2dda-161">RedirectToRoute - restituisce un risultato dell'azione RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="f2dda-162">JSON - restituisce un risultato dell'azione JsonResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="f2dda-163">JavaScriptResult - restituisce un JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="f2dda-164">Content: restituisce un risultato dell'azione ContentResult.</span><span class="sxs-lookup"><span data-stu-id="f2dda-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="f2dda-165">File - restituisce un FileContentResult, FilePathResult o FileStreamResult a seconda dei parametri passati al metodo.</span><span class="sxs-lookup"><span data-stu-id="f2dda-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="f2dda-166">Pertanto, se si desidera restituire una visualizzazione nel browser, chiamare il metodo View().</span><span class="sxs-lookup"><span data-stu-id="f2dda-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="f2dda-167">Se si desidera reindirizzare l'utente dall'azione di un controller a un'altra, chiamare il metodo RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="f2dda-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="f2dda-168">Ad esempio, l'azione Details() nel listato 3 viene mostrata una visualizzazione o reindirizza l'utente per l'azione Index () a seconda che il parametro Id abbia un valore.</span><span class="sxs-lookup"><span data-stu-id="f2dda-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="f2dda-169">**Listato 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="f2dda-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="f2dda-170">Il risultato dell'azione ContentResult è speciale.</span><span class="sxs-lookup"><span data-stu-id="f2dda-170">The ContentResult action result is special.</span></span> <span data-ttu-id="f2dda-171">È possibile usare il risultato dell'azione ContentResult per restituire un risultato dell'azione come testo normale.</span><span class="sxs-lookup"><span data-stu-id="f2dda-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="f2dda-172">Ad esempio, il metodo Index () nel listato 4 restituisce un messaggio come testo normale e non come HTML.</span><span class="sxs-lookup"><span data-stu-id="f2dda-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="f2dda-173">**Listato 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="f2dda-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="f2dda-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="f2dda-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="f2dda-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="f2dda-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="f2dda-176">Quando viene richiamata l'azione StatusController.Index(), una vista non viene restituita.</span><span class="sxs-lookup"><span data-stu-id="f2dda-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="f2dda-177">Al contrario, il testo "Hello World!" non elaborato</span><span class="sxs-lookup"><span data-stu-id="f2dda-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="f2dda-178">viene restituito al browser.</span><span class="sxs-lookup"><span data-stu-id="f2dda-178">is returned to the browser.</span></span>

<span data-ttu-id="f2dda-179">Se un'azione del controller restituisce un risultato che è non un risultato dell'azione, ad esempio, una data o un numero intero, quindi il risultato viene inserito in un ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f2dda-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="f2dda-180">Ad esempio, quando viene richiamata l'azione Index () di WorkController nel listato 5, la data viene restituita come un ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f2dda-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="f2dda-181">**Listato 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="f2dda-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="f2dda-182">L'azione Index () nel listato 5 restituisce un oggetto DateTime.</span><span class="sxs-lookup"><span data-stu-id="f2dda-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="f2dda-183">Il framework ASP.NET MVC Converte l'oggetto DateTime in una stringa ed esegue il wrapping del valore DateTime in un ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f2dda-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="f2dda-184">Il browser riceve la data e ora come testo normale.</span><span class="sxs-lookup"><span data-stu-id="f2dda-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="f2dda-185">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f2dda-185">Summary</span></span>

<span data-ttu-id="f2dda-186">Lo scopo di questa esercitazione è stato per un'introduzione ai concetti del controller, azioni del controller e i risultati dell'azione controller ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="f2dda-187">Nella prima sezione, è stato descritto come aggiungere nuovi controller per un progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2dda-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="f2dda-188">Successivamente, si è appreso come pubblici metodi di un controller vengono esposti di Universe (universo) come le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="f2dda-189">Infine, abbiamo parlato di diversi tipi di risultati delle azioni che possono essere restituiti da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="f2dda-190">In particolare, viene illustrato come restituire un elemento ViewResult, RedirectToActionResult e ContentResult da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="f2dda-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f2dda-191">[Precedente](creating-a-custom-route-constraint-cs.md)
> [Successivo](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f2dda-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
