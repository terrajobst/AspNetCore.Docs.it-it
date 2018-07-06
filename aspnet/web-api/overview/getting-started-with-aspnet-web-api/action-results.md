---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Azione risultante nell'API Web 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808927"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="f5c13-102">Risultati dell'azione nell'API Web 2</span><span class="sxs-lookup"><span data-stu-id="f5c13-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="f5c13-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5c13-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f5c13-104">In questo argomento viene descritto come API Web ASP.NET converte il valore restituito da un'azione del controller in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5c13-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="f5c13-105">Un'azione del controller API Web può restituire uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5c13-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="f5c13-106">void</span><span class="sxs-lookup"><span data-stu-id="f5c13-106">void</span></span>
2. <span data-ttu-id="f5c13-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f5c13-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="f5c13-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f5c13-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="f5c13-109">Un altro tipo</span><span class="sxs-lookup"><span data-stu-id="f5c13-109">Some other type</span></span>

<span data-ttu-id="f5c13-110">A seconda di quale di questi viene restituito, API Web Usa un meccanismo diverso per creare la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5c13-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="f5c13-111">Tipo restituito</span><span class="sxs-lookup"><span data-stu-id="f5c13-111">Return type</span></span> | <span data-ttu-id="f5c13-112">Come API Web crea la risposta</span><span class="sxs-lookup"><span data-stu-id="f5c13-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="f5c13-113">void</span><span class="sxs-lookup"><span data-stu-id="f5c13-113">void</span></span> | <span data-ttu-id="f5c13-114">Restituire vuoto 204 (nessun contenuto)</span><span class="sxs-lookup"><span data-stu-id="f5c13-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="f5c13-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f5c13-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="f5c13-116">Convertire direttamente in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5c13-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="f5c13-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f5c13-117">**IHttpActionResult**</span></span> | <span data-ttu-id="f5c13-118">Chiamare **ExecuteAsync** per creare un **HttpResponseMessage**, quindi convertire in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5c13-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="f5c13-119">Altro tipo</span><span class="sxs-lookup"><span data-stu-id="f5c13-119">Other type</span></span> | <span data-ttu-id="f5c13-120">Scrivere il valore restituito serializzato nel corpo della risposta; Restituisce il codice 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="f5c13-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="f5c13-121">La parte restante di questo argomento descrive ogni opzione in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="f5c13-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="f5c13-122">void</span><span class="sxs-lookup"><span data-stu-id="f5c13-122">void</span></span>

<span data-ttu-id="f5c13-123">Se il tipo restituito è `void`, API Web restituisce semplicemente una risposta HTTP vuota con il codice di stato 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="f5c13-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="f5c13-124">Controller di esempio:</span><span class="sxs-lookup"><span data-stu-id="f5c13-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="f5c13-125">Risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="f5c13-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="f5c13-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="f5c13-126">HttpResponseMessage</span></span>

<span data-ttu-id="f5c13-127">Se l'azione restituisce un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte il valore restituito direttamente in un messaggio di risposta HTTP, utilizzando le proprietà delle **HttpResponseMessage** oggetto per popolare il risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c13-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="f5c13-128">Questa opzione offre un notevole controllo sul messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c13-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="f5c13-129">Ad esempio, l'azione del controller seguente imposta l'intestazione Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="f5c13-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="f5c13-130">Risposta:</span><span class="sxs-lookup"><span data-stu-id="f5c13-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="f5c13-131">Se si passa un modello di dominio per il **CreateResponse** metodo, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per scrivere il modello serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c13-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="f5c13-132">API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="f5c13-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f5c13-133">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="f5c13-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="f5c13-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="f5c13-134">IHttpActionResult</span></span>

<span data-ttu-id="f5c13-135">Il **IHttpActionResult** interfaccia è stato introdotto in API Web 2.</span><span class="sxs-lookup"><span data-stu-id="f5c13-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="f5c13-136">In pratica, definisce un **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="f5c13-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="f5c13-137">Di seguito sono riportati alcuni vantaggi dell'uso di **IHttpActionResult** interfaccia:</span><span class="sxs-lookup"><span data-stu-id="f5c13-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="f5c13-138">Consente di semplificare [unit test](../testing-and-debugging/unit-testing-controllers-in-web-api.md) i controller.</span><span class="sxs-lookup"><span data-stu-id="f5c13-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="f5c13-139">Sposta la logica comune per la creazione di risposte HTTP in classi separate.</span><span class="sxs-lookup"><span data-stu-id="f5c13-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="f5c13-140">Rende l'intento dell'azione del controller più chiara, nascondendo i dettagli di basso livello di costruire la risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c13-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="f5c13-141">**IHttpActionResult** contiene un solo metodo, **ExecuteAsync**, che crea in modo asincrono un' **HttpResponseMessage** istanza.</span><span class="sxs-lookup"><span data-stu-id="f5c13-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="f5c13-142">Se un'azione del controller restituisce un **IHttpActionResult**, chiamate all'API Web di **ExecuteAsync** metodo per creare un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="f5c13-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="f5c13-143">Viene poi convertita la **HttpResponseMessage** in un messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5c13-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="f5c13-144">Ecco una semplice implementazione di **IHttpActionResult** che crea una risposta di testo normale:</span><span class="sxs-lookup"><span data-stu-id="f5c13-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="f5c13-145">Azione del controller di esempio:</span><span class="sxs-lookup"><span data-stu-id="f5c13-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="f5c13-146">Risposta:</span><span class="sxs-lookup"><span data-stu-id="f5c13-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="f5c13-147">Più spesso, si userà il **IHttpActionResult** implementazioni definite nel **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f5c13-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="f5c13-148">Il **ApiController** classe definisce i metodi helper che restituiscono queste risultati dell'azione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f5c13-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="f5c13-149">Nell'esempio seguente, se la richiesta non corrisponde a un ID prodotto esistente, il controller chiama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) per creare una risposta 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="f5c13-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="f5c13-150">In caso contrario, il controller chiama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), che consente di creare una risposta 200 (OK) che contiene il prodotto.</span><span class="sxs-lookup"><span data-stu-id="f5c13-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="f5c13-151">Altri tipi restituiti</span><span class="sxs-lookup"><span data-stu-id="f5c13-151">Other Return Types</span></span>

<span data-ttu-id="f5c13-152">Per tutti gli altri tipi restituiti, API Web Usa un [formattatore di media](../formats-and-model-binding/media-formatters.md) per serializzare il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="f5c13-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="f5c13-153">API Web scrive il valore serializzato nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="f5c13-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="f5c13-154">Il codice di stato della risposta è 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="f5c13-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="f5c13-155">Uno svantaggio di questo approccio è che è possibile restituire direttamente un codice di errore, ad esempio 404.</span><span class="sxs-lookup"><span data-stu-id="f5c13-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="f5c13-156">Tuttavia, è possibile generare una **HttpResponseException** i codici di errore.</span><span class="sxs-lookup"><span data-stu-id="f5c13-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="f5c13-157">Per altre informazioni, vedere [gestione delle eccezioni nell'API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="f5c13-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="f5c13-158">API Web Usa l'intestazione Accept nella richiesta per scegliere il formattatore.</span><span class="sxs-lookup"><span data-stu-id="f5c13-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f5c13-159">Per altre informazioni, vedere [negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="f5c13-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="f5c13-160">Richiesta di esempio</span><span class="sxs-lookup"><span data-stu-id="f5c13-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="f5c13-161">Esempio di risposta:</span><span class="sxs-lookup"><span data-stu-id="f5c13-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
