---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing e selezione dell'azione nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: b4912d3ee1e13651f2a63d54d7dbfd92e00f85f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837444"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="9a636-102">Routing e selezione dell'azione nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9a636-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9a636-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9a636-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9a636-104">Questo articolo descrive come API Web ASP.NET consente di indirizzare una richiesta HTTP per una determinata azione su un controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="9a636-105">Per una panoramica generale del routing, vedere [Routing in API Web ASP.NET](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="9a636-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="9a636-106">Questo articolo esamina i dettagli del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="9a636-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="9a636-107">Se si crea un progetto API Web e si scopre che alcune richieste Don ' t get indirizzato nel modo che previsto, si spera in questo articolo spiega come.</span><span class="sxs-lookup"><span data-stu-id="9a636-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="9a636-108">Routing prevede tre fasi principali:</span><span class="sxs-lookup"><span data-stu-id="9a636-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="9a636-109">Corrispondenza dell'URI per un modello di route.</span><span class="sxs-lookup"><span data-stu-id="9a636-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="9a636-110">Quando si seleziona un controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-110">Selecting a controller.</span></span>
3. <span data-ttu-id="9a636-111">Selezionare un'azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-111">Selecting an action.</span></span>

<span data-ttu-id="9a636-112">È possibile sostituire alcune parti del processo con i propri comportamenti personalizzati.</span><span class="sxs-lookup"><span data-stu-id="9a636-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="9a636-113">In questo articolo, descritto il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="9a636-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="9a636-114">Al termine, nota le posizioni in cui è possibile personalizzare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="9a636-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="9a636-115">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="9a636-115">Route Templates</span></span>

<span data-ttu-id="9a636-116">Un modello di route è simile a un percorso URI, ma può avere i valori segnaposto, indicati tra parentesi graffe:</span><span class="sxs-lookup"><span data-stu-id="9a636-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="9a636-117">Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:</span><span class="sxs-lookup"><span data-stu-id="9a636-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="9a636-118">È anche possibile fornire i vincoli, che limitano come un segmento dell'URI può corrispondere a un segnaposto:</span><span class="sxs-lookup"><span data-stu-id="9a636-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="9a636-119">Il framework tenta di far corrispondere i segmenti nel percorso dell'URI per il modello.</span><span class="sxs-lookup"><span data-stu-id="9a636-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="9a636-120">Valori letterali nel modello devono corrispondere esattamente.</span><span class="sxs-lookup"><span data-stu-id="9a636-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="9a636-121">Un segnaposto corrisponde a qualsiasi valore, a meno che non si specificano vincoli.</span><span class="sxs-lookup"><span data-stu-id="9a636-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="9a636-122">Il framework non corrisponde a altre parti dell'URI, ad esempio il nome host o i parametri di query.</span><span class="sxs-lookup"><span data-stu-id="9a636-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="9a636-123">Il framework sceglie la prima route nella tabella di route che corrisponde all'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="9a636-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="9a636-124">Esistono due segnaposto speciali: "{controller}" e "{action}".</span><span class="sxs-lookup"><span data-stu-id="9a636-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="9a636-125">"{controller}" fornisce il nome del controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="9a636-126">"{action}" fornisce il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="9a636-127">Nell'API Web, la convenzione consueto consiste nell'omettere "{action}".</span><span class="sxs-lookup"><span data-stu-id="9a636-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="9a636-128">attività</span><span class="sxs-lookup"><span data-stu-id="9a636-128">Defaults</span></span>

<span data-ttu-id="9a636-129">Se si specificano le impostazioni predefinite, route corrisponderà a un URI privo di tali segmenti.</span><span class="sxs-lookup"><span data-stu-id="9a636-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="9a636-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9a636-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="9a636-131">L'URI "`http://localhost/api/products`" corrisponde a questa route.</span><span class="sxs-lookup"><span data-stu-id="9a636-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="9a636-132">Il segmento "{category}" è assegnato il valore predefinito "all".</span><span class="sxs-lookup"><span data-stu-id="9a636-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="9a636-133">Dizionario della route</span><span class="sxs-lookup"><span data-stu-id="9a636-133">Route Dictionary</span></span>

<span data-ttu-id="9a636-134">Se il framework rileva una corrispondenza per un URI, crea un dizionario che contiene il valore di ogni segnaposto.</span><span class="sxs-lookup"><span data-stu-id="9a636-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="9a636-135">Le chiavi sono i nomi dei segnaposto, senza includere le parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="9a636-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="9a636-136">I valori vengono ricavati dal percorso URI o i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="9a636-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="9a636-137">Il dizionario viene archiviato nel **IHttpRouteData** oggetto.</span><span class="sxs-lookup"><span data-stu-id="9a636-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="9a636-138">Durante questa fase di corrispondenza dei route, la speciale "{controller}" e il segnaposto "{action}" vengono gestiti come i segnaposto di altri.</span><span class="sxs-lookup"><span data-stu-id="9a636-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="9a636-139">Sono semplicemente archiviati nel dizionario gli altri valori.</span><span class="sxs-lookup"><span data-stu-id="9a636-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="9a636-140">Un valore predefinito può avere il valore speciale **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="9a636-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="9a636-141">Se questo valore assegnato un segnaposto, il valore non viene aggiunto al dizionario della route.</span><span class="sxs-lookup"><span data-stu-id="9a636-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="9a636-142">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9a636-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="9a636-143">Per il percorso dell'URI "api/prodotti", conterrà il dizionario della route:</span><span class="sxs-lookup"><span data-stu-id="9a636-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="9a636-144">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="9a636-144">controller: "products"</span></span>
- <span data-ttu-id="9a636-145">category: "all"</span><span class="sxs-lookup"><span data-stu-id="9a636-145">category: "all"</span></span>

<span data-ttu-id="9a636-146">Per "api/prodotti/toys/123", tuttavia, il dizionario della route conterrà:</span><span class="sxs-lookup"><span data-stu-id="9a636-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="9a636-147">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="9a636-147">controller: "products"</span></span>
- <span data-ttu-id="9a636-148">category: "toys"</span><span class="sxs-lookup"><span data-stu-id="9a636-148">category: "toys"</span></span>
- <span data-ttu-id="9a636-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="9a636-149">id: "123"</span></span>

<span data-ttu-id="9a636-150">I valori predefiniti possono anche includere un valore che non viene visualizzato in un punto qualsiasi nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="9a636-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="9a636-151">Se la route corrisponde, tale valore viene archiviato nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="9a636-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="9a636-152">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9a636-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="9a636-153">Se il percorso dell'URI è "root/api/8", il dizionario contiene due valori:</span><span class="sxs-lookup"><span data-stu-id="9a636-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="9a636-154">controller: "customers"</span><span class="sxs-lookup"><span data-stu-id="9a636-154">controller: "customers"</span></span>
- <span data-ttu-id="9a636-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="9a636-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="9a636-156">Quando si seleziona un Controller</span><span class="sxs-lookup"><span data-stu-id="9a636-156">Selecting a Controller</span></span>

<span data-ttu-id="9a636-157">La selezione del controller è gestita dal **IHttpControllerSelector.SelectController** (metodo).</span><span class="sxs-lookup"><span data-stu-id="9a636-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="9a636-158">Questo metodo accetta un **HttpRequestMessage** istanza e restituisce un' **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="9a636-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="9a636-159">L'implementazione predefinita avviene tramite il **DefaultHttpControllerSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="9a636-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="9a636-160">Questa classe Usa un algoritmo molto semplice:</span><span class="sxs-lookup"><span data-stu-id="9a636-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="9a636-161">Cercare nel dizionario della route per la chiave "controller".</span><span class="sxs-lookup"><span data-stu-id="9a636-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="9a636-162">Il valore per questa chiave e aggiungere la stringa "Controller" per ottenere il nome del tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="9a636-163">Cercare un controller API Web con questo nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="9a636-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="9a636-164">Ad esempio, se il dizionario della route contiene il coppia chiave-valore "controller" = "prodotti", il tipo di controller è "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="9a636-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="9a636-165">Se è presente alcun tipo di corrispondenza o più corrispondenze, il framework restituisce un errore al client.</span><span class="sxs-lookup"><span data-stu-id="9a636-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="9a636-166">Passaggio 3, **DefaultHttpControllerSelector** Usa le **IHttpControllerTypeResolver** interfaccia da ottenere l'elenco dei tipi di controller API Web.</span><span class="sxs-lookup"><span data-stu-id="9a636-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="9a636-167">L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che implementano (a) **IHttpController**, (b) sono non astratta e (c) dispone di un nome che termina con "Controller".</span><span class="sxs-lookup"><span data-stu-id="9a636-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="9a636-168">Selezione azione</span><span class="sxs-lookup"><span data-stu-id="9a636-168">Action Selection</span></span>

<span data-ttu-id="9a636-169">Dopo aver selezionato il controller, il framework consente di selezionare l'azione chiamando il **IHttpActionSelector.SelectAction** (metodo).</span><span class="sxs-lookup"><span data-stu-id="9a636-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="9a636-170">Questo metodo accetta un **HttpControllerContext** e restituisce un' **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="9a636-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="9a636-171">L'implementazione predefinita avviene tramite il **ApiControllerActionSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="9a636-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="9a636-172">Per selezionare un'azione, analizza i seguenti:</span><span class="sxs-lookup"><span data-stu-id="9a636-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="9a636-173">Il metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9a636-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="9a636-174">Il segnaposto "{action}" nel modello di route, se presente.</span><span class="sxs-lookup"><span data-stu-id="9a636-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="9a636-175">I parametri delle operazioni nel controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="9a636-176">Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti di azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="9a636-177">**I metodi nel controller che sono considerati "azioni"?**</span><span class="sxs-lookup"><span data-stu-id="9a636-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="9a636-178">Quando si seleziona un'azione, il framework esamina solo i metodi di istanza pubblici nel controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="9a636-179">Inoltre, esclude ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metodi (costruttori, eventi, gli overload degli operatori e così via) e metodi ereditati dalle **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="9a636-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="9a636-180">**Metodi HTTP.**</span><span class="sxs-lookup"><span data-stu-id="9a636-180">**HTTP Methods.**</span></span> <span data-ttu-id="9a636-181">Il framework sceglie solo le azioni che corrispondono al metodo HTTP della richiesta, determinata come segue:</span><span class="sxs-lookup"><span data-stu-id="9a636-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="9a636-182">È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, oppure **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="9a636-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="9a636-183">In caso contrario, se il nome del metodo del controller inizia con "Get", "Post", "Put", "Delete", "Head", "Opzioni" o "Patch", quindi per convenzione l'azione supporta tale metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a636-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="9a636-184">Se nessuna delle precedenti, il metodo supporta POST.</span><span class="sxs-lookup"><span data-stu-id="9a636-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="9a636-185">**Associazioni di parametro.**</span><span class="sxs-lookup"><span data-stu-id="9a636-185">**Parameter Bindings.**</span></span> <span data-ttu-id="9a636-186">Un'associazione di parametri è come API Web crea un valore per un parametro.</span><span class="sxs-lookup"><span data-stu-id="9a636-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="9a636-187">Ecco la regola predefinita per l'associazione di parametro:</span><span class="sxs-lookup"><span data-stu-id="9a636-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="9a636-188">Tipi semplici vengono forniti dall'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="9a636-189">I tipi complessi sono tratti dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9a636-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="9a636-190">Tipi semplici includono tutte le [i tipi primitivi di .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **stringa** , e **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="9a636-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="9a636-191">Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9a636-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="9a636-192">È possibile sostituire le regole di associazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9a636-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="9a636-193">Visualizzare [associazione di parametri di API Web dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a636-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="9a636-194">Detto questo, ecco l'algoritmo di selezione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="9a636-195">Creare un elenco di tutte le azioni nel controller che corrisponde al metodo di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a636-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="9a636-196">Se il dizionario della route è una voce "action", rimuovere le azioni il cui nome corrisponde a questo valore.</span><span class="sxs-lookup"><span data-stu-id="9a636-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="9a636-197">Provare a corrispondere ai parametri di azione all'URI, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9a636-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="9a636-198">Per ogni azione, ottenere un elenco dei parametri sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="9a636-199">Escludere i parametri facoltativi.</span><span class="sxs-lookup"><span data-stu-id="9a636-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="9a636-200">In questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, il dizionario della route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="9a636-201">Corrispondenze sono maiuscole e minuscole e non dipendono l'ordine dei parametri.</span><span class="sxs-lookup"><span data-stu-id="9a636-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="9a636-202">Selezionare un'azione in cui ogni parametro nell'elenco ha una corrispondenza nell'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="9a636-203">Se più di una sola azione soddisfa questi criteri, scegliere quella con la maggior parte delle corrispondenze di parametro.</span><span class="sxs-lookup"><span data-stu-id="9a636-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="9a636-204">Ignorare le azioni con il **[NonAction]** attributo.</span><span class="sxs-lookup"><span data-stu-id="9a636-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="9a636-205">Passaggio #3 è probabilmente il più confusione.</span><span class="sxs-lookup"><span data-stu-id="9a636-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="9a636-206">L'idea di base è che un parametro può ottenere il relativo valore dall'URI, dal corpo della richiesta o da un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="9a636-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="9a636-207">Per i parametri forniti dall'URI, è opportuno assicurarsi che l'URI contiene effettivamente un valore per tale parametro, il percorso (tramite il dizionario della route) o nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9a636-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="9a636-208">Ad esempio, si consideri l'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="9a636-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="9a636-209">Il *id* il parametro viene associato all'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="9a636-210">Pertanto, questa azione può corrispondere solo a un URI che contiene un valore per "id", il dizionario della route o nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9a636-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="9a636-211">I parametri facoltativi sono un'eccezione, perché sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="9a636-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="9a636-212">Per un parametro facoltativo, è OK se l'associazione non è possibile ottenere il valore dall'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="9a636-213">I tipi complessi sono un'eccezione per un motivo diverso.</span><span class="sxs-lookup"><span data-stu-id="9a636-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="9a636-214">Un tipo complesso può associarsi solo a URI tramite un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="9a636-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="9a636-215">Ma in tal caso, il framework non è possibile tener presente se il parametro sarebbe associato a un particolare URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="9a636-216">Per individuare, sarebbe necessario richiamare l'associazione.</span><span class="sxs-lookup"><span data-stu-id="9a636-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="9a636-217">L'obiettivo dell'algoritmo consiste nel selezionare un'azione dalla descrizione statica, prima di richiamare tutti i binding.</span><span class="sxs-lookup"><span data-stu-id="9a636-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="9a636-218">Di conseguenza, i tipi complessi vengono esclusi dall'algoritmo di corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="9a636-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="9a636-219">Dopo aver selezionato l'azione, tutte le associazioni di parametro vengono richiamate.</span><span class="sxs-lookup"><span data-stu-id="9a636-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="9a636-220">Riepilogo:</span><span class="sxs-lookup"><span data-stu-id="9a636-220">Summary:</span></span>

- <span data-ttu-id="9a636-221">L'azione deve corrispondere al metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9a636-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="9a636-222">Il nome dell'azione deve corrispondere alla voce "action" nel dizionario della route, se presente.</span><span class="sxs-lookup"><span data-stu-id="9a636-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="9a636-223">Per ogni parametro dell'azione, se il parametro viene estratto dall'URI, quindi il nome del parametro deve essere trovato nel dizionario della route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="9a636-224">(I parametri facoltativi e parametri con tipi complessi vengono esclusi.)</span><span class="sxs-lookup"><span data-stu-id="9a636-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="9a636-225">Tentativo di rendere il maggior numero di parametri.</span><span class="sxs-lookup"><span data-stu-id="9a636-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="9a636-226">La corrispondenza migliore potrebbe essere un metodo senza parametri.</span><span class="sxs-lookup"><span data-stu-id="9a636-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="9a636-227">Esempio dettagliato</span><span class="sxs-lookup"><span data-stu-id="9a636-227">Extended Example</span></span>

<span data-ttu-id="9a636-228">Route:</span><span class="sxs-lookup"><span data-stu-id="9a636-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="9a636-229">Controller:</span><span class="sxs-lookup"><span data-stu-id="9a636-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="9a636-230">Richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="9a636-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="9a636-231">Corrispondenza di route</span><span class="sxs-lookup"><span data-stu-id="9a636-231">Route Matching</span></span>

<span data-ttu-id="9a636-232">L'URI corrisponde alla route denominata "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="9a636-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="9a636-233">Il dizionario della route contiene le voci seguenti:</span><span class="sxs-lookup"><span data-stu-id="9a636-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="9a636-234">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="9a636-234">controller: "products"</span></span>
- <span data-ttu-id="9a636-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="9a636-235">id: "1"</span></span>

<span data-ttu-id="9a636-236">Il dizionario della route non contiene i parametri della stringa di query, "version" e "Dettagli", ma questi continueranno ad essere considerati durante la selezione di azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="9a636-237">Selezione del controller</span><span class="sxs-lookup"><span data-stu-id="9a636-237">Controller Selection</span></span>

<span data-ttu-id="9a636-238">Dalla voce "controller" nel dizionario della route, il tipo di controller è `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="9a636-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="9a636-239">Selezione azione</span><span class="sxs-lookup"><span data-stu-id="9a636-239">Action Selection</span></span>

<span data-ttu-id="9a636-240">La richiesta HTTP è una richiesta GET.</span><span class="sxs-lookup"><span data-stu-id="9a636-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="9a636-241">Le azioni del controller che supportano il recupero vengono `GetAll`, `GetById`, e `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="9a636-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="9a636-242">Il dizionario della route non contiene una voce per "action", pertanto non è necessario affinché corrisponda al nome di azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="9a636-243">Quindi, cerchiamo corrispondano ai nomi di parametro per le azioni, osservando solo le azioni GET.</span><span class="sxs-lookup"><span data-stu-id="9a636-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="9a636-244">Operazione</span><span class="sxs-lookup"><span data-stu-id="9a636-244">Action</span></span> | <span data-ttu-id="9a636-245">Parametri di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="9a636-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="9a636-246">none</span><span class="sxs-lookup"><span data-stu-id="9a636-246">none</span></span> |
| `GetById` | <span data-ttu-id="9a636-247">"id"</span><span class="sxs-lookup"><span data-stu-id="9a636-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="9a636-248">"name"</span><span class="sxs-lookup"><span data-stu-id="9a636-248">"name"</span></span> |

<span data-ttu-id="9a636-249">Si noti che il *versione* parametro di `GetById` non viene considerato, perché è un parametro facoltativo.</span><span class="sxs-lookup"><span data-stu-id="9a636-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="9a636-250">Il `GetAll` metodo corrisponde alla facilmente.</span><span class="sxs-lookup"><span data-stu-id="9a636-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="9a636-251">Il `GetById` metodo anche corrispondenze, perché il dizionario della route contiene "id".</span><span class="sxs-lookup"><span data-stu-id="9a636-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="9a636-252">Il `FindProductsByName` (metodo) non corrisponde.</span><span class="sxs-lookup"><span data-stu-id="9a636-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="9a636-253">Il `GetById` metodo wins, poiché corrisponde a un parametro, rispetto a alcun parametro per `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="9a636-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="9a636-254">Il metodo viene richiamato con i valori dei parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="9a636-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="9a636-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="9a636-255">*id* = 1</span></span>
- <span data-ttu-id="9a636-256">*versione* Version=1.5</span><span class="sxs-lookup"><span data-stu-id="9a636-256">*version* = 1.5</span></span>

<span data-ttu-id="9a636-257">Si noti che sebbene *versione* non è stata usata nell'algoritmo di selezione, il valore del parametro proviene dalla stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="9a636-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="9a636-258">Punti di estensione</span><span class="sxs-lookup"><span data-stu-id="9a636-258">Extension Points</span></span>

<span data-ttu-id="9a636-259">API Web fornisce punti di estensione per alcune parti del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="9a636-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="9a636-260">Interfaccia</span><span class="sxs-lookup"><span data-stu-id="9a636-260">Interface</span></span> | <span data-ttu-id="9a636-261">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a636-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9a636-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="9a636-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="9a636-263">Seleziona il controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-263">Selects the controller.</span></span> |
| <span data-ttu-id="9a636-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="9a636-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="9a636-265">Ottiene l'elenco dei tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-265">Gets the list of controller types.</span></span> <span data-ttu-id="9a636-266">Il **DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco.</span><span class="sxs-lookup"><span data-stu-id="9a636-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="9a636-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="9a636-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="9a636-268">Ottiene l'elenco degli assembly di progetto.</span><span class="sxs-lookup"><span data-stu-id="9a636-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="9a636-269">Il **IHttpControllerTypeResolver** interfaccia Usa questo elenco per trovare i tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="9a636-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="9a636-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="9a636-271">Crea nuove istanze di controller.</span><span class="sxs-lookup"><span data-stu-id="9a636-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="9a636-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="9a636-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="9a636-273">Seleziona l'azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-273">Selects the action.</span></span> |
| <span data-ttu-id="9a636-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="9a636-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="9a636-275">Richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="9a636-275">Invokes the action.</span></span> |

<span data-ttu-id="9a636-276">Per garantire la propria implementazione di una di queste interfacce, usare il **Services** insieme nel **HttpConfiguration** oggetto:</span><span class="sxs-lookup"><span data-stu-id="9a636-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
