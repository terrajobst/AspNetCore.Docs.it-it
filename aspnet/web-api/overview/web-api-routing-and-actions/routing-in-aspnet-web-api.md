---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207303"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="e2c57-102">Routing in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e2c57-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e2c57-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e2c57-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e2c57-104">Questo articolo descrive come API Web ASP.NET instrada le richieste HTTP al controller.</span><span class="sxs-lookup"><span data-stu-id="e2c57-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c57-105">Se si ha familiarità con ASP.NET MVC, API Web di routing è molto simile a il routing MVC.</span><span class="sxs-lookup"><span data-stu-id="e2c57-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="e2c57-106">La differenza principale è che l'API Web Usa il verbo HTTP, non il percorso dell'URI, selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="e2c57-107">È inoltre possibile utilizzare routing in stile MVC nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="e2c57-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="e2c57-108">Questo articolo non presuppone alcuna conoscenza di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e2c57-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="e2c57-109">Tabelle di routing</span><span class="sxs-lookup"><span data-stu-id="e2c57-109">Routing Tables</span></span>

<span data-ttu-id="e2c57-110">Nell'API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c57-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="e2c57-111">I metodi del controller pubblici vengono chiamati *metodi di azione* o semplicemente *azioni*.</span><span class="sxs-lookup"><span data-stu-id="e2c57-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="e2c57-112">Quando il framework API Web riceve una richiesta, indirizza la richiesta a un'azione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="e2c57-113">Per determinare l'azione da richiamare, il framework Usa un *tabella di routing*.</span><span class="sxs-lookup"><span data-stu-id="e2c57-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="e2c57-114">Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:</span><span class="sxs-lookup"><span data-stu-id="e2c57-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e2c57-115">Questa route è definita nel *WebApiConfig.cs* file, che viene inserito nella *App\_avviare* directory:</span><span class="sxs-lookup"><span data-stu-id="e2c57-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e2c57-116">Per altre informazioni sul `WebApiConfig` classe, vedere [configurazione di ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2c57-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="e2c57-117">Se self-hosting di API Web, è necessario impostare la tabella di routing direttamente sul `HttpSelfHostConfiguration` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e2c57-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="e2c57-118">Per altre informazioni, vedere [self-hosting un'API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2c57-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="e2c57-119">Ogni voce nella tabella di routing contiene un *modello di route*.</span><span class="sxs-lookup"><span data-stu-id="e2c57-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="e2c57-120">Il modello di route predefinito per l'API Web &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2c57-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="e2c57-121">In questo modello &quot;api&quot; è un segmento di percorso letterale e {controller} e {id} sono variabili di segnaposto.</span><span class="sxs-lookup"><span data-stu-id="e2c57-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="e2c57-122">Quando il framework API Web riceve una richiesta HTTP, Cerca la corrispondenza con l'URI in base a uno dei modelli di route nella tabella di routing.</span><span class="sxs-lookup"><span data-stu-id="e2c57-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="e2c57-123">Se nessuna route corrisponde, il client riceve un errore 404.</span><span class="sxs-lookup"><span data-stu-id="e2c57-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="e2c57-124">Ad esempio, gli URI seguenti corrispondono alla route predefinita:</span><span class="sxs-lookup"><span data-stu-id="e2c57-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="e2c57-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="e2c57-125">/api/contacts</span></span>
- <span data-ttu-id="e2c57-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="e2c57-126">/api/contacts/1</span></span>
- <span data-ttu-id="e2c57-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="e2c57-127">/api/products/gizmo1</span></span>

<span data-ttu-id="e2c57-128">Tuttavia, l'URI seguente non corrisponde, perché manca il &quot;api&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="e2c57-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="e2c57-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="e2c57-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="e2c57-130">Il motivo per l'uso di "api" nella route è per evitare conflitti con il routing di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e2c57-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="e2c57-131">In questo modo, è possibile avere &quot;/contatta&quot; passare a un controller MVC, e &quot;/api/contacts&quot; passare a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="e2c57-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="e2c57-132">Naturalmente, se non si desidera che questa convenzione, è possibile modificare la tabella di route predefiniti.</span><span class="sxs-lookup"><span data-stu-id="e2c57-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="e2c57-133">Una volta trovata una route corrispondente, API Web consente di selezionare il controller e l'azione:</span><span class="sxs-lookup"><span data-stu-id="e2c57-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="e2c57-134">Per trovare il controller, API Web aggiunge &quot;Controller&quot; al valore della *{controller}* variabile.</span><span class="sxs-lookup"><span data-stu-id="e2c57-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="e2c57-135">Per trovare l'azione, API Web esamina il verbo HTTP e quindi cerca un'azione il cui nome inizia con lo stesso nome del verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c57-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="e2c57-136">Ad esempio, con una richiesta GET, API Web di Azure cerca un'azione con prefisso &quot;ottenere&quot;, ad esempio &quot;GetContact&quot; oppure &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2c57-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="e2c57-137">Questa convenzione viene applicata solo per GET, POST, PUT, DELETE, HEAD, opzioni e applicare PATCH di verbi.</span><span class="sxs-lookup"><span data-stu-id="e2c57-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="e2c57-138">È possibile abilitare altri verbi HTTP utilizzando attributi nel controller.</span><span class="sxs-lookup"><span data-stu-id="e2c57-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="e2c57-139">Vedremo un esempio in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="e2c57-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="e2c57-140">Altre variabili di segnaposto nel modello di route, ad esempio *{id},* vengono mappati ai parametri di azione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="e2c57-141">Esaminiamo un esempio.</span><span class="sxs-lookup"><span data-stu-id="e2c57-141">Let's look at an example.</span></span> <span data-ttu-id="e2c57-142">Si supponga di definisce il controller seguente:</span><span class="sxs-lookup"><span data-stu-id="e2c57-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e2c57-143">Ecco alcune possibili richieste HTTP, insieme all'azione che viene richiamato per ogni:</span><span class="sxs-lookup"><span data-stu-id="e2c57-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="e2c57-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="e2c57-144">HTTP Verb</span></span> | <span data-ttu-id="e2c57-145">Percorso dell'URI</span><span class="sxs-lookup"><span data-stu-id="e2c57-145">URI Path</span></span> | <span data-ttu-id="e2c57-146">Operazione</span><span class="sxs-lookup"><span data-stu-id="e2c57-146">Action</span></span> | <span data-ttu-id="e2c57-147">Parametro</span><span class="sxs-lookup"><span data-stu-id="e2c57-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e2c57-148">GET</span><span class="sxs-lookup"><span data-stu-id="e2c57-148">GET</span></span> | <span data-ttu-id="e2c57-149">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="e2c57-149">api/products</span></span> | <span data-ttu-id="e2c57-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="e2c57-150">GetAllProducts</span></span> | <span data-ttu-id="e2c57-151">*(nessuno)*</span><span class="sxs-lookup"><span data-stu-id="e2c57-151">*(none)*</span></span> |
| <span data-ttu-id="e2c57-152">GET</span><span class="sxs-lookup"><span data-stu-id="e2c57-152">GET</span></span> | <span data-ttu-id="e2c57-153">API/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="e2c57-153">api/products/4</span></span> | <span data-ttu-id="e2c57-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="e2c57-154">GetProductById</span></span> | <span data-ttu-id="e2c57-155">4</span><span class="sxs-lookup"><span data-stu-id="e2c57-155">4</span></span> |
| <span data-ttu-id="e2c57-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="e2c57-156">DELETE</span></span> | <span data-ttu-id="e2c57-157">API/prodotti/4</span><span class="sxs-lookup"><span data-stu-id="e2c57-157">api/products/4</span></span> | <span data-ttu-id="e2c57-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="e2c57-158">DeleteProduct</span></span> | <span data-ttu-id="e2c57-159">4</span><span class="sxs-lookup"><span data-stu-id="e2c57-159">4</span></span> |
| <span data-ttu-id="e2c57-160">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="e2c57-160">POST</span></span> | <span data-ttu-id="e2c57-161">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="e2c57-161">api/products</span></span> | <span data-ttu-id="e2c57-162">*(nessuna corrispondenza trovata)*</span><span class="sxs-lookup"><span data-stu-id="e2c57-162">*(no match)*</span></span> |  |

<span data-ttu-id="e2c57-163">Si noti che il *{id}* segmento dell'URI, se presente, viene eseguito il mapping per il *id* parametro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="e2c57-164">In questo esempio, il controller definisce due metodi GET, uno con un *id* parametro e uno senza parametri.</span><span class="sxs-lookup"><span data-stu-id="e2c57-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="e2c57-165">Inoltre, tenere presente che la richiesta POST non riuscirà, perché il controller non definisce un &quot;Post... &quot; (metodo).</span><span class="sxs-lookup"><span data-stu-id="e2c57-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="e2c57-166">Variazioni di routine</span><span class="sxs-lookup"><span data-stu-id="e2c57-166">Routing Variations</span></span>

<span data-ttu-id="e2c57-167">La sezione precedente descrive il meccanismo di routing di base per ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e2c57-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="e2c57-168">Questa sezione descrive alcune variazioni.</span><span class="sxs-lookup"><span data-stu-id="e2c57-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="e2c57-169">Verbi HTTP</span><span class="sxs-lookup"><span data-stu-id="e2c57-169">HTTP verbs</span></span>

<span data-ttu-id="e2c57-170">Invece di usare la convenzione di denominazione per i verbi HTTP, è possibile specificare il verbo HTTP per un'azione in modo esplicito tramite la decorazione del metodo di azione con uno dei seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="e2c57-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="e2c57-171">Nell'esempio seguente, il `FindProduct` metodo viene eseguito il mapping alle richieste GET:</span><span class="sxs-lookup"><span data-stu-id="e2c57-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e2c57-172">Per consentire più verbi HTTP per un'azione o per consentire i verbi HTTP diversi da GET, PUT, POST, DELETE, HEAD, opzioni e delle PATCH, usare il `[AcceptVerbs]` attributo, che accetta un elenco di verbi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2c57-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="e2c57-173">Routing in base al nome di azione</span><span class="sxs-lookup"><span data-stu-id="e2c57-173">Routing by Action Name</span></span>

<span data-ttu-id="e2c57-174">Con il modello di routing predefinito, API Web Usa il verbo HTTP per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="e2c57-175">Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:</span><span class="sxs-lookup"><span data-stu-id="e2c57-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e2c57-176">In questo modello di route, il *{action}* il metodo di azione nel controller di nomi dei parametri.</span><span class="sxs-lookup"><span data-stu-id="e2c57-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="e2c57-177">Con questo stile di routing, usare gli attributi per specificare i verbi HTTP consentiti.</span><span class="sxs-lookup"><span data-stu-id="e2c57-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="e2c57-178">Si supponga, ad esempio, che il controller è il metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="e2c57-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e2c57-179">In questo caso, una richiesta GET per "api/prodotti/dettagli/1" esegue il mapping al `Details` (metodo).</span><span class="sxs-lookup"><span data-stu-id="e2c57-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="e2c57-180">Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di tipo RPC.</span><span class="sxs-lookup"><span data-stu-id="e2c57-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="e2c57-181">È possibile sostituire il nome dell'azione utilizzando il `[ActionName]` attributo.</span><span class="sxs-lookup"><span data-stu-id="e2c57-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="e2c57-182">Nell'esempio seguente, esistono due azioni che eseguono il mapping a &quot;api/prodotti/miniature/*id*. Uno supporta GET e l'altro supporta POST:</span><span class="sxs-lookup"><span data-stu-id="e2c57-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="e2c57-183">Non-azioni</span><span class="sxs-lookup"><span data-stu-id="e2c57-183">Non-Actions</span></span>

<span data-ttu-id="e2c57-184">Per evitare che un metodo richiamato come un'azione, usare il `[NonAction]` attributo.</span><span class="sxs-lookup"><span data-stu-id="e2c57-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="e2c57-185">Segnala al framework che il metodo non è un'azione, anche se le regole di routing in caso contrario, in base alla distinzione.</span><span class="sxs-lookup"><span data-stu-id="e2c57-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="e2c57-186">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="e2c57-186">Further Reading</span></span>

<span data-ttu-id="e2c57-187">In questo argomento è fornita una panoramica del routing.</span><span class="sxs-lookup"><span data-stu-id="e2c57-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="e2c57-188">Per altre informazioni, vedere [Routing e selezione dell'azione](routing-and-action-selection.md), che descrive esattamente come il framework corrisponde a un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.</span><span class="sxs-lookup"><span data-stu-id="e2c57-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
