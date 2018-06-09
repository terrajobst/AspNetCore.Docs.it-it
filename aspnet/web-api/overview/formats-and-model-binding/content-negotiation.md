---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Contenuti di negoziazione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Descrive la modalità di implementazione negoziazione del contenuto HTTP in ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507020"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="e1f36-103">Negoziazione del contenuto in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e1f36-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e1f36-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e1f36-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e1f36-105">In questo articolo viene descritto come ASP.NET Web API implementa negoziazione del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="e1f36-106">La specifica di HTTP (RFC 2616) definisce negoziazione del contenuto come "il processo di selezione la migliore rappresentazione di una risposta specificata quando sono disponibili più rappresentazioni".</span><span class="sxs-lookup"><span data-stu-id="e1f36-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="e1f36-107">Il meccanismo principale per la negoziazione del contenuto HTTP sono queste intestazioni della richiesta:</span><span class="sxs-lookup"><span data-stu-id="e1f36-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="e1f36-108">**Accettare:** quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzata, ad esempio &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="e1f36-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="e1f36-109">**Charset Accept:** i set di caratteri accettabili, ad esempio UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="e1f36-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="e1f36-110">**Codifica:** le codifiche di contenuto sono accettabili, ad esempio gzip.</span><span class="sxs-lookup"><span data-stu-id="e1f36-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="e1f36-111">**Accept-Language:** il linguaggio naturale preferito, ad esempio "en-us".</span><span class="sxs-lookup"><span data-stu-id="e1f36-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="e1f36-112">Il server può inoltre esaminare altre parti della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1f36-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="e1f36-113">Ad esempio, se la richiesta contiene un'intestazione X-Requested-With, che indica una richiesta AJAX, il server potrebbe predefinito per JSON se è presente alcuna intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="e1f36-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="e1f36-114">In questo articolo verranno esaminate le intestazioni Accept e Accept-Charset di utilizzo delle API Web.</span><span class="sxs-lookup"><span data-stu-id="e1f36-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="e1f36-115">(In questo momento, non vi è alcun supporto predefinito per Accept-Encoding o Accept-Language).</span><span class="sxs-lookup"><span data-stu-id="e1f36-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="e1f36-116">Serializzazione</span><span class="sxs-lookup"><span data-stu-id="e1f36-116">Serialization</span></span>

<span data-ttu-id="e1f36-117">Se un controller API Web restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e di scriverla nel corpo della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1f36-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="e1f36-118">Si consideri ad esempio l'azione del controller seguente:</span><span class="sxs-lookup"><span data-stu-id="e1f36-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="e1f36-119">Un client può inviare la richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="e1f36-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="e1f36-120">In risposta, il server potrebbe inviare:</span><span class="sxs-lookup"><span data-stu-id="e1f36-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="e1f36-121">In questo esempio, il client ha richiesto JSON, Javascript o "qualsiasi" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="e1f36-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="e1f36-122">Il server è disponibile una rappresentazione JSON del `Product` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="e1f36-123">Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="e1f36-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="e1f36-124">Un controller può restituire anche un **HttpResponseMessage** oggetto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="e1f36-125">Per specificare un oggetto CLR per il corpo della risposta, chiamare il **CreateResponse** metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="e1f36-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="e1f36-126">Questa opzione garantisce un maggiore controllo sui dettagli della risposta.</span><span class="sxs-lookup"><span data-stu-id="e1f36-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="e1f36-127">È possibile impostare il codice di stato, aggiungere le intestazioni HTTP e così via.</span><span class="sxs-lookup"><span data-stu-id="e1f36-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="e1f36-128">L'oggetto che serializza la risorsa viene chiamato un *formattatore di media*.</span><span class="sxs-lookup"><span data-stu-id="e1f36-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="e1f36-129">Formattatori di Media derivano il **MediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="e1f36-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="e1f36-130">API Web fornisce formattatori di media per XML e JSON ed è possibile creare formattatori personalizzati per supportare altri tipi di supporti.</span><span class="sxs-lookup"><span data-stu-id="e1f36-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="e1f36-131">Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori di Media](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="e1f36-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="e1f36-132">Funzionamento di negoziazione come contenuto</span><span class="sxs-lookup"><span data-stu-id="e1f36-132">How Content Negotiation Works</span></span>

<span data-ttu-id="e1f36-133">Ottiene prima di tutto, la pipeline di **IContentNegotiator** servizio il **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="e1f36-134">Ottiene anche l'elenco di formattatori di media dal **HttpConfiguration.Formatters** insieme.</span><span class="sxs-lookup"><span data-stu-id="e1f36-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="e1f36-135">Successivamente, la pipeline chiama **IContentNegotiatior.Negotiate**, passando:</span><span class="sxs-lookup"><span data-stu-id="e1f36-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="e1f36-136">Il tipo di oggetto da serializzare</span><span class="sxs-lookup"><span data-stu-id="e1f36-136">The type of object to serialize</span></span>
- <span data-ttu-id="e1f36-137">La raccolta di formattatori di media</span><span class="sxs-lookup"><span data-stu-id="e1f36-137">The collection of media formatters</span></span>
- <span data-ttu-id="e1f36-138">La richiesta HTTP</span><span class="sxs-lookup"><span data-stu-id="e1f36-138">The HTTP request</span></span>

<span data-ttu-id="e1f36-139">Il **Negotiate** restituisce due tipi di informazioni:</span><span class="sxs-lookup"><span data-stu-id="e1f36-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="e1f36-140">Il formattatore da utilizzare</span><span class="sxs-lookup"><span data-stu-id="e1f36-140">Which formatter to use</span></span>
- <span data-ttu-id="e1f36-141">Il tipo di supporto per la risposta</span><span class="sxs-lookup"><span data-stu-id="e1f36-141">The media type for the response</span></span>

<span data-ttu-id="e1f36-142">Se non viene trovato alcun formattatore, il **Negotiate** restituisce **null**e l'errore del client riceve HTTP 406 (non valida).</span><span class="sxs-lookup"><span data-stu-id="e1f36-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="e1f36-143">Il codice seguente viene illustrato come un controller può richiamare direttamente negoziazione del contenuto:</span><span class="sxs-lookup"><span data-stu-id="e1f36-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="e1f36-144">Questo codice è equivalente per la pipeline viene eseguita automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e1f36-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="e1f36-145">Negoziatore del contenuto predefinito</span><span class="sxs-lookup"><span data-stu-id="e1f36-145">Default Content Negotiator</span></span>

<span data-ttu-id="e1f36-146">Il **DefaultContentNegotiator** classe fornisce l'implementazione predefinita di **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="e1f36-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="e1f36-147">Per selezionare un formattatore Usa diversi criteri.</span><span class="sxs-lookup"><span data-stu-id="e1f36-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="e1f36-148">In primo luogo, il formattatore deve essere in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="e1f36-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="e1f36-149">Questa operazione viene verificata chiamando **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="e1f36-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="e1f36-150">Successivamente, il negoziatore del contenuto esamina ciascun formattatore e valuta l'accuratezza corrisponde alla richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e1f36-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="e1f36-151">Per valutare la corrispondenza, il negoziatore del contenuto esamina due operazioni sul formattatore:</span><span class="sxs-lookup"><span data-stu-id="e1f36-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="e1f36-152">Il **SupportedMediaTypes** insieme, che contiene un elenco di tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="e1f36-153">Negoziatore del contenuto tenta di associare l'elenco di intestazione Accept della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e1f36-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="e1f36-154">Si noti che l'intestazione Accept può includere gli intervalli.</span><span class="sxs-lookup"><span data-stu-id="e1f36-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="e1f36-155">Ad esempio, "text/plain" è una corrispondenza per il testo /\* o \* / \*.</span><span class="sxs-lookup"><span data-stu-id="e1f36-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="e1f36-156">Il **MediaTypeMappings** insieme, che contiene un elenco di **MediaTypeMapping** oggetti.</span><span class="sxs-lookup"><span data-stu-id="e1f36-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="e1f36-157">Il **MediaTypeMapping** classe fornisce un modo generico per la corrispondenza di richieste HTTP con i tipi di supporto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="e1f36-158">Ad esempio, è possibile mappare un'intestazione HTTP personalizzata per un determinato tipo di supporto.</span><span class="sxs-lookup"><span data-stu-id="e1f36-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="e1f36-159">Se sono presenti più corrispondenze, la corrispondenza con il server wins fattore qualità più elevata.</span><span class="sxs-lookup"><span data-stu-id="e1f36-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="e1f36-160">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e1f36-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="e1f36-161">In questo esempio, application/json è un fattore di qualità implicita pari a 1,0, pertanto è preferibile su application/xml.</span><span class="sxs-lookup"><span data-stu-id="e1f36-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="e1f36-162">Se non vengono trovate corrispondenze, il negoziatore del contenuto tenta di corrispondenza per il tipo di supporto del corpo della richiesta, se presente.</span><span class="sxs-lookup"><span data-stu-id="e1f36-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="e1f36-163">Ad esempio, se la richiesta contiene i dati JSON, negoziatore del contenuto Cerca un formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="e1f36-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="e1f36-164">Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.</span><span class="sxs-lookup"><span data-stu-id="e1f36-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="e1f36-165">Selezionare una codifica dei caratteri</span><span class="sxs-lookup"><span data-stu-id="e1f36-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="e1f36-166">Dopo aver selezionato un formattatore, negoziatore del contenuto sceglie la migliore codifica dei caratteri esaminando il **SupportedEncodings** proprietà, il formattatore e trovare una corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).</span><span class="sxs-lookup"><span data-stu-id="e1f36-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
