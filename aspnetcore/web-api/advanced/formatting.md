---
title: Formattare i dati di risposta nell'API Web ASP.NET Core
author: ardalis
description: Informazioni su come formattare i dati di risposta nell'API Web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: e503df3d81efbb2800503c0cb4ff5ae093b6e1ac
ms.sourcegitcommit: 023495344053dc59115c80538f0ece935e7490a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592356"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="0f7aa-103">Formattare i dati di risposta nell'API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f7aa-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="0f7aa-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0f7aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0f7aa-105">ASP.NET Core MVC supporta la formattazione dei dati di risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="0f7aa-106">I dati di risposta possono essere formattati usando formati specifici o in risposta al formato richiesto dal client.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="0f7aa-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0f7aa-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="0f7aa-108">Risultati azione specifici del formato</span><span class="sxs-lookup"><span data-stu-id="0f7aa-108">Format-specific Action Results</span></span>

<span data-ttu-id="0f7aa-109">Alcuni tipi di risultati di azioni sono specifici di un particolare formato, ad esempio <xref:Microsoft.AspNetCore.Mvc.JsonResult> e <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="0f7aa-110">Le azioni possono restituire risultati formattati in un formato specifico, indipendentemente dalle preferenze dei client.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="0f7aa-111">Ad esempio, la `JsonResult` restituzione di restituisce dati in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="0f7aa-112">La `ContentResult` restituzione di o di una stringa restituisce dati di stringa in formato testo normale.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="0f7aa-113">Non è necessario eseguire un'azione per restituire un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="0f7aa-114">ASP.NET Core supporta qualsiasi valore restituito da un oggetto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="0f7aa-115">I risultati di azioni che restituiscono oggetti che <xref:Microsoft.AspNetCore.Mvc.IActionResult> non sono tipi vengono serializzati usando <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> l'implementazione appropriata.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="0f7aa-116">Per altre informazioni, vedere <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="0f7aa-117">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> Helper incorporato restituisce dati in formato JSON:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="0f7aa-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="0f7aa-118">Il download di esempio restituisce l'elenco degli autori.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="0f7aa-119">Utilizzando gli strumenti di sviluppo del browser [F12 o l'](https://www.getpostman.com/tools) autore del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="0f7aa-120">Viene visualizzata l'intestazione della risposta contenente **Content-Type:** `application/json; charset=utf-8` .</span><span class="sxs-lookup"><span data-stu-id="0f7aa-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="0f7aa-121">Verranno visualizzate le intestazioni della richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-121">The request headers are displayed.</span></span> <span data-ttu-id="0f7aa-122">Ad esempio, l' `Accept` intestazione.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-122">For example, the `Accept` header.</span></span> <span data-ttu-id="0f7aa-123">L' `Accept` intestazione viene ignorata dal codice precedente.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="0f7aa-124">Per restituire dati in formato testo normale, usare <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> e l'helper <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content>:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="0f7aa-125">Nel codice precedente, l'oggetto `Content-Type` restituito è `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="0f7aa-126">La restituzione di `Content-Type` `text/plain`una stringa fornisce:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="0f7aa-127">Per le azioni con più tipi restituiti, `IActionResult`restituire.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="0f7aa-128">Ad esempio, la restituzione di codici di stato HTTP diversi in base al risultato delle operazioni eseguite.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="0f7aa-129">Negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="0f7aa-129">Content negotiation</span></span>

<span data-ttu-id="0f7aa-130">La negoziazione del contenuto si verifica quando il client specifica un' [intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="0f7aa-131">Il formato predefinito usato da ASP.NET Core è [JSON](https://json.org/).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="0f7aa-132">Negoziazione del contenuto:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-132">Content negotiation is:</span></span>

* <span data-ttu-id="0f7aa-133">Implementato da <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="0f7aa-134">Incorporata nei risultati dell'azione specifici del codice di stato restituiti dai metodi helper.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="0f7aa-135">I metodi helper dei risultati delle azioni sono basati `ObjectResult`su.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="0f7aa-136">Quando viene restituito un tipo di modello, il tipo restituito `ObjectResult`è.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="0f7aa-137">Il metodo di azione seguente usa i metodi helper `Ok` e `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="0f7aa-138">Viene restituita una risposta in formato JSON a meno che non sia stato richiesto un altro formato e il server possa restituire il formato richiesto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-138">A JSON-formatted response is returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="0f7aa-139">Gli strumenti, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [post](https://www.getpostman.com/tools) , possono `Accept` impostare l'intestazione per specificare il formato restituito.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` header to specify the return format.</span></span> <span data-ttu-id="0f7aa-140">`Accept` Quando contiene un tipo supportato dal server, tale tipo viene restituito.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-140">When the `Accept` contains a type the server supports, that type is returned.</span></span>

<span data-ttu-id="0f7aa-141">Per impostazione predefinita, ASP.NET Core supporta solo JSON.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-141">By default, ASP.NET Core only supports JSON.</span></span> <span data-ttu-id="0f7aa-142">Per le app che non modificano l'impostazione predefinita, le risposte in formato JSON vengono sempre restituite indipendentemente dalla richiesta del client.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-142">For apps not changing the default, JSON-formatted responses are alway returned regardless of the client request.</span></span> <span data-ttu-id="0f7aa-143">Nella sezione successiva viene illustrato come aggiungere formattatori aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-143">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="0f7aa-144">Le azioni del controller possono restituire POCO (Plain Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-144">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="0f7aa-145">Quando viene restituito un oggetto poco, il runtime crea automaticamente `ObjectResult` un oggetto che esegue il wrapping dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-145">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="0f7aa-146">Il client ottiene l'oggetto serializzato formattato.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-146">The client gets the formatted serialized object.</span></span> <span data-ttu-id="0f7aa-147">Se l'oggetto restituito è `null`, viene restituita una `204 No Content` risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-147">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="0f7aa-148">Restituzione di un tipo di oggetto:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-148">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="0f7aa-149">Nel codice precedente, una richiesta di un alias autore valido restituisce una `200 OK` risposta con i dati dell'autore.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-149">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="0f7aa-150">Una richiesta di un alias non valido restituisce `204 No Content` una risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-150">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="0f7aa-151">Intestazione Accept</span><span class="sxs-lookup"><span data-stu-id="0f7aa-151">The Accept header</span></span>

<span data-ttu-id="0f7aa-152">La *negoziazione* del contenuto si verifica `Accept` quando viene visualizzata un'intestazione nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-152">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="0f7aa-153">Quando una richiesta contiene un'intestazione Accept, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-153">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="0f7aa-154">Enumera i tipi di supporto nell'intestazione Accept in ordine di preferenza.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-154">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="0f7aa-155">Tenta di trovare un formattatore in grado di generare una risposta in uno dei formati specificati.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-155">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="0f7aa-156">Se non viene trovato alcun formattatore in grado di soddisfare la richiesta del client, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-156">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="0f7aa-157">Restituisce `406 Not Acceptable` se<xref:Microsoft.AspNetCore.Mvc.MvcOptions> è stato impostato oppure-</span><span class="sxs-lookup"><span data-stu-id="0f7aa-157">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="0f7aa-158">Tenta di trovare il primo formattatore in grado di generare una risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-158">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="0f7aa-159">Se non è configurato alcun formattatore per il formato richiesto, viene utilizzato il primo formattatore in grado di formattare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-159">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="0f7aa-160">Se nella `Accept` richiesta non è presente alcuna intestazione:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-160">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="0f7aa-161">Il primo formattatore in grado di gestire l'oggetto viene utilizzato per serializzare la risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-161">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="0f7aa-162">Non si verifica alcuna negoziazione.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-162">There isn't any negotiation taking place.</span></span> <span data-ttu-id="0f7aa-163">Il server sta determinando il formato da restituire.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-163">The server is determining what format to return.</span></span>

<span data-ttu-id="0f7aa-164">Se l'intestazione Accept contiene `*/*`, l'intestazione viene ignorata, `RespectBrowserAcceptHeader` a meno che non sia <xref:Microsoft.AspNetCore.Mvc.MvcOptions>impostato su true in.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-164">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="0f7aa-165">Browser e negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="0f7aa-165">Browsers and content negotiation</span></span>

<span data-ttu-id="0f7aa-166">A differenza dei client API tipici, i Web `Accept` browser forniscono le intestazioni.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-166">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="0f7aa-167">Web browser specificare molti formati, inclusi i caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-167">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="0f7aa-168">Per impostazione predefinita, quando il Framework rileva che la richiesta viene ricevuta da un browser:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-168">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="0f7aa-169">L' `Accept` intestazione viene ignorata.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-169">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="0f7aa-170">Il contenuto viene restituito in JSON, a meno che non sia configurato diversamente.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-170">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="0f7aa-171">Ciò offre un'esperienza più coerente nei browser quando si utilizzano le API.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-171">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="0f7aa-172">Per configurare un'app in modo da rispettare le intestazioni Accept <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> del `true`browser, impostare su:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-172">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="0f7aa-173">Configura formattatori</span><span class="sxs-lookup"><span data-stu-id="0f7aa-173">Configure formatters</span></span>

<span data-ttu-id="0f7aa-174">Le app che devono supportare formati aggiuntivi possono aggiungere i pacchetti NuGet appropriati e configurare il supporto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-174">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="0f7aa-175">Esistono formattatori separati per input e output.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-175">There are separate formatters for input and output.</span></span> <span data-ttu-id="0f7aa-176">I formattatori di input vengono utilizzati dall' [associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-176">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="0f7aa-177">I formattatori di output vengono utilizzati per formattare le risposte.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-177">Output formatters are used to format responses.</span></span> <span data-ttu-id="0f7aa-178">Per informazioni sulla creazione di un formattatore personalizzato, vedere [formattatori personalizzati](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-178">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="0f7aa-179">Aggiungere il supporto per il formato XML</span><span class="sxs-lookup"><span data-stu-id="0f7aa-179">Add XML format support</span></span>

<span data-ttu-id="0f7aa-180">I formattatori XML implementati usando <xref:System.Xml.Serialization.XmlSerializer> vengono configurati <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>chiamando:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-180">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="0f7aa-181">Il codice precedente serializza i risultati usando `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-181">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="0f7aa-182">Quando si usa il codice precedente, i metodi del controller devono restituire il formato appropriato in base `Accept` all'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-182">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="0f7aa-183">Configurare i formattatori basati su System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="0f7aa-183">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="0f7aa-184">Le funzionalità per i formattatori basati su `System.Text.Json` possono essere configurate tramite `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-184">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="0f7aa-185">È possibile configurare le opzioni di serializzazione dell'output, in base alle singole `JsonResult`azioni, usando.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-185">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="0f7aa-186">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-186">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="0f7aa-187">Aggiungere il supporto del formato JSON basato su Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="0f7aa-187">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="0f7aa-188">Prima di ASP.NET Core 3,0, i formattatori JSON usati per impostazione predefinita venivano implementati utilizzando il `Newtonsoft.Json` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-188">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="0f7aa-189">In ASP.NET Core 3.0 o versione successiva, i formattatori JSON predefiniti sono basati su `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-189">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="0f7aa-190">Il supporto `Newtonsoft.Json` per formattatori e funzionalità basati è disponibile installando il pacchetto NuGet [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) e configurarlo in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-190">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="0f7aa-191">Alcune funzionalità potrebbero non funzionare correttamente con `System.Text.Json`formattatori basati su e richiedono un riferimento `Newtonsoft.Json`ai formattatori basati su.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-191">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="0f7aa-192">Continuare a usare `Newtonsoft.Json`i formattatori basati su se l'app:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-192">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="0f7aa-193">Utilizza `Newtonsoft.Json` gli attributi.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-193">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="0f7aa-194">Ad esempio, `[JsonProperty]` o `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-194">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="0f7aa-195">Personalizza le impostazioni di serializzazione.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-195">Customizes the serialization settings.</span></span>
* <span data-ttu-id="0f7aa-196">Si basa sulle funzionalità fornite `Newtonsoft.Json` da.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-196">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="0f7aa-197">Configura `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-197">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="0f7aa-198">Prima di ASP.NET Core 3.0 `JsonResult.SerializerSettings` accetta un'istanza di `JsonSerializerSettings` specifica di `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-198">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="0f7aa-199">Genera documentazione [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-199">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="0f7aa-200">Le funzionalità per `Newtonsoft.Json`i formattatori basati su possono essere configurate utilizzando: `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`</span><span class="sxs-lookup"><span data-stu-id="0f7aa-200">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="0f7aa-201">È possibile configurare le opzioni di serializzazione dell'output, in base alle singole `JsonResult`azioni, usando.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-201">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="0f7aa-202">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-202">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="0f7aa-203">Aggiungere il supporto per il formato XML</span><span class="sxs-lookup"><span data-stu-id="0f7aa-203">Add XML format support</span></span>

<span data-ttu-id="0f7aa-204">Per la formattazione XML è necessario il pacchetto NuGet [Microsoft. AspNetCore. Mvc. Formatters. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .</span><span class="sxs-lookup"><span data-stu-id="0f7aa-204">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="0f7aa-205">I formattatori XML implementati usando <xref:System.Xml.Serialization.XmlSerializer> vengono configurati <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>chiamando:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-205">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="0f7aa-206">Il codice precedente serializza i risultati usando `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-206">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="0f7aa-207">Quando si usa il codice precedente, i metodi del controller devono restituire il formato appropriato in base `Accept` all'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-207">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="0f7aa-208">Specificare un formato</span><span class="sxs-lookup"><span data-stu-id="0f7aa-208">Specify a format</span></span>

<span data-ttu-id="0f7aa-209">Per limitare i formati di risposta, applicare [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) il filtro.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-209">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="0f7aa-210">Come la maggior parte `[Produces]` dei [filtri](xref:mvc/controllers/filters), può essere applicato all'azione, al controller o all'ambito globale:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-210">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="0f7aa-211">Il filtro [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) precedente:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-211">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="0f7aa-212">Impone a tutte le azioni all'interno del controller di restituire risposte in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-212">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="0f7aa-213">Se sono configurati altri formattatori e il client specifica un formato diverso, viene restituito JSON.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-213">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="0f7aa-214">Per ulteriori informazioni, vedere [filtri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-214">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="0f7aa-215">Formattatori case speciali</span><span class="sxs-lookup"><span data-stu-id="0f7aa-215">Special case formatters</span></span>

<span data-ttu-id="0f7aa-216">Alcuni casi speciali vengono implementati tramite formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-216">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="0f7aa-217">Per impostazione predefinita `string` , i tipi restituiti vengono formattati come *testo/normale* (*testo/HTML* se `Accept` richiesto tramite l'intestazione).</span><span class="sxs-lookup"><span data-stu-id="0f7aa-217">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="0f7aa-218">Questo comportamento può essere eliminato rimuovendo <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-218">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span></span> <span data-ttu-id="0f7aa-219">I `Configure` formattatori vengono rimossi nel metodo.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-219">Formatters are removed in the `Configure` method.</span></span> <span data-ttu-id="0f7aa-220">Le azioni con un tipo restituito dall'oggetto modello `204 No Content` restituiscono `null`quando viene restituito.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-220">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="0f7aa-221">Questo comportamento può essere eliminato rimuovendo <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-221">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="0f7aa-222">Il codice seguente rimuove `TextOutputFormatter` e `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-222">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="0f7aa-223">Senza, `TextOutputFormatter` `string` i tipi restituiti restituiscono `406 Not Acceptable`.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-223">Without the `TextOutputFormatter`, `string` return types return `406 Not Acceptable`.</span></span> <span data-ttu-id="0f7aa-224">Se esiste un formattatore XML, formatta `string` i tipi restituiti se `TextOutputFormatter` l'oggetto viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-224">If an XML formatter exists, it formats `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="0f7aa-225">Senza `HttpNoContentOutputFormatter`, gli oggetti Null vengono formattati tramite il formattatore configurato.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-225">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="0f7aa-226">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-226">For example:</span></span>

* <span data-ttu-id="0f7aa-227">Il formattatore JSON restituisce una risposta con un corpo `null`di.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-227">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="0f7aa-228">Il formattatore XML restituisce un elemento XML vuoto con l' `xsi:nil="true"` attributo impostato.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-228">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="0f7aa-229">Mapping URL formato risposta</span><span class="sxs-lookup"><span data-stu-id="0f7aa-229">Response format URL mappings</span></span>

<span data-ttu-id="0f7aa-230">I client possono richiedere un particolare formato come parte dell'URL, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-230">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="0f7aa-231">Nella stringa di query o in una parte del percorso.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-231">In the query string or part of the path.</span></span>
* <span data-ttu-id="0f7aa-232">Utilizzando un'estensione di file specifica del formato, ad esempio XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-232">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="0f7aa-233">Il mapping dal percorso della richiesta deve essere specificato nella route usata dall'API.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-233">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="0f7aa-234">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0f7aa-234">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="0f7aa-235">La route precedente consente di specificare il formato richiesto come estensione di file facoltativa.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-235">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="0f7aa-236">L' [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attributo controlla l'esistenza del valore di formato `RouteData` in ed esegue il mapping del formato della risposta al formattatore appropriato al momento della creazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="0f7aa-236">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="0f7aa-237">Route</span><span class="sxs-lookup"><span data-stu-id="0f7aa-237">Route</span></span>        |             <span data-ttu-id="0f7aa-238">Formattatore</span><span class="sxs-lookup"><span data-stu-id="0f7aa-238">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="0f7aa-239">Formattatore di output predefinito</span><span class="sxs-lookup"><span data-stu-id="0f7aa-239">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="0f7aa-240">Formattatore JSON (se configurato)</span><span class="sxs-lookup"><span data-stu-id="0f7aa-240">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="0f7aa-241">Formattatore XML (se configurato)</span><span class="sxs-lookup"><span data-stu-id="0f7aa-241">The XML formatter (if configured)</span></span>  |
