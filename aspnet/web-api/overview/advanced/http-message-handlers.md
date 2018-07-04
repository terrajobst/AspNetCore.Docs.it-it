---
uid: web-api/overview/advanced/http-message-handlers
title: Gestori di messaggi HTTP nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: daf5bac8203beca3be0036b798188e373b02212a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394521"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="6c745-102">Gestori di messaggi HTTP nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6c745-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="6c745-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6c745-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6c745-104">Oggetto *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c745-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="6c745-105">Gestori di messaggi derivano dalla classe astratta **HttpMessageHandler** classe.</span><span class="sxs-lookup"><span data-stu-id="6c745-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="6c745-106">In genere, una serie di gestori di messaggi vengono concatenati.</span><span class="sxs-lookup"><span data-stu-id="6c745-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="6c745-107">Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e offre la richiesta al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="6c745-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="6c745-108">A un certo punto, la risposta viene creata e viene reimpostato fino alla catena.</span><span class="sxs-lookup"><span data-stu-id="6c745-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="6c745-109">Questo modello viene chiamato un *delega* gestore.</span><span class="sxs-lookup"><span data-stu-id="6c745-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="6c745-110">Gestori di messaggi sul lato server</span><span class="sxs-lookup"><span data-stu-id="6c745-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="6c745-111">Sul lato server, la pipeline di API Web Usa alcuni gestori di messaggi predefinito:</span><span class="sxs-lookup"><span data-stu-id="6c745-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="6c745-112">**HttpServer** riceve la richiesta dall'host.</span><span class="sxs-lookup"><span data-stu-id="6c745-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="6c745-113">**HttpRoutingDispatcher** invia la richiesta in base alla route.</span><span class="sxs-lookup"><span data-stu-id="6c745-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="6c745-114">**HttpControllerDispatcher** invia la richiesta a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="6c745-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="6c745-115">È possibile aggiungere gestori personalizzati per la pipeline.</span><span class="sxs-lookup"><span data-stu-id="6c745-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="6c745-116">Gestori di messaggi sono ideali per le problematiche trasversali che operano a livello di HTTP messaggi (anziché le azioni del controller).</span><span class="sxs-lookup"><span data-stu-id="6c745-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="6c745-117">Ad esempio, potrebbe essere un gestore di messaggi:</span><span class="sxs-lookup"><span data-stu-id="6c745-117">For example, a message handler might:</span></span>

- <span data-ttu-id="6c745-118">Leggere o modificare le intestazioni di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6c745-118">Read or modify request headers.</span></span>
- <span data-ttu-id="6c745-119">Aggiungere un'intestazione di risposta per le risposte.</span><span class="sxs-lookup"><span data-stu-id="6c745-119">Add a response header to responses.</span></span>
- <span data-ttu-id="6c745-120">Convalidare le richieste prima che raggiungano il controller.</span><span class="sxs-lookup"><span data-stu-id="6c745-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="6c745-121">Questo diagramma mostra due gestori personalizzati inseriti nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="6c745-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="6c745-122">Sul lato client, HttpClient Usa anche i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="6c745-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="6c745-123">Per altre informazioni, vedere [gestori di messaggi HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="6c745-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="6c745-124">Gestori di messaggi personalizzato</span><span class="sxs-lookup"><span data-stu-id="6c745-124">Custom Message Handlers</span></span>

<span data-ttu-id="6c745-125">Per scrivere un gestore di messaggi personalizzato, derivare da **System.Net.Http.DelegatingHandler** ed eseguire l'override di **SendAsync** (metodo).</span><span class="sxs-lookup"><span data-stu-id="6c745-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="6c745-126">Questo metodo ha la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="6c745-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="6c745-127">Il metodo accetta un **HttpRequestMessage** come input e in modo asincrono restituisce un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="6c745-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="6c745-128">Una tipica implementazione esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6c745-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="6c745-129">Elaborare il messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6c745-129">Process the request message.</span></span>
2. <span data-ttu-id="6c745-130">Chiamare `base.SendAsync` per inviare la richiesta per il gestore interno.</span><span class="sxs-lookup"><span data-stu-id="6c745-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="6c745-131">Il gestore interno restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="6c745-131">The inner handler returns a response message.</span></span> <span data-ttu-id="6c745-132">(Questo passaggio è asincrono).</span><span class="sxs-lookup"><span data-stu-id="6c745-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="6c745-133">Elaborare la risposta e restituirlo al chiamante.</span><span class="sxs-lookup"><span data-stu-id="6c745-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="6c745-134">Ecco un esempio semplice:</span><span class="sxs-lookup"><span data-stu-id="6c745-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="6c745-135">La chiamata a `base.SendAsync` è asincrona.</span><span class="sxs-lookup"><span data-stu-id="6c745-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="6c745-136">Se il gestore esegue qualsiasi attività dopo questa chiamata, usare il **await** (parola chiave), come illustrato.</span><span class="sxs-lookup"><span data-stu-id="6c745-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="6c745-137">Un gestore delega può anche ignorare il gestore interno e creare direttamente la risposta:</span><span class="sxs-lookup"><span data-stu-id="6c745-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="6c745-138">Se una delega gestore crea la risposta senza chiamare `base.SendAsync`, la richiesta ignora il resto della pipeline.</span><span class="sxs-lookup"><span data-stu-id="6c745-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="6c745-139">Ciò può essere utile per un gestore che convalida la richiesta (creazione di una risposta di errore).</span><span class="sxs-lookup"><span data-stu-id="6c745-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="6c745-140">Aggiunta di un gestore per la Pipeline</span><span class="sxs-lookup"><span data-stu-id="6c745-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="6c745-141">Per aggiungere un gestore di messaggi sul lato server, aggiungere il gestore per il **HttpConfiguration.MessageHandlers** raccolta.</span><span class="sxs-lookup"><span data-stu-id="6c745-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="6c745-142">Se si usa il modello "Applicazione Web di ASP.NET MVC 4" per creare il progetto, non è presente all'interno di **WebApiConfig** classe:</span><span class="sxs-lookup"><span data-stu-id="6c745-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="6c745-143">Vengono chiamati i gestori di messaggi nello stesso ordine in cui appaiono nel **MessageHandlers** raccolta.</span><span class="sxs-lookup"><span data-stu-id="6c745-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="6c745-144">Perché sono nidificati, il messaggio di risposta viene trasferita in altra direzione.</span><span class="sxs-lookup"><span data-stu-id="6c745-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="6c745-145">L'ultimo gestore, ovvero è il primo per ottenere il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="6c745-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="6c745-146">Si noti che non è necessario impostare gestori interni; il framework API Web si connette automaticamente i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="6c745-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="6c745-147">Se sei [self-hosting](../older-versions/self-host-a-web-api.md), creare un'istanza del **HttpSelfHostConfiguration** classe e aggiungere i gestori per il **MessageHandlers** raccolta.</span><span class="sxs-lookup"><span data-stu-id="6c745-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="6c745-148">Ora verranno esaminati alcuni esempi di gestori di messaggi personalizzati.</span><span class="sxs-lookup"><span data-stu-id="6c745-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="6c745-149">Esempio: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="6c745-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="6c745-150">X-HTTP-Method-Override è un'intestazione HTTP non standard.</span><span class="sxs-lookup"><span data-stu-id="6c745-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="6c745-151">È progettato per i client che non è possibile inviare determinati tipi di richiesta HTTP, ad esempio PUT o DELETE.</span><span class="sxs-lookup"><span data-stu-id="6c745-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="6c745-152">Al contrario, il client invia una richiesta POST e imposta l'intestazione X-HTTP-Method-Override il metodo desiderato.</span><span class="sxs-lookup"><span data-stu-id="6c745-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="6c745-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c745-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="6c745-154">Di seguito è riportato un gestore di messaggi che aggiunge il supporto per X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="6c745-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="6c745-155">Nel **SendAsync** metodo, il gestore verifica se il messaggio di richiesta è una richiesta POST, e se contiene l'intestazione X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="6c745-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="6c745-156">In questo caso, convalida il valore dell'intestazione e quindi modifica il metodo di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6c745-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="6c745-157">Infine, il gestore chiama `base.SendAsync` per passare il messaggio al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="6c745-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="6c745-158">Quando la richiesta raggiunga la **HttpControllerDispatcher** (classe), **HttpControllerDispatcher** instrada la richiesta in base al metodo di richiesta di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6c745-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="6c745-159">Esempio: Aggiunta di un'intestazione di risposta personalizzata</span><span class="sxs-lookup"><span data-stu-id="6c745-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="6c745-160">Di seguito è riportato un gestore di messaggi che aggiunge un'intestazione personalizzata a ogni messaggio di risposta:</span><span class="sxs-lookup"><span data-stu-id="6c745-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="6c745-161">In primo luogo, il gestore chiama `base.SendAsync` per passare la richiesta al gestore di messaggi interno.</span><span class="sxs-lookup"><span data-stu-id="6c745-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="6c745-162">Il gestore interno restituisce un messaggio di risposta, ma lo fa in modo asincrono usando una **Task&lt;T&gt;**  oggetto.</span><span class="sxs-lookup"><span data-stu-id="6c745-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="6c745-163">Il messaggio di risposta è disponibile solo `base.SendAsync` viene completata in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6c745-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="6c745-164">Questo esempio Usa la **await** parola chiave per eseguire operazioni in modo asincrono dopo `SendAsync` viene completata.</span><span class="sxs-lookup"><span data-stu-id="6c745-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="6c745-165">Se la destinazione è .NET Framework 4.0, utilizzare il **Task**&lt;T&gt;**. ContinueWith** metodo:</span><span class="sxs-lookup"><span data-stu-id="6c745-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="6c745-166">Esempio: Cercare una chiave API</span><span class="sxs-lookup"><span data-stu-id="6c745-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="6c745-167">Alcuni servizi web richiedono i client includere una chiave API nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="6c745-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="6c745-168">Nell'esempio seguente viene illustrato come un gestore di messaggi può controllare le richieste per una chiave API valida:</span><span class="sxs-lookup"><span data-stu-id="6c745-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="6c745-169">Questo gestore di è simile per la chiave API nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="6c745-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="6c745-170">(In questo esempio si presuppone che la chiave è una stringa statica.</span><span class="sxs-lookup"><span data-stu-id="6c745-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="6c745-171">Un'implementazione reale preferiranno utilizzare convalida più complessa.) Se la stringa di query contiene la chiave, il gestore passa la richiesta per il gestore interno.</span><span class="sxs-lookup"><span data-stu-id="6c745-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="6c745-172">Se la richiesta non ha una chiave valida, il gestore consente di creare un messaggio di risposta con stato, 403 accesso negato.</span><span class="sxs-lookup"><span data-stu-id="6c745-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="6c745-173">In questo caso, il gestore non chiama `base.SendAsync`, pertanto, la richiesta non riceve mai il gestore interno e neppure il controller.</span><span class="sxs-lookup"><span data-stu-id="6c745-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="6c745-174">Pertanto, il controller può presupporre che tutte le richieste in ingresso hanno una chiave API valida.</span><span class="sxs-lookup"><span data-stu-id="6c745-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="6c745-175">Se la chiave API si applica solo a determinate azioni del controller, prendere in considerazione l'uso di un filtro azione invece di un gestore di messaggi.</span><span class="sxs-lookup"><span data-stu-id="6c745-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="6c745-176">I filtri azione eseguiti dopo l'esecuzione dell'URI di routing.</span><span class="sxs-lookup"><span data-stu-id="6c745-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="6c745-177">Gestori di messaggi per ogni Route</span><span class="sxs-lookup"><span data-stu-id="6c745-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="6c745-178">I gestori nel **HttpConfiguration.MessageHandlers** raccolta si applicano globalmente.</span><span class="sxs-lookup"><span data-stu-id="6c745-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="6c745-179">In alternativa, è possibile aggiungere un gestore di messaggi a una route specifica quando si definisce la route:</span><span class="sxs-lookup"><span data-stu-id="6c745-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="6c745-180">In questo esempio, se l'URI della richiesta corrisponde a "Route2", la richiesta viene inviata al `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="6c745-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="6c745-181">Il diagramma seguente illustra la pipeline per queste due route:</span><span class="sxs-lookup"><span data-stu-id="6c745-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="6c745-182">Si noti che `MessageHandler2` sostituisce il valore predefinito **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="6c745-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="6c745-183">In questo esempio `MessageHandler2` crea la risposta, mentre le richieste che corrispondono a "Route2" mai passare a un controller.</span><span class="sxs-lookup"><span data-stu-id="6c745-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="6c745-184">Ciò consente di sostituire l'intero meccanismo controller API Web con il proprio endpoint personalizzati.</span><span class="sxs-lookup"><span data-stu-id="6c745-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="6c745-185">In alternativa, un gestore di messaggi per ogni route può delegare al **HttpControllerDispatcher**, che quindi lo invia a un controller.</span><span class="sxs-lookup"><span data-stu-id="6c745-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="6c745-186">Il codice seguente viene illustrato come configurare questa route:</span><span class="sxs-lookup"><span data-stu-id="6c745-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
