---
title: Formattare i dati di risposta nell'API Web ASP.NET Core
author: ardalis
description: Informazioni su come formattare i dati di risposta nell'API Web ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/formatting
ms.openlocfilehash: f25705bcd3a704de12ea24c3e196de1ba1afd492
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35217441"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="dc9f4-103">Formattare i dati di risposta nell'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc9f4-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="dc9f4-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="dc9f4-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="dc9f4-105">ASP.NET Core MVC include il supporto incorporato per la formattazione dei dati di risposta, tramite formati fissi o basati sulle specifiche dei client.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="dc9f4-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc9f4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="dc9f4-107">Risultati di azioni specifici per il formato</span><span class="sxs-lookup"><span data-stu-id="dc9f4-107">Format-Specific Action Results</span></span>

<span data-ttu-id="dc9f4-108">Alcuni tipi di risultati di azioni sono specifici di un particolare formato, ad esempio `JsonResult` e `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="dc9f4-109">Le azioni possono restituire risultati specifici che vengono sempre formattati in modo particolare.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="dc9f4-110">Con `JsonResult`, ad esempio, vengono restituiti dati in formato JSON, indipendentemente dalle preferenze del client.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="dc9f4-111">Analogamente, con `ContentResult` vengono restituiti dati stringa in formato testo normale (ovvero vengono semplicemente restituite stringhe).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="dc9f4-112">Un'azione non deve necessariamente restituire un tipo specifico. MVC supporta qualsiasi valore restituito oggetto.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="dc9f4-113">Se un'azione restituisce un'implementazione `IActionResult` e il controller eredita da `Controller`, gli sviluppatori hanno a disposizione molti metodi helper, corrispondenti a molte delle scelte.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="dc9f4-114">I risultati di azioni che restituiscono oggetti che non corrispondono a tipi `IActionResult` vengono serializzati tramite l'implementazione `IOutputFormatter` appropriata.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="dc9f4-115">Per restituire i dati in un formato specifico da un controller che eredita dalla classe di base `Controller`, usare il metodo helper predefinito `Json` per restituire JSON e `Content` per restituire testo normale.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="dc9f4-116">Il metodo di azione usato deve restituire il tipo di risultato specifico (ad esempio, `JsonResult`) o `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="dc9f4-117">Restituzione di dati in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="dc9f4-118">Risposta di esempio da questa azione:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-118">Sample response from this action:</span></span>

![Scheda Rete di Strumenti di sviluppo in Microsoft Edge che mostra che il tipo di contenuto della risposta è application/json](formatting/_static/json-response.png)

<span data-ttu-id="dc9f4-120">Si noti che il tipo di contenuto della risposta, `application/json`, è illustrato sia nell'elenco delle richieste di rete e nella sezione Intestazioni delle risposte.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="dc9f4-121">Si noti anche l'elenco delle opzioni visualizzate dal browser (in questo caso, Microsoft Edge) nell'intestazione Accept nella sezione Intestazioni delle risposte.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="dc9f4-122">La tecnica corrente ignora questa intestazione. Di seguito viene illustrato come seguirla.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="dc9f4-123">Per restituire dati in formato testo normale, usare `ContentResult` e l'helper `Content`:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="dc9f4-124">Una risposta da questa azione:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-124">A response from this action:</span></span>

![Scheda Rete di Strumenti di sviluppo in Microsoft Edge che mostra che il tipo di contenuto della risposta è text/plain](formatting/_static/text-response.png)

<span data-ttu-id="dc9f4-126">Si noti che in questo caso il `Content-Type` restituito è `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="dc9f4-127">È anche possibile ottenere lo stesso comportamento usando solo un tipo di risposta stringa:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="dc9f4-128">Per azioni non banali con più tipi restituiti o opzioni, ad esempio codici di stato HTTP diversi a seconda del risultato delle operazioni eseguite, preferire `IActionResult` come tipo restituito.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="dc9f4-129">Negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="dc9f4-129">Content Negotiation</span></span>

<span data-ttu-id="dc9f4-130">La negoziazione del contenuto (abbreviata in *conneg*) si verifica quando il client specifica un'[intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="dc9f4-131">Il formato predefinito usato da ASP.NET Core MVC è JSON.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="dc9f4-132">La negoziazione del contenuto viene implementata da `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="dc9f4-133">È anche incorporata nei risultati dell'azione specifica del codice stato restituiti dai metodi helper (tutti basati su `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="dc9f4-134">È anche possibile restituire un tipo di modello (una classe definita come tipo di trasferimento dei dati personalizzato). Il framework eseguirà automaticamente il wrapping in un `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="dc9f4-135">Il metodo di azione seguente usa i metodi helper `Ok` e `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="dc9f4-136">Verrà restituita una risposta in formato JSON, a meno che non sia stato richiesto un altro formato che il server è in grado di restituire.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="dc9f4-137">Per creare una richiesta che includa un'intestazione Accept e specificare un altro formato, è possibile usare uno strumento come [Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="dc9f4-138">In questo caso, se il server ha un *formattatore* in grado di generare una risposta nel formato richiesto, il risultato viene restituito nel formato preferito del client.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Console di Fiddler che mostra una richiesta GET creata manualmente con un'intestazione Accept corrispondente ad application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="dc9f4-140">Nello screenshot precedente, è stato usato Fiddler Composer per generare una richiesta, specificando `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="dc9f4-141">Per impostazione predefinita, ASP.NET Core MVC supporta solo il formato JSON. Anche se viene specificato un altro formato, quindi, il risultato restituito è comunque in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="dc9f4-142">Nella prossima sezione viene illustrato come aggiungere altri formattatori.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="dc9f4-143">Le azioni del controller possono restituire oggetti POCO (Plain Old CLR Object). In questo caso ASP.NET Core MVC crea automaticamente un `ObjectResult` per il wrapping dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="dc9f4-144">Il client otterrà l'oggetto serializzato formattato. L'impostazione predefinita è il formato JSON. È possibile configurare il formato XML o altri formati.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="dc9f4-145">Se l'oggetto restituito è `null`, il framework restituisce una risposta `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="dc9f4-146">Restituzione di un tipo di oggetto:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="dc9f4-147">Nell'esempio, la richiesta di un alias autore valido riceve una risposta 200 OK con i dati dell'autore.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="dc9f4-148">La richiesta di un alias non valido riceve una risposta 204 Nessun contenuto.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="dc9f4-149">Di seguito sono riportati gli screenshot con la risposta nei formati XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="dc9f4-150">Processo di negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="dc9f4-150">Content Negotiation Process</span></span>

<span data-ttu-id="dc9f4-151">La *negoziazione* del contenuto avviene solo se nella richiesta è presente un'intestazione `Accept`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="dc9f4-152">Se una richiesta contiene un'intestazione Accept, il framework enumera i tipi di supporti in tale intestazione in ordine di preferenza e tenta di trovare un formattatore in grado di generare una risposta in uno dei formati specificati dall'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="dc9f4-153">Nel caso in cui non venga trovato alcun formattatore in grado di soddisfare la richiesta del client, il framework tenta di trovare il primo formattatore in grado di generare una risposta, a meno che lo sviluppatore non abbia configurato in `MvcOptions` l'opzione per restituire l'errore 406 Pagina non valida.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="dc9f4-154">Se la richiesta specifica il formato XML, ma il formattatore XML non è stato configurato, viene usato il formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="dc9f4-155">Più in generale, se non è configurato alcun formattatore in grado di offrire il formato richiesto, viene usato il primo formattatore che possa formattare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="dc9f4-156">Se non viene specificata alcuna intestazione, viene usato il primo formattatore in grado di gestire l'oggetto da restituire.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="dc9f4-157">In questo caso, non avviene alcuna negoziazione. È il server a determinare il formato da usare.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="dc9f4-158">Se l'intestazione Accept contiene `*/*`, l'intestazione viene tenuta in considerazione solo se l'opzione `RespectBrowserAcceptHeader` è impostata su true in `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="dc9f4-159">Browser e negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="dc9f4-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="dc9f4-160">A differenza dei client API tipici, i Web browser tendono a fornire intestazioni `Accept` che includono un'ampia gamma di formati, inclusi i caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="dc9f4-161">Per impostazione predefinita, se il framework rileva che la richiesta proviene da un browser, ignora l'intestazione `Accept` e restituisce il contenuto nel formato predefinito dell'applicazione, ovvero nel formato JSON, se la configurazione non è stata modificata.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="dc9f4-162">Ciò offre un'esperienza più coerente quando si usano API con browser diversi.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="dc9f4-163">Se si preferisce che l'applicazione rispetti le intestazioni Accept del browser, è possibile configurare questo aspetto nell'ambito della configurazione di MVC impostando `RespectBrowserAcceptHeader` su `true` nel metodo `ConfigureServices` in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="dc9f4-164">Configurazione di formattatori</span><span class="sxs-lookup"><span data-stu-id="dc9f4-164">Configuring Formatters</span></span>

<span data-ttu-id="dc9f4-165">Se oltre al formato JSON predefinito l'applicazione deve supportare formati aggiuntivi, è possibile aggiungere pacchetti NuGet e configurare MVC in modo che possa supportarli.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="dc9f4-166">Esistono formattatori separati per input e output.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="dc9f4-167">I formattatori di input vengono usati dall'[associazione di modelli](xref:mvc/models/model-binding), mentre i formattatori di output vengono usati per formattare le risposte.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="dc9f4-168">È anche possibile configurare [formattatori personalizzati](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="dc9f4-169">Aggiunta del supporto per il formato XML</span><span class="sxs-lookup"><span data-stu-id="dc9f4-169">Adding XML Format Support</span></span>

<span data-ttu-id="dc9f4-170">Per aggiungere il supporto della formattazione XML, installare il pacchetto NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="dc9f4-171">Aggiungere XmlSerializerFormatters alla configurazione di MVC in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="dc9f4-172">In alternativa, è possibile aggiungere solo il formattatore di output:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="dc9f4-173">Questi due approcci serializzano i risultati tramite `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="dc9f4-174">Se si preferisce, è possibile usare `System.Runtime.Serialization.DataContractSerializer` tramite l'aggiunta del formattatore associato:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="dc9f4-175">Dopo aver aggiunto il supporto per la formattazione XML, i metodi del controller devono restituire il formato appropriato in base all'intestazione `Accept` della richiesta, come illustrato da questo esempio in Fiddler:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Console di Fiddler: la scheda Raw (Non elaborato) relativa alla richiesta mostra che il valore dell'intestazione Accept è application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="dc9f4-178">Nella scheda Inspectors (Controlli) è possibile vedere che per la richiesta GET in Raw (Non elaborato) è impostata un'intestazione `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="dc9f4-179">Il riquadro della risposta mostra l'intestazione `Content-Type: application/xml` e l'oggetto `Author` serializzato in XML.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="dc9f4-180">Usare la scheda Composer (Compositore) per modificare la richiesta, specificando `application/json` nell'intestazione `Accept`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="dc9f4-181">Eseguire la richiesta. La risposta verrà formattata come JSON:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Console di Fiddler: la scheda Raw (Non elaborato) relativa alla richiesta mostra che il valore dell'intestazione Accept è application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="dc9f4-184">In questo screenshot, è possibile vedere che la richiesta imposta un'intestazione corrispondente a `Accept: application/json` e che la risposta specifica lo stesso valore come `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="dc9f4-185">L'oggetto `Author` viene visualizzato nel corpo della risposta, in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="dc9f4-186">Imposizione di un formato specifico</span><span class="sxs-lookup"><span data-stu-id="dc9f4-186">Forcing a Particular Format</span></span>

<span data-ttu-id="dc9f4-187">Se si vogliono limitare i formati di risposta per un'azione specifica, è possibile applicare il filtro `[Produces]`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="dc9f4-188">Il filtro `[Produces]` specifica i formati di risposta per un'azione o per un controller specifico.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="dc9f4-189">Come la maggior parte dei [filtri](xref:mvc/controllers/filters), questo può essere applicato all'azione, al controller o all'ambito globale.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-189">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="dc9f4-190">Il filtro `[Produces]` impone a tutte le azioni all'interno di `AuthorsController` di restituire risposte in formato JSON, anche se altri formattatori configurati per l'applicazione e il client hanno fornito un'intestazione `Accept` che richiede un formato disponibile diverso.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="dc9f4-191">Per altre informazioni, ad esempio su come applicare filtri a livello globale, vedere [Filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-191">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="dc9f4-192">Formattatori per casi speciali</span><span class="sxs-lookup"><span data-stu-id="dc9f4-192">Special Case Formatters</span></span>

<span data-ttu-id="dc9f4-193">Alcuni casi speciali vengono implementati tramite formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="dc9f4-194">Per impostazione predefinita, i tipi restituiti `string` vengono formattati come *text/plain* (*text/html* se richiesto tramite intestazione `Accept`).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="dc9f4-195">È possibile rimuovere questo comportamento rimuovendo `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="dc9f4-196">È possibile rimuovere formattatori del metodo `Configure` in *Startup.cs* (illustrato di seguito).</span><span class="sxs-lookup"><span data-stu-id="dc9f4-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="dc9f4-197">Le azioni con un tipo restituito corrispondente a oggetto modello restituiscono una risposta 204 Nessun contenuto quando restituiscono `null`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="dc9f4-198">È possibile rimuovere questo comportamento rimuovendo `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="dc9f4-199">Il codice seguente rimuove `TextOutputFormatter` e `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="dc9f4-200">Senza `TextOutputFormatter`, i tipi restituiti `string` restituiscono, ad esempio, 406 Pagina non valida.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="dc9f4-201">Si noti che se `TextOutputFormatter` è stato rimosso ed è presente un formattatore XML, i tipi restituiti `string` vengono formattati da quest'ultimo.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="dc9f4-202">Senza `HttpNoContentOutputFormatter`, gli oggetti Null vengono formattati tramite il formattatore configurato.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="dc9f4-203">Ad esempio, il formattatore JSON si limita a restituire una risposta con corpo `null`, mentre il formattatore XML restituisce un elemento XML vuoto con l'attributo `xsi:nil="true"` impostato.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="dc9f4-204">Mapping di URL dei formati di risposta</span><span class="sxs-lookup"><span data-stu-id="dc9f4-204">Response Format URL Mappings</span></span>

<span data-ttu-id="dc9f4-205">I client possono richiedere un formato specifico all'interno dell'URL, ad esempio nella stringa di query o in una parte del percorso, oppure tramite un'estensione di file specifica di formato, ad esempio xml o json.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="dc9f4-206">Il mapping dal percorso della richiesta deve essere specificato nella route usata dall'API.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="dc9f4-207">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc9f4-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="dc9f4-208">Questa route consente di specificare il formato richiesto sotto forma di estensione di file facoltativa.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="dc9f4-209">L'attributo `[FormatFilter]` verifica l'esistenza del valore di formato in `RouteData` e quando la risposta viene creata esegue il mapping del formato della risposta al formattatore appropriato.</span><span class="sxs-lookup"><span data-stu-id="dc9f4-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>


|           <span data-ttu-id="dc9f4-210">Route</span><span class="sxs-lookup"><span data-stu-id="dc9f4-210">Route</span></span>            |             <span data-ttu-id="dc9f4-211">Formattatore</span><span class="sxs-lookup"><span data-stu-id="dc9f4-211">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="dc9f4-212">Formattatore di output predefinito</span><span class="sxs-lookup"><span data-stu-id="dc9f4-212">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="dc9f4-213">Formattatore JSON (se configurato)</span><span class="sxs-lookup"><span data-stu-id="dc9f4-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="dc9f4-214">Formattatore XML (se configurato)</span><span class="sxs-lookup"><span data-stu-id="dc9f4-214">The XML formatter (if configured)</span></span>  |

