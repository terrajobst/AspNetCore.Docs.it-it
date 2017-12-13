---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Unit test controller in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: "In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller Web API 2. Prima di leggere questo argomento, è consigliabile leggere l'esercitazione unità..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="52a73-104">Unit test controller in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="52a73-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="52a73-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="52a73-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="52a73-106">In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller Web API 2.</span><span class="sxs-lookup"><span data-stu-id="52a73-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="52a73-107">Prima di leggere questo argomento, è possibile leggere l'esercitazione [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), che mostra come aggiungere un progetto di unit test alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="52a73-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="52a73-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="52a73-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="52a73-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="52a73-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="52a73-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="52a73-110">Web API 2</span></span>
> - <span data-ttu-id="52a73-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="52a73-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="52a73-112">Dopo aver utilizzato Moq, ma lo stesso concetto si applica a qualsiasi framework di simulazione.</span><span class="sxs-lookup"><span data-stu-id="52a73-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="52a73-113">Moq 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn, .NET 4.5 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="52a73-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="52a73-114">È un modello comune negli unit test &quot;Disponi-act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="52a73-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="52a73-115">Disponi: Consente di impostare tutti i prerequisiti per il test da eseguire.</span><span class="sxs-lookup"><span data-stu-id="52a73-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="52a73-116">ACT: Eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="52a73-116">Act: Perform the test.</span></span>
- <span data-ttu-id="52a73-117">Asserzione: Verificare che il test ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="52a73-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="52a73-118">Nel passaggio di disposizione, si utilizzeranno spesso fittizi o gli oggetti stub.</span><span class="sxs-lookup"><span data-stu-id="52a73-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="52a73-119">Che riduce al minimo il numero di dipendenze, pertanto il test è incentrato su un elemento di test.</span><span class="sxs-lookup"><span data-stu-id="52a73-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="52a73-120">Ecco alcuni aspetti di cui si deve essere unit test nei controller di Web API:</span><span class="sxs-lookup"><span data-stu-id="52a73-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="52a73-121">L'azione restituisce il tipo corretto di risposta.</span><span class="sxs-lookup"><span data-stu-id="52a73-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="52a73-122">Parametri non validi restituiscono la risposta di errore corretto.</span><span class="sxs-lookup"><span data-stu-id="52a73-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="52a73-123">L'azione chiama il metodo corretto sul livello del servizio o del repository.</span><span class="sxs-lookup"><span data-stu-id="52a73-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="52a73-124">Se la risposta include un modello di dominio, verificare il tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="52a73-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="52a73-125">Queste sono alcune delle operazioni generali per eseguire il test, ma gli aspetti specifici dipendono dall'implementazione del controller.</span><span class="sxs-lookup"><span data-stu-id="52a73-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="52a73-126">In particolare, ha una grande importanza che indica se restituiscono le azioni del controller **HttpResponseMessage** o **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="52a73-127">Per ulteriori informazioni su questi tipi di risultati, vedere [risultati dell'azione in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="52a73-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="52a73-128">Test per le azioni che restituiscono HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="52a73-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="52a73-129">Ecco un esempio di un controller di cui restituire azioni **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="52a73-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="52a73-130">Si noti che il controller utilizza inserimento di dipendenze di inserire un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="52a73-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="52a73-131">Che rende i controller di più verificabili, poiché è possibile inserire un repository fittizio.</span><span class="sxs-lookup"><span data-stu-id="52a73-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="52a73-132">Lo unit test seguente verifica che il `Get` metodo scrive un `Product` al corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="52a73-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="52a73-133">Si supponga che `repository` è una simulazione `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="52a73-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="52a73-134">È importante impostare **richiesta** e **configurazione** nel controller.</span><span class="sxs-lookup"><span data-stu-id="52a73-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="52a73-135">In caso contrario, il test avrà esito negativo con un **ArgumentNullException** o **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="52a73-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="52a73-136">Generazione di collegamento di test</span><span class="sxs-lookup"><span data-stu-id="52a73-136">Testing Link Generation</span></span>

<span data-ttu-id="52a73-137">Il `Post` chiamate al metodo **UrlHelper.Link** per creare i collegamenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="52a73-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="52a73-138">Questa operazione richiede un po' più il programma di installazione nello unit test:</span><span class="sxs-lookup"><span data-stu-id="52a73-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="52a73-139">Il **UrlHelper** classe deve avere i dati della richiesta URL e route, il test deve impostare i valori per questi.</span><span class="sxs-lookup"><span data-stu-id="52a73-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="52a73-140">Un'altra opzione consiste fittizi o stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="52a73-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="52a73-141">Con questo approccio, si sostituisce il valore predefinito di [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) con una versione fittizi o stub che restituisce un valore fisso.</span><span class="sxs-lookup"><span data-stu-id="52a73-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="52a73-142">Possibile riscrivere il test utilizzando il [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="52a73-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="52a73-143">Installare il `Moq` pacchetto NuGet nel progetto di test.</span><span class="sxs-lookup"><span data-stu-id="52a73-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="52a73-144">In questa versione, non è necessario impostare i dati di route, in quanto la simulazione **UrlHelper** restituisce una stringa costante.</span><span class="sxs-lookup"><span data-stu-id="52a73-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="52a73-145">Test per le azioni che restituiscono IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="52a73-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="52a73-146">In Web API 2, può restituire un'azione del controller **IHttpActionResult**, che è simile alla **ActionResult** in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="52a73-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="52a73-147">Il **IHttpActionResult** interfaccia definisce un modello di comando per la creazione di risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="52a73-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="52a73-148">Anziché creare direttamente la risposta, il controller restituisca un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="52a73-149">In un secondo momento, la pipeline richiama il **IHttpActionResult** per creare la risposta.</span><span class="sxs-lookup"><span data-stu-id="52a73-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="52a73-150">Questo approccio rende più semplice scrivere unit test, in quanto è possibile ignorare un lotto del programma di installazione necessari per **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="52a73-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="52a73-151">Ecco un esempio controller il cui ritorno azioni **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="52a73-152">Questo esempio illustra alcuni modelli comuni utilizzando **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="52a73-153">Di seguito viene illustrato come unità testarle.</span><span class="sxs-lookup"><span data-stu-id="52a73-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="52a73-154">Azione restituisce 200 (OK) con un corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="52a73-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="52a73-155">Il `Get` chiamate al metodo `Ok(product)` se viene trovato il prodotto.</span><span class="sxs-lookup"><span data-stu-id="52a73-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="52a73-156">Nello unit test, assicurarsi che il tipo restituito è **OkNegotiatedContentResult** e il prodotto restituito con l'ID corretto.</span><span class="sxs-lookup"><span data-stu-id="52a73-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="52a73-157">Si noti che lo unit test non esegue il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="52a73-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="52a73-158">È possibile presupporre che il risultato dell'azione consente di creare la risposta HTTP in modo corretto.</span><span class="sxs-lookup"><span data-stu-id="52a73-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="52a73-159">(Per tale motivo framework Web API con il proprio unit test!)</span><span class="sxs-lookup"><span data-stu-id="52a73-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="52a73-160">Azione restituisce 404 (non trovato)</span><span class="sxs-lookup"><span data-stu-id="52a73-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="52a73-161">Il `Get` chiamate al metodo `NotFound()` se non viene trovato il prodotto.</span><span class="sxs-lookup"><span data-stu-id="52a73-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="52a73-162">In questo caso, lo unit test verifica solo se il tipo restituito è **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="52a73-163">Azione restituisce 200 (OK) con alcun corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="52a73-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="52a73-164">Il `Delete` chiamate al metodo `Ok()` per restituire una risposta HTTP 200 vuota.</span><span class="sxs-lookup"><span data-stu-id="52a73-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="52a73-165">Come illustrato nell'esempio precedente, lo unit test controlla il tipo restituito, in questo caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="52a73-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="52a73-166">Azione restituisce 201 (creato) con un'intestazione di posizione</span><span class="sxs-lookup"><span data-stu-id="52a73-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="52a73-167">Il `Post` chiamate al metodo `CreatedAtRoute` per restituire una risposta HTTP 201 con un URI nell'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="52a73-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="52a73-168">Nello unit test, verificare che l'azione imposta i valori corretti di routing.</span><span class="sxs-lookup"><span data-stu-id="52a73-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="52a73-169">Azione restituisce un'altra 2xx con un corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="52a73-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="52a73-170">Il `Put` chiamate al metodo `Content` per restituire una risposta HTTP 202 (accettato) con un corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="52a73-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="52a73-171">In questo caso è simile alla restituzione di 200 (OK), ma lo unit test controllare inoltre il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="52a73-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="52a73-172">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="52a73-172">Additional Resources</span></span>

- [<span data-ttu-id="52a73-173">Tali Entity Framework quando gli Unit test ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="52a73-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="52a73-174">[Scrittura di test per un servizio ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di blog di Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="52a73-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="52a73-175">Debugging ASP.NET Web API with Route Debugger</span><span class="sxs-lookup"><span data-stu-id="52a73-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
