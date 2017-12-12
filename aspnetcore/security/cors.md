---
title: Abilitare le richieste tra le origini (CORS)
author: rick-anderson
description: Questo documento introduce CORS come standard per consentire o rifiutare le richieste cross-origin in un'applicazione ASP.NET Core.
keywords: ASP.NET Core, CORS, cross-origin
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 5398b6ad6531710de2b8000cb368e5fa607ae7ff
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="eab4d-104">Abilitare le richieste tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="eab4d-104">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="eab4d-105">Da [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eab4d-105">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="eab4d-106">Protezione del browser impedisce che una pagina web effettua le richieste AJAX a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="eab4d-106">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="eab4d-107">Questa restrizione viene chiamata il *criteri stessa origine*e impedisce a un sito di leggere i dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="eab4d-107">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="eab4d-108">Tuttavia, in alcuni casi è consigliabile consentire ad altri siti richieste cross-origin per l'API web.</span><span class="sxs-lookup"><span data-stu-id="eab4d-108">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="eab4d-109">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server per ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-109">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="eab4d-110">Tramite condivisione CORS, un server può consentire in modo esplicito alcune richieste cross-origin durante il rifiuto di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="eab4d-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="eab4d-111">CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="eab4d-111">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="eab4d-112">In questo argomento viene illustrato come abilitare CORS in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eab4d-112">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="eab4d-113">Che cos'è "stessa origine"?</span><span class="sxs-lookup"><span data-stu-id="eab4d-113">What is "same origin"?</span></span>

<span data-ttu-id="eab4d-114">Se dispongono di porte, host e gli schemi identici, due URL abbia la stessa origine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-114">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="eab4d-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="eab4d-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="eab4d-116">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="eab4d-116">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="eab4d-117">Questi URL sono origini diverse rispetto a quelli due:</span><span class="sxs-lookup"><span data-stu-id="eab4d-117">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="eab4d-118">`http://example.net`-Altro dominio</span><span class="sxs-lookup"><span data-stu-id="eab4d-118">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="eab4d-119">`http://www.example.com/foo.html`-Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="eab4d-119">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="eab4d-120">`https://example.com/foo.html`-Schema differente</span><span class="sxs-lookup"><span data-stu-id="eab4d-120">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="eab4d-121">`http://example.com:9000/foo.html`-Porta diversa</span><span class="sxs-lookup"><span data-stu-id="eab4d-121">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="eab4d-122">Internet Explorer non considera la porta quando si confrontano le origini.</span><span class="sxs-lookup"><span data-stu-id="eab4d-122">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="eab4d-123">Impostazione di CORS</span><span class="sxs-lookup"><span data-stu-id="eab4d-123">Setting up CORS</span></span>

<span data-ttu-id="eab4d-124">Per impostare CORS per l'applicazione aggiungere le `Microsoft.AspNetCore.Cors` pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="eab4d-124">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="eab4d-125">Aggiungere i servizi CORS in Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="eab4d-125">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="eab4d-126">Abilitazione di CORS con middleware</span><span class="sxs-lookup"><span data-stu-id="eab4d-126">Enabling CORS with middleware</span></span>

<span data-ttu-id="eab4d-127">Per abilitare CORS per l'intera applicazione, aggiungere il middleware CORS la richiesta di pipeline utilizzando il `UseCors` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-127">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="eab4d-128">Si noti che il middleware CORS deve precedere qualsiasi endpoint definiti nell'applicazione che si desidera supportare le richieste di multiorigine (ad esempio,</span><span class="sxs-lookup"><span data-stu-id="eab4d-128">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="eab4d-129">prima di qualsiasi chiamata a `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="eab4d-129">before any call to `UseMvc`).</span></span>

<span data-ttu-id="eab4d-130">È possibile specificare un criterio di cross-origin quando si aggiunge il middleware CORS tramite la `CorsPolicyBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="eab4d-130">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="eab4d-131">È possibile ottenere questo risultato in due modi.</span><span class="sxs-lookup"><span data-stu-id="eab4d-131">There are two ways to do this.</span></span> <span data-ttu-id="eab4d-132">Il primo consiste nel chiamare UseCors con un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="eab4d-132">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="eab4d-133">**Nota:** l'URL deve essere specificato senza una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="eab4d-133">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="eab4d-134">Se l'URL termina con `/`, verrà restituito il confronto `false` e non verrà restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-134">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="eab4d-135">L'espressione lambda ha un `CorsPolicyBuilder` oggetto.</span><span class="sxs-lookup"><span data-stu-id="eab4d-135">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="eab4d-136">È disponibile un elenco di [opzioni di configurazione](#cors-policy-options) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="eab4d-136">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="eab4d-137">In questo esempio, i criteri consentono le richieste tra le origini da `http://example.com` e non altre origini.</span><span class="sxs-lookup"><span data-stu-id="eab4d-137">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="eab4d-138">Si noti che CorsPolicyBuilder presenta un'API intuitiva, pertanto è possibile concatenare le chiamate al metodo:</span><span class="sxs-lookup"><span data-stu-id="eab4d-138">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="eab4d-139">Il secondo approccio consiste nel definire uno o più criteri CORS denominati e quindi selezionare i criteri in base al nome in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-139">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="eab4d-140">Questo esempio aggiunge un criterio CORS denominato "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="eab4d-140">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="eab4d-141">Per selezionare i criteri, passare il nome di `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="eab4d-141">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="eab4d-142">Abilitazione di CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="eab4d-142">Enabling CORS in MVC</span></span>

<span data-ttu-id="eab4d-143">In alternativa, è possibile utilizzare MVC per applicare specifiche CORS per ogni azione, per ogni controller o a livello globale per tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="eab4d-143">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="eab4d-144">Quando si utilizza MVC per abilitare CORS vengono utilizzati gli stessi servizi CORS, ma non il middleware CORS.</span><span class="sxs-lookup"><span data-stu-id="eab4d-144">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="eab4d-145">Per ogni azione</span><span class="sxs-lookup"><span data-stu-id="eab4d-145">Per action</span></span>

<span data-ttu-id="eab4d-146">Per specificare un criterio CORS per un'azione specifica aggiungere il `[EnableCors]` attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-146">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="eab4d-147">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="eab4d-147">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="eab4d-148">Per ogni controller</span><span class="sxs-lookup"><span data-stu-id="eab4d-148">Per controller</span></span>

<span data-ttu-id="eab4d-149">Per specificare il criterio CORS per un controller specifico aggiungere il `[EnableCors]` attributo alla classe controller.</span><span class="sxs-lookup"><span data-stu-id="eab4d-149">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="eab4d-150">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="eab4d-150">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="eab4d-151">A livello globale</span><span class="sxs-lookup"><span data-stu-id="eab4d-151">Globally</span></span>

<span data-ttu-id="eab4d-152">È possibile abilitare a livello globale CORS per tutti i controller mediante l'aggiunta di `CorsAuthorizationFilterFactory` filtro all'insieme di filtri globali:</span><span class="sxs-lookup"><span data-stu-id="eab4d-152">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="eab4d-153">L'ordine di precedenza è: azione, controller, globale.</span><span class="sxs-lookup"><span data-stu-id="eab4d-153">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="eab4d-154">A livello di azione criteri hanno la precedenza sui criteri a livello di controller e i criteri a livello di controller hanno la precedenza sui criteri globali.</span><span class="sxs-lookup"><span data-stu-id="eab4d-154">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="eab4d-155">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="eab4d-155">Disable CORS</span></span>

<span data-ttu-id="eab4d-156">Per disabilitare CORS per un controller o un'azione, utilizzare il `[DisableCors]` attributo.</span><span class="sxs-lookup"><span data-stu-id="eab4d-156">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="eab4d-157">Opzioni dei criteri CORS</span><span class="sxs-lookup"><span data-stu-id="eab4d-157">CORS policy options</span></span>

<span data-ttu-id="eab4d-158">Questa sezione descrive le varie opzioni che è possibile impostare un criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="eab4d-158">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="eab4d-159">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="eab4d-159">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="eab4d-160">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="eab4d-160">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="eab4d-161">Impostare le intestazioni di richieste consentito</span><span class="sxs-lookup"><span data-stu-id="eab4d-161">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="eab4d-162">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="eab4d-162">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="eab4d-163">Credenziali in richieste cross-origin</span><span class="sxs-lookup"><span data-stu-id="eab4d-163">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="eab4d-164">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="eab4d-164">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="eab4d-165">Per alcune opzioni può essere utile leggere [funziona come CORS](#how-cors-works) prima.</span><span class="sxs-lookup"><span data-stu-id="eab4d-165">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="eab4d-166">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="eab4d-166">Set the allowed origins</span></span>

<span data-ttu-id="eab4d-167">Per consentire a uno o più origini specifiche:</span><span class="sxs-lookup"><span data-stu-id="eab4d-167">To allow one or more specific origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="eab4d-168">Per consentire tutte le origini:</span><span class="sxs-lookup"><span data-stu-id="eab4d-168">To allow all origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="eab4d-169">Valutare con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-169">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="eab4d-170">Significa che letteralmente qualsiasi sito Web di effettuare chiamate AJAX all'API.</span><span class="sxs-lookup"><span data-stu-id="eab4d-170">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="eab4d-171">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="eab4d-171">Set the allowed HTTP methods</span></span>

<span data-ttu-id="eab4d-172">Per consentire a tutti i metodi HTTP:</span><span class="sxs-lookup"><span data-stu-id="eab4d-172">To allow all HTTP methods:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="eab4d-173">Questo influisce sulle richieste preliminari e intestazione Access-controllo-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="eab4d-173">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="eab4d-174">Impostare le intestazioni di richieste consentito</span><span class="sxs-lookup"><span data-stu-id="eab4d-174">Set the allowed request headers</span></span>

<span data-ttu-id="eab4d-175">Una richiesta preliminare CORS potrebbe includere un'intestazione Access-Control-Request-Headers, elenco di intestazioni HTTP impostate dall'applicazione (la cosiddetta "author intestazioni delle richieste").</span><span class="sxs-lookup"><span data-stu-id="eab4d-175">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="eab4d-176">A intestazioni specifiche whitelist:</span><span class="sxs-lookup"><span data-stu-id="eab4d-176">To whitelist specific headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="eab4d-177">Per consentire tutti creare intestazioni di richiesta:</span><span class="sxs-lookup"><span data-stu-id="eab4d-177">To allow all author request headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="eab4d-178">Browser non sono completamente coerenti in modalità impostano Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="eab4d-178">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="eab4d-179">Se si imposta le intestazioni a un valore diverso da "*", è necessario includere almeno "accetta", "content-type" e "origine", più eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="eab4d-179">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="eab4d-180">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="eab4d-180">Set the exposed response headers</span></span>

<span data-ttu-id="eab4d-181">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-181">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="eab4d-182">(Vedere [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="eab4d-182">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="eab4d-183">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="eab4d-183">Cache-Control</span></span>

* <span data-ttu-id="eab4d-184">Content-Language</span><span class="sxs-lookup"><span data-stu-id="eab4d-184">Content-Language</span></span>

* <span data-ttu-id="eab4d-185">Tipo di contenuto</span><span class="sxs-lookup"><span data-stu-id="eab4d-185">Content-Type</span></span>

* <span data-ttu-id="eab4d-186">Scadenza</span><span class="sxs-lookup"><span data-stu-id="eab4d-186">Expires</span></span>

* <span data-ttu-id="eab4d-187">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="eab4d-187">Last-Modified</span></span>

* <span data-ttu-id="eab4d-188">Pragma</span><span class="sxs-lookup"><span data-stu-id="eab4d-188">Pragma</span></span>

<span data-ttu-id="eab4d-189">Le specifiche CORS chiama questi *le intestazioni di risposta semplice*.</span><span class="sxs-lookup"><span data-stu-id="eab4d-189">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="eab4d-190">Per rendere disponibili per l'applicazione altre intestazioni:</span><span class="sxs-lookup"><span data-stu-id="eab4d-190">To make other headers available to the application:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="eab4d-191">Credenziali in richieste cross-origin</span><span class="sxs-lookup"><span data-stu-id="eab4d-191">Credentials in cross-origin requests</span></span>

<span data-ttu-id="eab4d-192">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="eab4d-192">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="eab4d-193">Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-193">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="eab4d-194">Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="eab4d-194">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="eab4d-195">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare XMLHttpRequest.withCredentials su true.</span><span class="sxs-lookup"><span data-stu-id="eab4d-195">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="eab4d-196">Utilizzando XMLHttpRequest direttamente:</span><span class="sxs-lookup"><span data-stu-id="eab4d-196">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="eab4d-197">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="eab4d-197">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="eab4d-198">Inoltre, il server deve concedere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="eab4d-198">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="eab4d-199">Per consentire le credenziali di cross-origin:</span><span class="sxs-lookup"><span data-stu-id="eab4d-199">To allow cross-origin credentials:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="eab4d-200">Ora la risposta HTTP includerà un'intestazione controllo-Consenti-le credenziali di accesso, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-200">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="eab4d-201">Se il browser invia le credenziali, ma la risposta non include un'intestazione controllo-Consenti-le credenziali di accesso valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="eab4d-201">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="eab4d-202">Prestare molta attenzione su come consentire tra le origini credenziali, in quanto significa che un sito Web in un altro dominio può inviare le credenziali dell'utente connesso all'applicazione per conto dell'utente, senza che l'utente viene presa in considerazione.</span><span class="sxs-lookup"><span data-stu-id="eab4d-202">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="eab4d-203">CORS della specifica anche gli stati di origine di impostazione da "*" (tutte le origini) non è valido se l'intestazione controllo-Consenti-le credenziali di accesso è presente.</span><span class="sxs-lookup"><span data-stu-id="eab4d-203">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="eab4d-204">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="eab4d-204">Set the preflight expiration time</span></span>

<span data-ttu-id="eab4d-205">L'intestazione di controllo accesso-Max-Age specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare.</span><span class="sxs-lookup"><span data-stu-id="eab4d-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="eab4d-206">Per impostare questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="eab4d-206">To set this header:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="eab4d-207">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="eab4d-207">How CORS works</span></span>

<span data-ttu-id="eab4d-208">In questa sezione descrive cosa accade in una richiesta CORS, a livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="eab4d-208">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="eab4d-209">È importante comprendere come funziona la condivisione CORS, in modo che è possibile configurare il criterio CORS correttamente e risolvere i problemi se ciò non funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="eab4d-209">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="eab4d-210">La specifica CORS introduce diverse nuove intestazioni HTTP che consentono di richieste cross-origin.</span><span class="sxs-lookup"><span data-stu-id="eab4d-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="eab4d-211">Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste di cross-origin. non è necessario eseguire alcuna operazione speciali nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="eab4d-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="eab4d-212">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="eab4d-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="eab4d-213">L'intestazione "Origin" fornisce il dominio del sito che effettua la richiesta:</span><span class="sxs-lookup"><span data-stu-id="eab4d-213">The "Origin" header gives the domain of the site that is making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="eab4d-214">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin nella risposta.</span><span class="sxs-lookup"><span data-stu-id="eab4d-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="eab4d-215">Il valore di questa intestazione corrisponde all'intestazione di origine dalla richiesta o è il valore del carattere jolly "*", che significa che è consentita qualsiasi origine:</span><span class="sxs-lookup"><span data-stu-id="eab4d-215">The value of this header either matches the Origin header from the request, or is the wildcard value "*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="eab4d-216">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="eab4d-216">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="eab4d-217">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="eab4d-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="eab4d-218">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="eab4d-218">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="eab4d-219">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="eab4d-219">Preflight Requests</span></span>

<span data-ttu-id="eab4d-220">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare," prima dell'invio della richiesta per la risorsa effettiva.</span><span class="sxs-lookup"><span data-stu-id="eab4d-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="eab4d-221">Il browser è possibile ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab4d-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="eab4d-222">Il metodo di richiesta è GET, HEAD o POST, e</span><span class="sxs-lookup"><span data-stu-id="eab4d-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="eab4d-223">L'applicazione non imposta le intestazioni di richiesta diverso da Accept, Accept-Language, Content-Language, Content-Type o ID di evento Last, e</span><span class="sxs-lookup"><span data-stu-id="eab4d-223">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="eab4d-224">L'intestazione Content-Type (se impostato) è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="eab4d-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="eab4d-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="eab4d-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="eab4d-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="eab4d-226">multipart/form-data</span></span>

  * <span data-ttu-id="eab4d-227">Testo</span><span class="sxs-lookup"><span data-stu-id="eab4d-227">text/plain</span></span>

<span data-ttu-id="eab4d-228">Si applica la regola sulle intestazioni di richiesta per le intestazioni che l'applicazione imposta chiamando setRequestHeader sull'oggetto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="eab4d-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="eab4d-229">(Specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni in cui è possibile impostare il browser, ad esempio User-Agent, l'Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="eab4d-229">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="eab4d-230">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="eab4d-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="eab4d-231">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="eab4d-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="eab4d-232">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="eab4d-232">It includes two special headers:</span></span>

* <span data-ttu-id="eab4d-233">Access-Control-Request-Method: Il metodo HTTP utilizzato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="eab4d-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="eab4d-234">Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che l'applicazione è impostata su richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="eab4d-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="eab4d-235">(Nuovamente, questo non includono le intestazioni che imposta il browser).</span><span class="sxs-lookup"><span data-stu-id="eab4d-235">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="eab4d-236">Di seguito è riportata una risposta di esempio, supponendo che il server consenta la richiesta:</span><span class="sxs-lookup"><span data-stu-id="eab4d-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="eab4d-237">La risposta include un'intestazione Access-controllo-Allow-Methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="eab4d-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="eab4d-238">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="eab4d-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
