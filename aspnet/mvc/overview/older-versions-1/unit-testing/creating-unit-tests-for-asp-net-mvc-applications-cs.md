---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Creazione di Unit test per applicazioni ASP.NET MVC (c#) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce un parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: ccd9a1b3aee8379c23c01c5eb7f756a786f6359d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869712"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="df672-104">Creazione di Unit test per applicazioni ASP.NET MVC (c#)</span><span class="sxs-lookup"><span data-stu-id="df672-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="df672-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="df672-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="df672-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="df672-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="df672-107">Informazioni su come creare unit test per le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="df672-108">In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce una visualizzazione particolare, restituisce un set specifico di dati oppure un altro tipo di risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="df672-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="df672-109">L'obiettivo di questa esercitazione è dimostrare come è possibile scrivere unit test per i controller in ASP.NET MVC applicazioni.</span><span class="sxs-lookup"><span data-stu-id="df672-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="df672-110">È descritto come creare tre tipi diversi di unit test.</span><span class="sxs-lookup"><span data-stu-id="df672-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="df672-111">È illustrato come testare la vista restituita da un'azione del controller, come i dati della visualizzazione restituito da un'azione del controller di test e come testare o meno un'azione del controller si viene reindirizzati a una seconda azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="df672-112">Creazione di Controller sottoposta a Test</span><span class="sxs-lookup"><span data-stu-id="df672-112">Creating the Controller under Test</span></span>

<span data-ttu-id="df672-113">Iniziamo creando il controller che si desidera testare.</span><span class="sxs-lookup"><span data-stu-id="df672-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="df672-114">Il controller, denominato il `ProductController`, è contenuto in elenco 1.</span><span class="sxs-lookup"><span data-stu-id="df672-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="df672-115">**Elenco 1: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="df672-116">Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`.</span><span class="sxs-lookup"><span data-stu-id="df672-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="df672-117">Entrambi i metodi di azione restituiscono una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="df672-117">Both action methods return a view.</span></span> <span data-ttu-id="df672-118">Si noti che il `Details()` azione accetta un parametro denominato ID.</span><span class="sxs-lookup"><span data-stu-id="df672-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="df672-119">Test della visualizzazione restituito da un Controller</span><span class="sxs-lookup"><span data-stu-id="df672-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="df672-120">Si supponga che si desidera testare o meno il `ProductController` restituisce della vista destra.</span><span class="sxs-lookup"><span data-stu-id="df672-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="df672-121">Si desidera assicurarsi che quando il `ProductController.Details()` azione viene richiamata, viene restituita la visualizzazione dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="df672-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="df672-122">La classe di test nel listato 2 contiene uno unit test per la visualizzazione restituita da test di `ProductController.Details()` azione.</span><span class="sxs-lookup"><span data-stu-id="df672-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="df672-123">**Elenco 2: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="df672-124">La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="df672-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="df672-125">Questo metodo contiene tre righe di codice.</span><span class="sxs-lookup"><span data-stu-id="df672-125">This method contains three lines of code.</span></span> <span data-ttu-id="df672-126">La prima riga di codice crea una nuova istanza di `ProductController` classe.</span><span class="sxs-lookup"><span data-stu-id="df672-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="df672-127">La seconda riga di codice richiama il controller `Details()` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="df672-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="df672-128">Infine, l'ultima riga di codice controlli o meno la visualizzazione restituita dal `Details()` azione è la visualizzazione dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="df672-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="df672-129">Il `ViewResult.ViewName` proprietà rappresenta il nome della visualizzazione restituito da un controller.</span><span class="sxs-lookup"><span data-stu-id="df672-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="df672-130">Un avviso di grandi dimensioni sul test di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="df672-130">One big warning about testing this property.</span></span> <span data-ttu-id="df672-131">Esistono due modi che un controller può restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="df672-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="df672-132">Un controller può restituire in modo esplicito una visualizzazione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="df672-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="df672-133">In alternativa, il nome della visualizzazione può essere dedotto dal nome dell'azione del controller simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="df672-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="df672-134">Questa azione del controller restituisce inoltre una visualizzazione denominata `Details`.</span><span class="sxs-lookup"><span data-stu-id="df672-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="df672-135">Tuttavia, il nome della visualizzazione viene dedotto dal nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="df672-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="df672-136">Se si desidera verificare il nome della vista, è necessario restituire in modo esplicito il nome della visualizzazione dall'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="df672-137">È possibile eseguire lo unit test nel listato 2 immettendo la combinazione di tasti **Ctrl-R, A** o facendo clic di **eseguire tutti i test nella soluzione** pulsante (vedere Figura 1).</span><span class="sxs-lookup"><span data-stu-id="df672-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="df672-138">Se il test viene superato, verrà visualizzato nella finestra Risultati Test nella figura 2.</span><span class="sxs-lookup"><span data-stu-id="df672-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="df672-139">[![Eseguire i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df672-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="df672-140">**Figura 01**: eseguire tutti i test nella soluzione ([fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df672-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="df672-141">[![Operazione completata](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="df672-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="df672-142">**Figura 02**: esito positivo.</span><span class="sxs-lookup"><span data-stu-id="df672-142">**Figure 02**: Success!</span></span> <span data-ttu-id="df672-143">([Fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="df672-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="df672-144">Verifica dei dati della visualizzazione restituito da un Controller</span><span class="sxs-lookup"><span data-stu-id="df672-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="df672-145">Controller MVC passa i dati in una vista utilizzando uno strumento denominato *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="df672-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="df672-146">Ad esempio, si supponga che si desidera visualizzare i dettagli per un determinato prodotto quando si richiama il `ProductController Details()` azione.</span><span class="sxs-lookup"><span data-stu-id="df672-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="df672-147">In tal caso, è possibile creare un'istanza di un `Product` classe (definito nel modello) e passare l'istanza di `Details` visualizzazione sfruttando `View Data`.</span><span class="sxs-lookup"><span data-stu-id="df672-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="df672-148">Modificato `ProductController` listato 3 include una versione aggiornata `Details()` azione che restituisce un prodotto.</span><span class="sxs-lookup"><span data-stu-id="df672-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="df672-149">**Elenco di 3: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="df672-150">Prima di tutto, il `Details()` azione viene creata una nuova istanza di `Product` classe che rappresenta un computer portatile.</span><span class="sxs-lookup"><span data-stu-id="df672-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="df672-151">Successivamente, l'istanza del `Product` classe viene passata come secondo parametro per il `View()` metodo.</span><span class="sxs-lookup"><span data-stu-id="df672-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="df672-152">È possibile scrivere unit test per verificare se i dati previsti sono contenuto nella visualizzazione dati.</span><span class="sxs-lookup"><span data-stu-id="df672-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="df672-153">Lo unit test in test listato 4 o meno un prodotto che rappresenta un computer portatile viene restituito quando si chiama il `ProductController Details()` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="df672-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="df672-154">**Elenco di 4: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="df672-155">Listato 4 il `TestDetailsView()` metodo verifica i dati della visualizzazione restituito richiamando il `Details()` metodo.</span><span class="sxs-lookup"><span data-stu-id="df672-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="df672-156">Il `ViewData` esposto come proprietà nel `ViewResult` restituito richiamando il `Details()` metodo.</span><span class="sxs-lookup"><span data-stu-id="df672-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="df672-157">Il `ViewData.Model` proprietà contiene il prodotto passato alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="df672-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="df672-158">Il test di verifica semplicemente che il prodotto contenuto nei dati di visualizzazione con il nome di computer portatile.</span><span class="sxs-lookup"><span data-stu-id="df672-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="df672-159">Verifica il risultato dell'azione restituito da un Controller</span><span class="sxs-lookup"><span data-stu-id="df672-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="df672-160">Un'azione del controller più complessa può restituire diversi tipi di risultati dell'azione in base ai valori dei parametri passati all'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="df672-161">Un'azione del controller può restituire un'ampia gamma di tipi di risultati dell'azione tra una `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="df672-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="df672-162">Ad esempio, l'elemento modificato `Details()` azione listato 5 restituisce il `Details` visualizzare quando si passa un Id di prodotto valido per l'azione.</span><span class="sxs-lookup"><span data-stu-id="df672-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="df672-163">Se si passa un prodotto valido Id - Id con un valore minore di - 1, quindi si viene reindirizzati al `Index()` azione.</span><span class="sxs-lookup"><span data-stu-id="df672-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="df672-164">**Elenco 5: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="df672-165">È possibile testare il comportamento del `Details()` azione con lo unit test nel listato 6.</span><span class="sxs-lookup"><span data-stu-id="df672-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="df672-166">Lo unit test nel listato 6 verifica che si verrà reindirizzati al `Index` visualizzare quando viene passato un Id con il valore -1 per il `Details()` metodo.</span><span class="sxs-lookup"><span data-stu-id="df672-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="df672-167">**Elenco 6: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="df672-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="df672-168">Quando si chiama il `RedirectToAction()` azione del controller di metodo in un'azione del controller, restituisce un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="df672-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="df672-169">I controlli di test se il `RedirectToRouteResult` reindirizzerà l'utente a un'azione del controller denominata `Index`.</span><span class="sxs-lookup"><span data-stu-id="df672-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="df672-170">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="df672-170">Summary</span></span>

<span data-ttu-id="df672-171">In questa esercitazione è stato descritto come creare unit test per le azioni del controller MVC.</span><span class="sxs-lookup"><span data-stu-id="df672-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="df672-172">In primo luogo, è stato descritto come verificare se la vista destra verrà restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="df672-173">Si è appreso come utilizzare il `ViewResult.ViewName` proprietà per verificare il nome di una vista.</span><span class="sxs-lookup"><span data-stu-id="df672-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="df672-174">Successivamente, viene esaminato come è possibile testare il contenuto di `View Data`.</span><span class="sxs-lookup"><span data-stu-id="df672-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="df672-175">È stato descritto come controllare se il prodotto di destra è stato restituito `View Data` dopo la chiamata a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="df672-176">Infine, è illustrato come è possibile verificare se vengono restituiti tipi diversi di risultati dell'azione da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="df672-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="df672-177">È stato descritto come verificare se un controller restituisce un `ViewResult` o `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="df672-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="df672-178">avanti</span><span class="sxs-lookup"><span data-stu-id="df672-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
