---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing di ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26509260"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="69838-102">Routing di ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="69838-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="69838-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="69838-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="69838-104">In questo articolo viene descritto come ASP.NET Web API instrada le richieste HTTP per i controller.</span><span class="sxs-lookup"><span data-stu-id="69838-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="69838-105">Se si ha familiarità con ASP.NET MVC, API Web routing è molto simile per il routing MVC.</span><span class="sxs-lookup"><span data-stu-id="69838-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="69838-106">La differenza principale è che l'API Web utilizza il metodo HTTP, non il percorso dell'URI, per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="69838-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="69838-107">È anche possibile utilizzare il routing MVC stile nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="69838-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="69838-108">In questo articolo non si presuppone la conoscenza di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="69838-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="69838-109">Tabelle di routing</span><span class="sxs-lookup"><span data-stu-id="69838-109">Routing Tables</span></span>

<span data-ttu-id="69838-110">In ASP.NET Web API, un *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="69838-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="69838-111">Vengono chiamati i metodi pubblici del controller *metodi di azione* o semplicemente *azioni*.</span><span class="sxs-lookup"><span data-stu-id="69838-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="69838-112">Quando il framework di API Web riceve una richiesta, la richiesta viene indirizzata a un'azione.</span><span class="sxs-lookup"><span data-stu-id="69838-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="69838-113">Per determinare l'azione da richiamare, il framework utilizza un *tabella di routing*.</span><span class="sxs-lookup"><span data-stu-id="69838-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="69838-114">Il modello di progetto di Visual Studio per l'API Web crea una route predefinita:</span><span class="sxs-lookup"><span data-stu-id="69838-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="69838-115">La route è definita nel file WebApiConfig.cs, che viene inserito nell'App\_directory di avvio:</span><span class="sxs-lookup"><span data-stu-id="69838-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="69838-116">Per ulteriori informazioni sul **WebApiConfig** classe, vedere [la configurazione di ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="69838-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="69838-117">Se l'API Web indipendente, è necessario impostare la tabella di routing direttamente sul **HttpSelfHostConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="69838-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="69838-118">Per ulteriori informazioni, vedere [indipendente un'API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="69838-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="69838-119">Ogni voce nella tabella di routing contiene un *modello di route*.</span><span class="sxs-lookup"><span data-stu-id="69838-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="69838-120">Il modello di route predefinita per l'API Web è &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="69838-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="69838-121">In questo modello, &quot;api&quot; è un segmento di percorso letterale e {controller} e {id} sono variabili di segnaposto.</span><span class="sxs-lookup"><span data-stu-id="69838-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="69838-122">Quando il framework di API Web riceve una richiesta HTTP, tenta di mettere in corrispondenza l'URI su uno dei modelli della route nella tabella di routing.</span><span class="sxs-lookup"><span data-stu-id="69838-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="69838-123">Se nessuna route corrispondente, il client riceve un errore 404.</span><span class="sxs-lookup"><span data-stu-id="69838-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="69838-124">Ad esempio, il seguente URI corrispondenza la route predefinita:</span><span class="sxs-lookup"><span data-stu-id="69838-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="69838-125">/ api/contatti</span><span class="sxs-lookup"><span data-stu-id="69838-125">/api/contacts</span></span>
- <span data-ttu-id="69838-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="69838-126">/api/contacts/1</span></span>
- <span data-ttu-id="69838-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="69838-127">/api/products/gizmo1</span></span>

<span data-ttu-id="69838-128">Tuttavia, l'URI seguente non corrisponde, perché manca il &quot;api&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="69838-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="69838-129">contatti/1</span><span class="sxs-lookup"><span data-stu-id="69838-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="69838-130">Il motivo per l'uso di "api" nella route è per evitare conflitti con il routing di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="69838-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="69838-131">In questo modo, è possibile avere &quot;/contatta&quot; passare a un controller MVC, e &quot;/api/contatti&quot; passare a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="69838-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="69838-132">Naturalmente, se non si desidera che questa convenzione, è possibile modificare la tabella di route predefinita.</span><span class="sxs-lookup"><span data-stu-id="69838-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="69838-133">Una volta trovata una route corrispondente, API Web consente di selezionare il controller e l'azione:</span><span class="sxs-lookup"><span data-stu-id="69838-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="69838-134">Per trovare il controller, API Web aggiunge &quot;Controller&quot; il valore di *{controller}* variabile.</span><span class="sxs-lookup"><span data-stu-id="69838-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="69838-135">Per trovare l'azione, API Web analizza il metodo HTTP e cercherà un'azione il cui nome inizia con lo stesso nome di metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="69838-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="69838-136">Ad esempio, con una richiesta GET, API Web cerca un'azione che inizia con &quot;Get... &quot;, ad esempio &quot;GetContact&quot; o &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="69838-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="69838-137">Questa convenzione si applica solo a GET, POST, PUT e DELETE di metodi.</span><span class="sxs-lookup"><span data-stu-id="69838-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="69838-138">È possibile abilitare altri metodi HTTP tramite attributi nel controller.</span><span class="sxs-lookup"><span data-stu-id="69838-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="69838-139">Vedremo un esempio di che in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="69838-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="69838-140">Altre variabili di segnaposto nel modello di route, ad esempio *{id},* mappati ai parametri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="69838-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="69838-141">Esaminiamo un esempio.</span><span class="sxs-lookup"><span data-stu-id="69838-141">Let's look at an example.</span></span> <span data-ttu-id="69838-142">Si supponga di definisce il seguente controller:</span><span class="sxs-lookup"><span data-stu-id="69838-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="69838-143">Ecco alcune possibili richieste HTTP, insieme all'azione che viene richiamato per ogni:</span><span class="sxs-lookup"><span data-stu-id="69838-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="69838-144">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="69838-144">HTTP Method</span></span> | <span data-ttu-id="69838-145">Percorso URI</span><span class="sxs-lookup"><span data-stu-id="69838-145">URI Path</span></span> | <span data-ttu-id="69838-146">Azione</span><span class="sxs-lookup"><span data-stu-id="69838-146">Action</span></span> | <span data-ttu-id="69838-147">Parametro</span><span class="sxs-lookup"><span data-stu-id="69838-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="69838-148">GET</span><span class="sxs-lookup"><span data-stu-id="69838-148">GET</span></span> | <span data-ttu-id="69838-149">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="69838-149">api/products</span></span> | <span data-ttu-id="69838-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="69838-150">GetAllProducts</span></span> | <span data-ttu-id="69838-151">*(none)*</span><span class="sxs-lookup"><span data-stu-id="69838-151">*(none)*</span></span> |
| <span data-ttu-id="69838-152">GET</span><span class="sxs-lookup"><span data-stu-id="69838-152">GET</span></span> | <span data-ttu-id="69838-153">prodotti/API/4</span><span class="sxs-lookup"><span data-stu-id="69838-153">api/products/4</span></span> | <span data-ttu-id="69838-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="69838-154">GetProductById</span></span> | <span data-ttu-id="69838-155">4</span><span class="sxs-lookup"><span data-stu-id="69838-155">4</span></span> |
| <span data-ttu-id="69838-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="69838-156">DELETE</span></span> | <span data-ttu-id="69838-157">prodotti/API/4</span><span class="sxs-lookup"><span data-stu-id="69838-157">api/products/4</span></span> | <span data-ttu-id="69838-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="69838-158">DeleteProduct</span></span> | <span data-ttu-id="69838-159">4</span><span class="sxs-lookup"><span data-stu-id="69838-159">4</span></span> |
| <span data-ttu-id="69838-160">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="69838-160">POST</span></span> | <span data-ttu-id="69838-161">API/prodotti</span><span class="sxs-lookup"><span data-stu-id="69838-161">api/products</span></span> | <span data-ttu-id="69838-162">*(nessuna corrispondenza)*</span><span class="sxs-lookup"><span data-stu-id="69838-162">*(no match)*</span></span> |  |

<span data-ttu-id="69838-163">Si noti che il *{id}* segmento dell'URI, se presente, viene eseguito il mapping per il *id* parametro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="69838-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="69838-164">In questo esempio, il controller definisce due metodi GET, uno con un *id* parametro e uno senza parametri.</span><span class="sxs-lookup"><span data-stu-id="69838-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="69838-165">Inoltre, si noti che la richiesta POST avrà esito negativo, perché il controller non definisce un &quot;Post... &quot; metodo.</span><span class="sxs-lookup"><span data-stu-id="69838-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="69838-166">Variazioni di routine</span><span class="sxs-lookup"><span data-stu-id="69838-166">Routing Variations</span></span>

<span data-ttu-id="69838-167">La sezione precedente è descritto il meccanismo di routing di base per l'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="69838-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="69838-168">Questa sezione vengono descritte alcune varianti.</span><span class="sxs-lookup"><span data-stu-id="69838-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="69838-169">Metodi HTTP</span><span class="sxs-lookup"><span data-stu-id="69838-169">HTTP Methods</span></span>

<span data-ttu-id="69838-170">Anziché utilizzare la convenzione di denominazione per i metodi HTTP, è possibile specificare in modo esplicito il metodo HTTP per un'azione tramite il metodo di azione con la **HttpGet**, **HttpPut**, **HttpPost** , o **HttpDelete** attributo.</span><span class="sxs-lookup"><span data-stu-id="69838-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="69838-171">Nell'esempio seguente, il metodo FindProduct viene eseguito il mapping alle richieste GET:</span><span class="sxs-lookup"><span data-stu-id="69838-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="69838-172">Per consentire più metodi HTTP per un'azione, o per consentire a metodi HTTP diversi da GET, PUT, POST e DELETE, utilizzare il **AcceptVerbs** attributo, che accetta un elenco di metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="69838-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="69838-173">Routing in base al nome di azione</span><span class="sxs-lookup"><span data-stu-id="69838-173">Routing by Action Name</span></span>

<span data-ttu-id="69838-174">Con il modello di routing predefinito, API Web Usa il metodo HTTP per selezionare l'azione.</span><span class="sxs-lookup"><span data-stu-id="69838-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="69838-175">Tuttavia, è anche possibile creare una route in cui il nome dell'azione è incluso nell'URI:</span><span class="sxs-lookup"><span data-stu-id="69838-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="69838-176">In questo modello di route, il *{action}* nomi di parametro del metodo di azione nel controller.</span><span class="sxs-lookup"><span data-stu-id="69838-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="69838-177">Con questo stile di routing, utilizzare gli attributi per specificare i metodi HTTP consentiti.</span><span class="sxs-lookup"><span data-stu-id="69838-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="69838-178">Ad esempio, si supponga che il controller sia il metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="69838-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="69838-179">In questo caso, sarebbe eseguire il mapping di una richiesta GET per "api/prodotti/dettagli/1" al metodo di dettagli.</span><span class="sxs-lookup"><span data-stu-id="69838-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="69838-180">Questo stile di routing è simile a ASP.NET MVC e potrebbe essere appropriato per un'API di stile RPC.</span><span class="sxs-lookup"><span data-stu-id="69838-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="69838-181">È possibile ignorare il nome dell'azione utilizzando il **ActionName** attributo.</span><span class="sxs-lookup"><span data-stu-id="69838-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="69838-182">Nell'esempio seguente, sono disponibili due azioni che eseguono il mapping a &quot;prodotti/api/anteprima/*id*. Uno supporta GET e l'altro supporta POST:</span><span class="sxs-lookup"><span data-stu-id="69838-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="69838-183">Non azioni</span><span class="sxs-lookup"><span data-stu-id="69838-183">Non-Actions</span></span>

<span data-ttu-id="69838-184">Per impedire che un metodo richiamato come un'azione, utilizzare il **NonAction** attributo.</span><span class="sxs-lookup"><span data-stu-id="69838-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="69838-185">Segnala al framework che il metodo non è un'azione, anche se le regole di routing in caso contrario sarebbe corrisponde.</span><span class="sxs-lookup"><span data-stu-id="69838-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="69838-186">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="69838-186">Further Reading</span></span>

<span data-ttu-id="69838-187">In questo argomento viene fornita una panoramica generale del routing.</span><span class="sxs-lookup"><span data-stu-id="69838-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="69838-188">Per ulteriori dettagli, vedere [Routing e la selezione di azione](routing-and-action-selection.md), che descrive esattamente come il framework corrisponde a un URI a una route, seleziona un controller e quindi seleziona l'azione da richiamare.</span><span class="sxs-lookup"><span data-stu-id="69838-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
