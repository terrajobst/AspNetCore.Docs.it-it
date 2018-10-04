---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Unit test controller in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller nell'API Web 2. Prima di leggere questo argomento, è possibile leggere l'esercitazione unità...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794824"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="98987-104">Unit test controller in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="98987-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="98987-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98987-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="98987-106">In questo argomento vengono descritte alcune tecniche specifiche per gli unit test controller nell'API Web 2.</span><span class="sxs-lookup"><span data-stu-id="98987-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="98987-107">Prima di leggere questo argomento, è possibile leggere l'esercitazione [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), che illustra come aggiungere un progetto di unit test alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="98987-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="98987-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="98987-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="98987-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="98987-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="98987-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="98987-110">Web API 2</span></span>
> - <span data-ttu-id="98987-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="98987-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="98987-112">Ho utilizzato Moq, ma lo stesso concetto si applica a qualsiasi framework di simulazione.</span><span class="sxs-lookup"><span data-stu-id="98987-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="98987-113">Moq 4.5.30 (e versioni successive) supporta Visual Studio 2017, Roslyn e .NET 4.5 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="98987-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="98987-114">È comune negli unit test &quot;Disponi-act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="98987-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="98987-115">Disponi: Configurare i prerequisiti per eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="98987-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="98987-116">ACT: Eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="98987-116">Act: Perform the test.</span></span>
- <span data-ttu-id="98987-117">Assert: Verificare che il test ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="98987-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="98987-118">Nel passaggio arrange, si utilizzeranno spesso simulazione o gli oggetti stub.</span><span class="sxs-lookup"><span data-stu-id="98987-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="98987-119">Che riduce al minimo il numero di dipendenze, in modo che il test è incentrato su un'operazione di test.</span><span class="sxs-lookup"><span data-stu-id="98987-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="98987-120">Ecco alcuni aspetti che occorre inserire unit test nei controller API Web:</span><span class="sxs-lookup"><span data-stu-id="98987-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="98987-121">L'azione restituisce il tipo corretto di risposta.</span><span class="sxs-lookup"><span data-stu-id="98987-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="98987-122">Parametri non validi restituiscono la risposta di errore corretto.</span><span class="sxs-lookup"><span data-stu-id="98987-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="98987-123">L'azione chiama il metodo corretto sul livello del repository o un servizio.</span><span class="sxs-lookup"><span data-stu-id="98987-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="98987-124">Se la risposta include un modello di dominio, verificare il tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="98987-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="98987-125">Questi sono alcuni degli aspetti generali da testare, ma gli aspetti specifici dipendono dall'implementazione del controller.</span><span class="sxs-lookup"><span data-stu-id="98987-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="98987-126">In particolare, fa la differenza che indica se restituiscono le azioni del controller **HttpResponseMessage** oppure **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="98987-127">Per altre informazioni su questi tipi di risultati, vedere [risultati delle azioni nell'Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="98987-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="98987-128">Test per le azioni che restituiscono HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="98987-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="98987-129">Di seguito è riportato un esempio di un controller di cui restituire le azioni **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="98987-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="98987-130">Si noti che il controller Usa l'inserimento delle dipendenze per inserire un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="98987-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="98987-131">Modo più testabili, il controller perché è possibile inserire un repository fittizio.</span><span class="sxs-lookup"><span data-stu-id="98987-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="98987-132">Lo unit test seguente verifica che il `Get` scritture metodo un `Product` nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="98987-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="98987-133">Si supponga che `repository` è una simulazione `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="98987-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="98987-134">È importante impostare **richiedere** e **configurazione** nel controller.</span><span class="sxs-lookup"><span data-stu-id="98987-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="98987-135">In caso contrario, il test avrà esito negativo con un **ArgumentNullException** oppure **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="98987-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="98987-136">Generazione di collegamenti di test</span><span class="sxs-lookup"><span data-stu-id="98987-136">Testing Link Generation</span></span>

<span data-ttu-id="98987-137">Il `Post` chiamate al metodo **UrlHelper.Link** per creare i collegamenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="98987-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="98987-138">Questa operazione richiede un po' più il programma di installazione dello unit test:</span><span class="sxs-lookup"><span data-stu-id="98987-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="98987-139">Il **UrlHelper** classe necessita dei dati di route e URL di richiesta, in modo che il test ha impostare i valori per questi.</span><span class="sxs-lookup"><span data-stu-id="98987-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="98987-140">Un'altra opzione consiste simulazione o stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="98987-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="98987-141">Con questo approccio, sostituire il valore predefinito di [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versione fittizi e stub che restituisce un valore fisso.</span><span class="sxs-lookup"><span data-stu-id="98987-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="98987-142">È possibile riscrivere il test utilizzando la [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="98987-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="98987-143">Installare il `Moq` pacchetto NuGet nel progetto di test.</span><span class="sxs-lookup"><span data-stu-id="98987-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="98987-144">In questa versione, non è necessario configurare eventuali dati di route, perché la simulazione **UrlHelper** restituisce una stringa costante.</span><span class="sxs-lookup"><span data-stu-id="98987-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="98987-145">Test per le azioni che restituiscono IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="98987-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="98987-146">Nell'API Web 2, è possibile restituire un'azione del controller **IHttpActionResult**, che è simile alla **ActionResult** in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="98987-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="98987-147">Il **IHttpActionResult** interfaccia definisce un modello di comando per la creazione di risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="98987-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="98987-148">Anziché creare direttamente la risposta, il controller restituisce un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="98987-149">In un secondo momento, la pipeline richiama il **IHttpActionResult** per creare la risposta.</span><span class="sxs-lookup"><span data-stu-id="98987-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="98987-150">Questo approccio rende più semplice scrivere unit test, perché è possibile ignorare molti il programma di installazione necessario per **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="98987-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="98987-151">Ecco un esempio controller cui restituire le azioni **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="98987-152">In questo esempio illustra alcuni modelli comuni di uso **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="98987-153">Di seguito viene illustrato come eseguire lo unit test li.</span><span class="sxs-lookup"><span data-stu-id="98987-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="98987-154">Azione restituisce 200 (OK) con un corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="98987-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="98987-155">Il `Get` chiamate al metodo `Ok(product)` se viene trovato il prodotto.</span><span class="sxs-lookup"><span data-stu-id="98987-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="98987-156">Nello unit test, assicurarsi che il tipo restituito sia **OkNegotiatedContentResult** e il prodotto restituito con l'ID corretto.</span><span class="sxs-lookup"><span data-stu-id="98987-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="98987-157">Si noti che lo unit test non viene eseguito il risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="98987-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="98987-158">Si può presupporre che il risultato dell'azione consente di creare la risposta HTTP corretta.</span><span class="sxs-lookup"><span data-stu-id="98987-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="98987-159">(Questo motivo il framework API Web ha un proprio unit test!)</span><span class="sxs-lookup"><span data-stu-id="98987-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="98987-160">Azione restituisce 404 (non trovato)</span><span class="sxs-lookup"><span data-stu-id="98987-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="98987-161">Il `Get` chiamate al metodo `NotFound()` se il prodotto non viene trovato.</span><span class="sxs-lookup"><span data-stu-id="98987-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="98987-162">In questo caso, lo unit test verifica solo se il tipo restituito è **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="98987-163">Azione restituisce 200 (OK) senza corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="98987-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="98987-164">Il `Delete` chiamate al metodo `Ok()` per restituire una risposta HTTP 200 vuota.</span><span class="sxs-lookup"><span data-stu-id="98987-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="98987-165">Uguale all'esempio precedente, lo unit test verifica il tipo restituito, in questo caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="98987-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="98987-166">Azione restituisce 201 (creato) con un'intestazione di posizione</span><span class="sxs-lookup"><span data-stu-id="98987-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="98987-167">Il `Post` chiamate al metodo `CreatedAtRoute` per restituire una risposta HTTP 201 con l'URI nell'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="98987-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="98987-168">Nello unit test, verificare che l'azione imposta i valori di routing corretti.</span><span class="sxs-lookup"><span data-stu-id="98987-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="98987-169">Azione restituisce un'altra 2xx con un corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="98987-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="98987-170">Il `Put` chiamate al metodo `Content` per restituire una risposta HTTP 202 (accettato) con un corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="98987-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="98987-171">In questo caso è simile a restituire 200 (OK), ma lo unit test dovrà verificare anche il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="98987-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="98987-172">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="98987-172">Additional Resources</span></span>

- [<span data-ttu-id="98987-173">Comportamento fittizio di Entity Framework quando gli Unit test ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="98987-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="98987-174">[Scrittura di test per un servizio API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (post di blog di Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="98987-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="98987-175">Debug di ASP.NET Web API with Route Debugger</span><span class="sxs-lookup"><span data-stu-id="98987-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
