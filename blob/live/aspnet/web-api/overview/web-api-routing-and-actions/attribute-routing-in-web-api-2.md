---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attributo di Routing in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7c563f566b8456b63ffe0a3c4876432c60a19e89
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="f6643-102">Attributo di Routing in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f6643-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f6643-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f6643-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f6643-104">*Routing* è come API Web corrisponde a un URI a un'azione.</span><span class="sxs-lookup"><span data-stu-id="f6643-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="f6643-105">Web API 2 supporta un nuovo tipo di routing, denominata *attributo routing*.</span><span class="sxs-lookup"><span data-stu-id="f6643-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="f6643-106">Come suggerisce il nome, l'attributo routing utilizza gli attributi per definire le route.</span><span class="sxs-lookup"><span data-stu-id="f6643-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="f6643-107">Attributo routing offre maggiore controllo sull'URI dell'API web.</span><span class="sxs-lookup"><span data-stu-id="f6643-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="f6643-108">Ad esempio, è possibile creare facilmente gli URI che descrivono le gerarchie di risorse.</span><span class="sxs-lookup"><span data-stu-id="f6643-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="f6643-109">Lo stile di versioni precedenti di routing, denominato basato sulle convenzioni di routing, è ancora completamente supportato.</span><span class="sxs-lookup"><span data-stu-id="f6643-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="f6643-110">In effetti, è possibile combinare entrambe le tecniche nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="f6643-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="f6643-111">In questo argomento viene illustrato come abilitare il routing di attributo e descrive le varie opzioni per il routing di attributo.</span><span class="sxs-lookup"><span data-stu-id="f6643-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="f6643-112">Per un'esercitazione end-to-end che utilizza il routing di attributo, vedere [creare un'API REST con il Routing degli attributi in Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="f6643-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f6643-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f6643-113">Prerequisites</span></span>

<span data-ttu-id="f6643-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="f6643-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="f6643-115">In alternativa, è possibile utilizzare Gestione pacchetti NuGet per installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="f6643-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="f6643-116">Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f6643-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f6643-117">Immettere il comando seguente nella finestra della Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="f6643-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="f6643-118">Attributo perché Routing?</span><span class="sxs-lookup"><span data-stu-id="f6643-118">Why Attribute Routing?</span></span>

<span data-ttu-id="f6643-119">La prima versione dell'API Web utilizzato *basato sulle convenzioni* routing.</span><span class="sxs-lookup"><span data-stu-id="f6643-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="f6643-120">In tale tipo di routing, si definisce uno o più modelli di route, che sono fondamentalmente stringhe con parametri.</span><span class="sxs-lookup"><span data-stu-id="f6643-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="f6643-121">Quando il framework riceve una richiesta, corrisponde a URI di base del modello di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="f6643-122">(Per ulteriori informazioni sul routing basato sulle convenzioni, vedere [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6643-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="f6643-123">Un vantaggio offerto da basato sulle convenzioni di routing è che i modelli sono definiti in un'unica posizione e le regole di routing vengono applicate in modo coerente in tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="f6643-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="f6643-124">Purtroppo, basato sulle convenzioni di routing rende difficile supportare alcuni modelli di URI che sono comuni nelle API REST.</span><span class="sxs-lookup"><span data-stu-id="f6643-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="f6643-125">Ad esempio, le risorse spesso contengono risorse figlio: sono presenti ordini, filmati hanno attori, documentazione avere autori e così via.</span><span class="sxs-lookup"><span data-stu-id="f6643-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="f6643-126">È naturale per creare URI che riflette queste relazioni:</span><span class="sxs-lookup"><span data-stu-id="f6643-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="f6643-127">Questo tipo di URI è difficile creare utilizzando basato sulle convenzioni di routing.</span><span class="sxs-lookup"><span data-stu-id="f6643-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="f6643-128">Anche se può essere eseguito, i risultati non scalabilità anche se si dispone di numerosi controller o tipi di risorse.</span><span class="sxs-lookup"><span data-stu-id="f6643-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="f6643-129">Con il routing di attributo, risulta semplice per definire una route per questo URI.</span><span class="sxs-lookup"><span data-stu-id="f6643-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="f6643-130">È sufficiente aggiungere un attributo azione del controller:</span><span class="sxs-lookup"><span data-stu-id="f6643-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="f6643-131">Ecco alcuni altri modelli di tale attributo routing rende facile.</span><span class="sxs-lookup"><span data-stu-id="f6643-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="f6643-132">**Controllo delle versioni API**</span><span class="sxs-lookup"><span data-stu-id="f6643-132">**API versioning**</span></span>

<span data-ttu-id="f6643-133">In questo esempio, "/ v1/api/prodotti" sarà indirizzato a un controller diverso rispetto a "/ v2/api/prodotti".</span><span class="sxs-lookup"><span data-stu-id="f6643-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="f6643-134">**Segmenti URI overload**</span><span class="sxs-lookup"><span data-stu-id="f6643-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="f6643-135">In questo esempio, "1" è un numero di ordine, ma "pending" esegue il mapping a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="f6643-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="f6643-136">**Più tipi di parametro**</span><span class="sxs-lookup"><span data-stu-id="f6643-136">**Mulitple parameter types**</span></span>

<span data-ttu-id="f6643-137">In questo esempio, "1" è un numero di ordine, ma "2013/06/16" specifica una data.</span><span class="sxs-lookup"><span data-stu-id="f6643-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="f6643-138">Abilitazione del Routing di attributo</span><span class="sxs-lookup"><span data-stu-id="f6643-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="f6643-139">Per abilitare il routing di attributo, chiamare **MapHttpAttributeRoutes** durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="f6643-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="f6643-140">Questo metodo di estensione è definito nel **System.Web.Http.HttpConfigurationExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="f6643-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="f6643-141">Routing degli attributi possono essere combinati con [basato sulle convenzioni](routing-in-aspnet-web-api.md) routing.</span><span class="sxs-lookup"><span data-stu-id="f6643-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="f6643-142">Per definire le route basato sulle convenzioni, chiamare il **MapHttpRoute** metodo.</span><span class="sxs-lookup"><span data-stu-id="f6643-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="f6643-143">Per ulteriori informazioni sulla configurazione Web API, vedere [la configurazione di ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6643-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="f6643-144">Nota: La migrazione da API Web 1</span><span class="sxs-lookup"><span data-stu-id="f6643-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="f6643-145">Prima di API Web 2, i modelli di progetto API Web generato codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f6643-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="f6643-146">Se è attivato il routing di attributo, il codice genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="f6643-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="f6643-147">Se si aggiorna un progetto di API Web esistente per l'utilizzo di routing degli attributi, assicurarsi di aggiornare il codice di configurazione per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f6643-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="f6643-148">Per ulteriori informazioni, vedere [la configurazione di API Web con Hosting ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="f6643-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="f6643-149">Aggiunta di attributi delle Route</span><span class="sxs-lookup"><span data-stu-id="f6643-149">Adding Route Attributes</span></span>

<span data-ttu-id="f6643-150">Di seguito è riportato un esempio di una route definita utilizzando un attributo:</span><span class="sxs-lookup"><span data-stu-id="f6643-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="f6643-151">La stringa &quot;clienti / {customerId} / ordini&quot; è il modello URI per la route.</span><span class="sxs-lookup"><span data-stu-id="f6643-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="f6643-152">API Web tenta di far corrispondere l'URI della richiesta per il modello.</span><span class="sxs-lookup"><span data-stu-id="f6643-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="f6643-153">In questo esempio, "customers" e "orders" sono valori letterali segmenti e "{customerId}" è un parametro della variabile.</span><span class="sxs-lookup"><span data-stu-id="f6643-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="f6643-154">Gli URI seguenti corrisponderebbe a questo modello:</span><span class="sxs-lookup"><span data-stu-id="f6643-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="f6643-155">È possibile limitare l'individuazione delle corrispondenze utilizzando [vincoli](#constraints), descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="f6643-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="f6643-156">Si noti che il &quot;{customerId}&quot; parametro nel modello di route corrisponde al nome del *customerId* parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="f6643-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="f6643-157">Quando l'API Web richiama l'azione del controller, tenta di associare i parametri di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="f6643-158">Ad esempio, se l'URI è `http://example.com/customers/1/orders`, API Web tenta di associare il valore "1" per il *customerId* parametro dell'azione.</span><span class="sxs-lookup"><span data-stu-id="f6643-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="f6643-159">Un modello URI può includere diversi parametri:</span><span class="sxs-lookup"><span data-stu-id="f6643-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="f6643-160">Metodi controller che non dispongono di un attributo route utilizzano basato sulle convenzioni di routing.</span><span class="sxs-lookup"><span data-stu-id="f6643-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="f6643-161">In questo modo, è possibile combinare entrambi i tipi di routing nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="f6643-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="f6643-162">Metodi HTTP</span><span class="sxs-lookup"><span data-stu-id="f6643-162">HTTP Methods</span></span>

<span data-ttu-id="f6643-163">API Web seleziona anche azioni in base al metodo HTTP della richiesta (GET, POST e così via).</span><span class="sxs-lookup"><span data-stu-id="f6643-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="f6643-164">Per impostazione predefinita, Web API Cerca una corrispondenza tra maiuscole e minuscole con l'inizio del nome di metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="f6643-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="f6643-165">Ad esempio, un metodo del controller denominato `PutCustomers` corrisponde a una richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f6643-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="f6643-166">È possibile ignorare questa convenzione dichiarando il metodo con i seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="f6643-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="f6643-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="f6643-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="f6643-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="f6643-168">**[HttpGet]**</span></span>
- <span data-ttu-id="f6643-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="f6643-169">**[HttpHead]**</span></span>
- <span data-ttu-id="f6643-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="f6643-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="f6643-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="f6643-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="f6643-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="f6643-172">**[HttpPost]**</span></span>
- <span data-ttu-id="f6643-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="f6643-173">**[HttpPut]**</span></span>

<span data-ttu-id="f6643-174">Nell'esempio seguente viene eseguito il mapping del metodo CreateBook alle richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f6643-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="f6643-175">Per tutti gli altri metodi HTTP, compresi i metodi non standard, utilizzano il **AcceptVerbs** attributo, che accetta un elenco di metodi HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6643-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="f6643-176">Prefissi di route</span><span class="sxs-lookup"><span data-stu-id="f6643-176">Route Prefixes</span></span>

<span data-ttu-id="f6643-177">Spesso, le route presenti in un controller avviino tutti con lo stesso prefisso.</span><span class="sxs-lookup"><span data-stu-id="f6643-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="f6643-178">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f6643-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="f6643-179">È possibile impostare un prefisso comune per un intero controller utilizzando il **[RoutePrefix]** attributo:</span><span class="sxs-lookup"><span data-stu-id="f6643-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="f6643-180">Utilizzare una tilde (~) dell'attributo del metodo per sostituire il prefisso della route:</span><span class="sxs-lookup"><span data-stu-id="f6643-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="f6643-181">Il prefisso della route può includere parametri:</span><span class="sxs-lookup"><span data-stu-id="f6643-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="f6643-182">Vincoli della route</span><span class="sxs-lookup"><span data-stu-id="f6643-182">Route Constraints</span></span>

<span data-ttu-id="f6643-183">Vincoli della route consentono di limitare la modalità in cui vengono confrontati i parametri nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="f6643-184">La sintassi generale è &quot;{vincolo: parametro}&quot;.</span><span class="sxs-lookup"><span data-stu-id="f6643-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="f6643-185">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f6643-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="f6643-186">In questo caso, la prima route verrà solo selezionata se il &quot;id&quot; segmento dell'URI è un numero intero.</span><span class="sxs-lookup"><span data-stu-id="f6643-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="f6643-187">In caso contrario, verrà scelta la seconda route.</span><span class="sxs-lookup"><span data-stu-id="f6643-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="f6643-188">Nella tabella seguente vengono elencati i vincoli che sono supportati.</span><span class="sxs-lookup"><span data-stu-id="f6643-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="f6643-189">Vincolo</span><span class="sxs-lookup"><span data-stu-id="f6643-189">Constraint</span></span> | <span data-ttu-id="f6643-190">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f6643-190">Description</span></span> | <span data-ttu-id="f6643-191">Esempio</span><span class="sxs-lookup"><span data-stu-id="f6643-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f6643-192">alpha</span><span class="sxs-lookup"><span data-stu-id="f6643-192">alpha</span></span> | <span data-ttu-id="f6643-193">Corrispondenze lettere maiuscole o minuscole dell'alfabeto latino caratteri (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="f6643-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="f6643-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="f6643-194">{x:alpha}</span></span> |
| <span data-ttu-id="f6643-195">bool</span><span class="sxs-lookup"><span data-stu-id="f6643-195">bool</span></span> | <span data-ttu-id="f6643-196">Corrisponde a un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="f6643-196">Matches a Boolean value.</span></span> | <span data-ttu-id="f6643-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="f6643-197">{x:bool}</span></span> |
| <span data-ttu-id="f6643-198">datetime</span><span class="sxs-lookup"><span data-stu-id="f6643-198">datetime</span></span> | <span data-ttu-id="f6643-199">Corrispondenze un **DateTime** valore.</span><span class="sxs-lookup"><span data-stu-id="f6643-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="f6643-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="f6643-200">{x:datetime}</span></span> |
| <span data-ttu-id="f6643-201">decimal</span><span class="sxs-lookup"><span data-stu-id="f6643-201">decimal</span></span> | <span data-ttu-id="f6643-202">Corrisponde a un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="f6643-202">Matches a decimal value.</span></span> | <span data-ttu-id="f6643-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="f6643-203">{x:decimal}</span></span> |
| <span data-ttu-id="f6643-204">double</span><span class="sxs-lookup"><span data-stu-id="f6643-204">double</span></span> | <span data-ttu-id="f6643-205">Corrisponde a un valore a virgola mobile a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="f6643-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="f6643-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="f6643-206">{x:double}</span></span> |
| <span data-ttu-id="f6643-207">float</span><span class="sxs-lookup"><span data-stu-id="f6643-207">float</span></span> | <span data-ttu-id="f6643-208">Corrisponde a un valore a virgola mobile a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="f6643-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="f6643-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="f6643-209">{x:float}</span></span> |
| <span data-ttu-id="f6643-210">guid</span><span class="sxs-lookup"><span data-stu-id="f6643-210">guid</span></span> | <span data-ttu-id="f6643-211">Corrisponde a un valore GUID.</span><span class="sxs-lookup"><span data-stu-id="f6643-211">Matches a GUID value.</span></span> | <span data-ttu-id="f6643-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="f6643-212">{x:guid}</span></span> |
| <span data-ttu-id="f6643-213">int</span><span class="sxs-lookup"><span data-stu-id="f6643-213">int</span></span> | <span data-ttu-id="f6643-214">Corrisponde a un valore integer a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="f6643-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="f6643-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="f6643-215">{x:int}</span></span> |
| <span data-ttu-id="f6643-216">length</span><span class="sxs-lookup"><span data-stu-id="f6643-216">length</span></span> | <span data-ttu-id="f6643-217">Corrisponde a una stringa con la lunghezza specificata o in un determinato intervallo di lunghezza.</span><span class="sxs-lookup"><span data-stu-id="f6643-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="f6643-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="f6643-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="f6643-219">long</span><span class="sxs-lookup"><span data-stu-id="f6643-219">long</span></span> | <span data-ttu-id="f6643-220">Corrisponde a un valore integer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="f6643-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="f6643-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="f6643-221">{x:long}</span></span> |
| <span data-ttu-id="f6643-222">max</span><span class="sxs-lookup"><span data-stu-id="f6643-222">max</span></span> | <span data-ttu-id="f6643-223">Consente di ricercare un numero intero con un valore massimo.</span><span class="sxs-lookup"><span data-stu-id="f6643-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="f6643-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="f6643-224">{x:max(10)}</span></span> |
| <span data-ttu-id="f6643-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="f6643-225">maxlength</span></span> | <span data-ttu-id="f6643-226">Corrisponde a una stringa con una lunghezza massima.</span><span class="sxs-lookup"><span data-stu-id="f6643-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="f6643-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="f6643-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="f6643-228">min</span><span class="sxs-lookup"><span data-stu-id="f6643-228">min</span></span> | <span data-ttu-id="f6643-229">Consente di ricercare un numero intero con un valore minimo.</span><span class="sxs-lookup"><span data-stu-id="f6643-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="f6643-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="f6643-230">{x:min(10)}</span></span> |
| <span data-ttu-id="f6643-231">minLength</span><span class="sxs-lookup"><span data-stu-id="f6643-231">minlength</span></span> | <span data-ttu-id="f6643-232">Corrisponde a una stringa con una lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="f6643-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="f6643-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="f6643-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="f6643-234">range</span><span class="sxs-lookup"><span data-stu-id="f6643-234">range</span></span> | <span data-ttu-id="f6643-235">Consente di ricercare un numero intero in un intervallo di valori.</span><span class="sxs-lookup"><span data-stu-id="f6643-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="f6643-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="f6643-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="f6643-237">regex</span><span class="sxs-lookup"><span data-stu-id="f6643-237">regex</span></span> | <span data-ttu-id="f6643-238">Corrisponde a un'espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="f6643-238">Matches a regular expression.</span></span> | <span data-ttu-id="f6643-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="f6643-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="f6643-240">Si noti che alcuni dei vincoli, ad esempio &quot;min&quot;, accettano argomenti tra parentesi.</span><span class="sxs-lookup"><span data-stu-id="f6643-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="f6643-241">È possibile applicare più vincoli a un parametro, separato da due punti.</span><span class="sxs-lookup"><span data-stu-id="f6643-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="f6643-242">Vincoli della Route personalizzato</span><span class="sxs-lookup"><span data-stu-id="f6643-242">Custom Route Constraints</span></span>

<span data-ttu-id="f6643-243">È possibile creare vincoli della route personalizzate implementando il **IHttpRouteConstraint** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="f6643-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="f6643-244">Ad esempio, il vincolo seguente limita un parametro a un valore intero diverso da zero.</span><span class="sxs-lookup"><span data-stu-id="f6643-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="f6643-245">Il codice seguente viene illustrato come registrare il vincolo:</span><span class="sxs-lookup"><span data-stu-id="f6643-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="f6643-246">Ora è possibile applicare il vincolo nei percorsi di:</span><span class="sxs-lookup"><span data-stu-id="f6643-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="f6643-247">È inoltre possibile sostituire l'intero **DefaultInlineConstraintResolver** classe mediante l'implementazione di **IInlineConstraintResolver** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="f6643-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="f6643-248">In questo modo sostituirà tutti i vincoli predefiniti, a meno che l'implementazione di **IInlineConstraintResolver** li aggiunge in modo specifico.</span><span class="sxs-lookup"><span data-stu-id="f6643-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="f6643-249">I parametri URI facoltativo e i valori predefiniti</span><span class="sxs-lookup"><span data-stu-id="f6643-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="f6643-250">È possibile apportare un parametro URI facoltativo mediante l'aggiunta di un punto interrogativo al parametro di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="f6643-251">Se un parametro di route è facoltativo, è necessario definire un valore predefinito per il parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="f6643-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="f6643-252">In questo esempio, `/api/books/locale/1033` e `/api/books/locale` restituiscono la stessa risorsa.</span><span class="sxs-lookup"><span data-stu-id="f6643-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="f6643-253">In alternativa, è possibile specificare un valore predefinito all'interno del modello di route, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f6643-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="f6643-254">Questo è pressoché lo stesso dell'esempio precedente, ma è presente una leggera differenza di comportamento quando viene applicato il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f6643-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="f6643-255">Nel primo esempio ("{lcid?}"), il valore predefinito di 1033 viene assegnato direttamente al parametro del metodo, pertanto il parametro avrà il valore esatto.</span><span class="sxs-lookup"><span data-stu-id="f6643-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="f6643-256">Nel secondo esempio ("{lcid = 1033}"), il valore predefinito di "1033" passa attraverso il processo di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="f6643-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="f6643-257">Raccoglitore di modelli predefinito convertirà "1033" al valore numerico 1033.</span><span class="sxs-lookup"><span data-stu-id="f6643-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="f6643-258">Tuttavia, è possibile collegare un raccoglitore di modelli personalizzati, che potrebbe eseguire un'operazione diversa.</span><span class="sxs-lookup"><span data-stu-id="f6643-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="f6643-259">(Nella maggior parte dei casi, a meno che non si dispone di raccoglitori di modelli personalizzati nella pipeline, le due forme sarà equivalente.)</span><span class="sxs-lookup"><span data-stu-id="f6643-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="f6643-260">Nomi di route</span><span class="sxs-lookup"><span data-stu-id="f6643-260">Route Names</span></span>

<span data-ttu-id="f6643-261">Nell'API Web, ogni route ha un nome.</span><span class="sxs-lookup"><span data-stu-id="f6643-261">In Web API, every route has a name.</span></span> <span data-ttu-id="f6643-262">I nomi di route sono utili per la generazione di collegamenti, in modo che è possibile includere un collegamento in una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6643-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="f6643-263">Per specificare il nome della route, impostare il **nome** proprietà dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="f6643-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="f6643-264">Nell'esempio seguente viene illustrato come impostare il nome della route, nonché come utilizzare il nome di route per la generazione di un collegamento.</span><span class="sxs-lookup"><span data-stu-id="f6643-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="f6643-265">Ordine della route</span><span class="sxs-lookup"><span data-stu-id="f6643-265">Route Order</span></span>

<span data-ttu-id="f6643-266">Quando il framework tenta di abbinare un URI con una route, valuta le route presenti in un ordine particolare.</span><span class="sxs-lookup"><span data-stu-id="f6643-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="f6643-267">Per specificare l'ordine, impostare il **RouteOrder** proprietà dell'attributo della route.</span><span class="sxs-lookup"><span data-stu-id="f6643-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="f6643-268">I valori più bassi vengono valutati per primi.</span><span class="sxs-lookup"><span data-stu-id="f6643-268">Lower values are evaluated first.</span></span> <span data-ttu-id="f6643-269">Il valore dell'ordine predefinito è zero.</span><span class="sxs-lookup"><span data-stu-id="f6643-269">The default order value is zero.</span></span>

<span data-ttu-id="f6643-270">Ecco come viene determinata l'ordinamento totale:</span><span class="sxs-lookup"><span data-stu-id="f6643-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="f6643-271">Confrontare il **RouteOrder** proprietà dell'attributo di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="f6643-272">Esaminare ogni segmento URI nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="f6643-273">Per ogni segmento, ordine come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f6643-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="f6643-274">Valore letterale segmenti.</span><span class="sxs-lookup"><span data-stu-id="f6643-274">Literal segments.</span></span>
    2. <span data-ttu-id="f6643-275">Parametri di route con vincoli.</span><span class="sxs-lookup"><span data-stu-id="f6643-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="f6643-276">Parametri di route senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="f6643-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="f6643-277">Segmenti di parametro con caratteri jolly con vincoli.</span><span class="sxs-lookup"><span data-stu-id="f6643-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="f6643-278">Segmenti di parametro con caratteri jolly senza vincoli.</span><span class="sxs-lookup"><span data-stu-id="f6643-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="f6643-279">Nel caso di pareggio, le route vengono ordinate in un confronto ordinale tra stringhe tra maiuscole e minuscole ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) del modello di route.</span><span class="sxs-lookup"><span data-stu-id="f6643-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="f6643-280">Di seguito è riportato un esempio.</span><span class="sxs-lookup"><span data-stu-id="f6643-280">Here is an example.</span></span> <span data-ttu-id="f6643-281">Si supponga di che definisce il seguente controller:</span><span class="sxs-lookup"><span data-stu-id="f6643-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="f6643-282">Queste route vengono ordinate nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="f6643-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="f6643-283">orders/details</span><span class="sxs-lookup"><span data-stu-id="f6643-283">orders/details</span></span>
2. <span data-ttu-id="f6643-284">orders/{id}</span><span class="sxs-lookup"><span data-stu-id="f6643-284">orders/{id}</span></span>
3. <span data-ttu-id="f6643-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="f6643-285">orders/{customerName}</span></span>
4. <span data-ttu-id="f6643-286">orders/{\\*date}</span><span class="sxs-lookup"><span data-stu-id="f6643-286">orders/{\\*date}</span></span>
5. <span data-ttu-id="f6643-287">orders/pending</span><span class="sxs-lookup"><span data-stu-id="f6643-287">orders/pending</span></span>

<span data-ttu-id="f6643-288">Si noti che "Dettagli" sono un valore letterale segmento e viene visualizzato prima di "{id}", ma "in sospeso" viene visualizzato ultima perché il **RouteOrder** proprietà è 1.</span><span class="sxs-lookup"><span data-stu-id="f6643-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="f6643-289">(In questo esempio presuppone che vi sono Nessun cliente denominato "Dettagli" o "in sospeso".</span><span class="sxs-lookup"><span data-stu-id="f6643-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="f6643-290">In generale, tentare di evitare cicli ambigui.</span><span class="sxs-lookup"><span data-stu-id="f6643-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="f6643-291">In questo esempio, un modello di route migliore per `GetByCustomer` è "clienti / {NomeCliente}")</span><span class="sxs-lookup"><span data-stu-id="f6643-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
