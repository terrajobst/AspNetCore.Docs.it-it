---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste cross-origin in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077548"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="866b0-103">Abilitare le richieste tra le origini (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="866b0-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="866b0-104">Da [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="866b0-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="866b0-105">Protezione del browser impedisce che una pagina web effettua le richieste AJAX a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="866b0-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="866b0-106">Questa restrizione viene chiamata il *criteri stessa origine*e impedisce a un sito di leggere i dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="866b0-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="866b0-107">Tuttavia, in alcuni casi è consigliabile consentire ad altri siti richieste cross-origin per l'API web.</span><span class="sxs-lookup"><span data-stu-id="866b0-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="866b0-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server per ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="866b0-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="866b0-109">Tramite condivisione CORS, un server può consentire in modo esplicito alcune richieste cross-origin durante il rifiuto di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="866b0-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="866b0-110">CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="866b0-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="866b0-111">In questo argomento viene illustrato come abilitare CORS in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="866b0-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="866b0-112">Che cos'è "stessa origine"?</span><span class="sxs-lookup"><span data-stu-id="866b0-112">What is "same origin"?</span></span>

<span data-ttu-id="866b0-113">Se dispongono di porte, host e gli schemi identici, due URL abbia la stessa origine.</span><span class="sxs-lookup"><span data-stu-id="866b0-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="866b0-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="866b0-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="866b0-115">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="866b0-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="866b0-116">Questi URL sono origini diverse rispetto a quelli due:</span><span class="sxs-lookup"><span data-stu-id="866b0-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="866b0-117">`http://example.net` -Altro dominio</span><span class="sxs-lookup"><span data-stu-id="866b0-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="866b0-118">`http://www.example.com/foo.html` -Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="866b0-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="866b0-119">`https://example.com/foo.html` -Schema differente</span><span class="sxs-lookup"><span data-stu-id="866b0-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="866b0-120">`http://example.com:9000/foo.html` -Porta diversa</span><span class="sxs-lookup"><span data-stu-id="866b0-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="866b0-121">Internet Explorer non viene considerata la porta quando si confrontano le origini.</span><span class="sxs-lookup"><span data-stu-id="866b0-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="866b0-122">Impostazione di CORS</span><span class="sxs-lookup"><span data-stu-id="866b0-122">Setting up CORS</span></span>

<span data-ttu-id="866b0-123">Per impostare CORS per l'applicazione aggiungere le `Microsoft.AspNetCore.Cors` pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="866b0-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="866b0-124">Aggiungere i servizi CORS in Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="866b0-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="866b0-125">Abilitazione di CORS con middleware</span><span class="sxs-lookup"><span data-stu-id="866b0-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="866b0-126">Per abilitare CORS per l'intera applicazione, aggiungere il middleware CORS la richiesta di pipeline utilizzando il `UseCors` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="866b0-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="866b0-127">Si noti che il middleware CORS deve precedere qualsiasi endpoint definiti nell'applicazione che si desidera supportare le richieste di multiorigine (ad esempio,</span><span class="sxs-lookup"><span data-stu-id="866b0-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="866b0-128">prima di qualsiasi chiamata a `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="866b0-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="866b0-129">È possibile specificare un criterio di cross-origin quando si aggiunge il middleware CORS tramite la `CorsPolicyBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="866b0-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="866b0-130">È possibile ottenere questo risultato in due modi.</span><span class="sxs-lookup"><span data-stu-id="866b0-130">There are two ways to do this.</span></span> <span data-ttu-id="866b0-131">Il primo consiste nel chiamare UseCors con un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="866b0-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="866b0-132">**Nota:** l'URL deve essere specificato senza una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="866b0-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="866b0-133">Se l'URL termina con `/`, verrà restituito il confronto `false` e non verrà restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="866b0-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="866b0-134">L'espressione lambda ha un `CorsPolicyBuilder` oggetto.</span><span class="sxs-lookup"><span data-stu-id="866b0-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="866b0-135">È disponibile un elenco di [opzioni di configurazione](#cors-policy-options) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="866b0-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="866b0-136">In questo esempio, i criteri consentono le richieste tra le origini da `http://example.com` e non altre origini.</span><span class="sxs-lookup"><span data-stu-id="866b0-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="866b0-137">Si noti che CorsPolicyBuilder presenta un'API intuitiva, pertanto è possibile concatenare le chiamate al metodo:</span><span class="sxs-lookup"><span data-stu-id="866b0-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="866b0-138">Il secondo approccio consiste nel definire uno o più criteri CORS denominati e quindi selezionare i criteri in base al nome in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="866b0-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="866b0-139">Questo esempio aggiunge un criterio CORS denominato "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="866b0-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="866b0-140">Per selezionare i criteri, passare il nome di `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="866b0-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="866b0-141">Abilitazione di CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="866b0-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="866b0-142">In alternativa, è possibile utilizzare MVC per applicare specifiche CORS per ogni azione, per ogni controller o a livello globale per tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="866b0-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="866b0-143">Quando si utilizza MVC per abilitare CORS vengono utilizzati gli stessi servizi CORS, ma non il middleware CORS.</span><span class="sxs-lookup"><span data-stu-id="866b0-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="866b0-144">Per ogni azione</span><span class="sxs-lookup"><span data-stu-id="866b0-144">Per action</span></span>

<span data-ttu-id="866b0-145">Per specificare un criterio CORS per un'azione specifica aggiungere il `[EnableCors]` attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="866b0-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="866b0-146">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="866b0-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="866b0-147">Per ogni controller</span><span class="sxs-lookup"><span data-stu-id="866b0-147">Per controller</span></span>

<span data-ttu-id="866b0-148">Per specificare il criterio CORS per un controller specifico aggiungere il `[EnableCors]` attributo alla classe controller.</span><span class="sxs-lookup"><span data-stu-id="866b0-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="866b0-149">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="866b0-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="866b0-150">A livello globale</span><span class="sxs-lookup"><span data-stu-id="866b0-150">Globally</span></span>

<span data-ttu-id="866b0-151">È possibile abilitare a livello globale CORS per tutti i controller mediante l'aggiunta di `CorsAuthorizationFilterFactory` filtro all'insieme di filtri globali:</span><span class="sxs-lookup"><span data-stu-id="866b0-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="866b0-152">L'ordine di precedenza è: azione, controller, globale.</span><span class="sxs-lookup"><span data-stu-id="866b0-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="866b0-153">A livello di azione criteri hanno la precedenza sui criteri a livello di controller e i criteri a livello di controller hanno la precedenza sui criteri globali.</span><span class="sxs-lookup"><span data-stu-id="866b0-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="866b0-154">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="866b0-154">Disable CORS</span></span>

<span data-ttu-id="866b0-155">Per disabilitare CORS per un controller o un'azione, utilizzare il `[DisableCors]` attributo.</span><span class="sxs-lookup"><span data-stu-id="866b0-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="866b0-156">Opzioni dei criteri CORS</span><span class="sxs-lookup"><span data-stu-id="866b0-156">CORS policy options</span></span>

<span data-ttu-id="866b0-157">Questa sezione descrive le varie opzioni che è possibile impostare un criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="866b0-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="866b0-158">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="866b0-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="866b0-159">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="866b0-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="866b0-160">Impostare le intestazioni di richieste consentito</span><span class="sxs-lookup"><span data-stu-id="866b0-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="866b0-161">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="866b0-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="866b0-162">Credenziali in richieste cross-origin</span><span class="sxs-lookup"><span data-stu-id="866b0-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="866b0-163">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="866b0-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="866b0-164">Per alcune opzioni può essere utile leggere [funziona come CORS](#how-cors-works) prima.</span><span class="sxs-lookup"><span data-stu-id="866b0-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="866b0-165">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="866b0-165">Set the allowed origins</span></span>

<span data-ttu-id="866b0-166">Per consentire a uno o più origini specifiche:</span><span class="sxs-lookup"><span data-stu-id="866b0-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="866b0-167">Per consentire tutte le origini:</span><span class="sxs-lookup"><span data-stu-id="866b0-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="866b0-168">Valutare con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="866b0-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="866b0-169">Significa che letteralmente qualsiasi sito Web di effettuare chiamate AJAX all'API.</span><span class="sxs-lookup"><span data-stu-id="866b0-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="866b0-170">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="866b0-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="866b0-171">Per consentire a tutti i metodi HTTP:</span><span class="sxs-lookup"><span data-stu-id="866b0-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="866b0-172">Questo influisce sulle richieste preliminari e intestazione Access-controllo-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="866b0-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="866b0-173">Impostare le intestazioni di richieste consentito</span><span class="sxs-lookup"><span data-stu-id="866b0-173">Set the allowed request headers</span></span>

<span data-ttu-id="866b0-174">Una richiesta preliminare CORS potrebbe includere un'intestazione Access-Control-Request-Headers, elenco di intestazioni HTTP impostate dall'applicazione (la cosiddetta "author intestazioni delle richieste").</span><span class="sxs-lookup"><span data-stu-id="866b0-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="866b0-175">A intestazioni specifiche whitelist:</span><span class="sxs-lookup"><span data-stu-id="866b0-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="866b0-176">Per consentire tutti creare intestazioni di richiesta:</span><span class="sxs-lookup"><span data-stu-id="866b0-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="866b0-177">Browser non sono completamente coerenti in modalità impostano Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="866b0-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="866b0-178">Se si imposta le intestazioni a un valore diverso da "\*", è necessario includere almeno "accetta", "content-type" e "origine", più eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="866b0-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="866b0-179">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="866b0-179">Set the exposed response headers</span></span>

<span data-ttu-id="866b0-180">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="866b0-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="866b0-181">(Vedere [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="866b0-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="866b0-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="866b0-182">Cache-Control</span></span>

* <span data-ttu-id="866b0-183">Content-Language</span><span class="sxs-lookup"><span data-stu-id="866b0-183">Content-Language</span></span>

* <span data-ttu-id="866b0-184">Content-Type</span><span class="sxs-lookup"><span data-stu-id="866b0-184">Content-Type</span></span>

* <span data-ttu-id="866b0-185">Scadenza</span><span class="sxs-lookup"><span data-stu-id="866b0-185">Expires</span></span>

* <span data-ttu-id="866b0-186">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="866b0-186">Last-Modified</span></span>

* <span data-ttu-id="866b0-187">Pragma</span><span class="sxs-lookup"><span data-stu-id="866b0-187">Pragma</span></span>

<span data-ttu-id="866b0-188">Le specifiche CORS chiama questi *le intestazioni di risposta semplice*.</span><span class="sxs-lookup"><span data-stu-id="866b0-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="866b0-189">Per rendere disponibili per l'applicazione altre intestazioni:</span><span class="sxs-lookup"><span data-stu-id="866b0-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="866b0-190">Credenziali in richieste cross-origin</span><span class="sxs-lookup"><span data-stu-id="866b0-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="866b0-191">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="866b0-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="866b0-192">Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="866b0-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="866b0-193">Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="866b0-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="866b0-194">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare XMLHttpRequest.withCredentials su true.</span><span class="sxs-lookup"><span data-stu-id="866b0-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="866b0-195">Utilizzando XMLHttpRequest direttamente:</span><span class="sxs-lookup"><span data-stu-id="866b0-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="866b0-196">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="866b0-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="866b0-197">Inoltre, il server deve concedere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="866b0-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="866b0-198">Per consentire le credenziali di cross-origin:</span><span class="sxs-lookup"><span data-stu-id="866b0-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="866b0-199">Ora la risposta HTTP includerà un'intestazione controllo-Consenti-le credenziali di accesso, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="866b0-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="866b0-200">Se il browser invia le credenziali, ma la risposta non include un'intestazione controllo-Consenti-le credenziali di accesso valida, il browser non esporre la risposta all'applicazione e la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="866b0-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="866b0-201">Prestare attenzione quando si utilizza le credenziali di cross-origin.</span><span class="sxs-lookup"><span data-stu-id="866b0-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="866b0-202">Un sito Web in un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="866b0-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="866b0-203">La specifica di CORS indica anche tale impostazione origine da "\*" (tutte le origini) non è valido se il `Access-Control-Allow-Credentials` intestazione è presente.</span><span class="sxs-lookup"><span data-stu-id="866b0-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="866b0-204">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="866b0-204">Set the preflight expiration time</span></span>

<span data-ttu-id="866b0-205">L'intestazione di controllo accesso-Max-Age specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare.</span><span class="sxs-lookup"><span data-stu-id="866b0-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="866b0-206">Per impostare questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="866b0-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="866b0-207">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="866b0-207">How CORS works</span></span>

<span data-ttu-id="866b0-208">In questa sezione descrive cosa accade in una richiesta CORS al livello di messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="866b0-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="866b0-209">È importante comprendere come funziona la condivisione CORS in modo che il criterio CORS può essere configurato correttamente e risolverli verificarsi comportamenti imprevisti.</span><span class="sxs-lookup"><span data-stu-id="866b0-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="866b0-210">La specifica CORS introduce diverse nuove intestazioni HTTP che consentono di richieste cross-origin.</span><span class="sxs-lookup"><span data-stu-id="866b0-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="866b0-211">Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste cross-origin.</span><span class="sxs-lookup"><span data-stu-id="866b0-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="866b0-212">Il codice JavaScript personalizzato non è necessario abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="866b0-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="866b0-213">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="866b0-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="866b0-214">Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:</span><span class="sxs-lookup"><span data-stu-id="866b0-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="866b0-215">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin nella risposta.</span><span class="sxs-lookup"><span data-stu-id="866b0-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="866b0-216">Il valore di questa intestazione corrisponde all'intestazione di origine dalla richiesta o è il valore del carattere jolly "\*", che significa che è consentita qualsiasi origine:</span><span class="sxs-lookup"><span data-stu-id="866b0-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="866b0-217">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="866b0-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="866b0-218">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="866b0-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="866b0-219">Anche se il server restituisce una risposta con esito positivo, il browser non verificare la risposta disponibili per l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="866b0-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="866b0-220">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="866b0-220">Preflight Requests</span></span>

<span data-ttu-id="866b0-221">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare," prima dell'invio della richiesta per la risorsa effettiva.</span><span class="sxs-lookup"><span data-stu-id="866b0-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="866b0-222">Il browser è possibile ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="866b0-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="866b0-223">Il metodo di richiesta è GET, HEAD o POST, e</span><span class="sxs-lookup"><span data-stu-id="866b0-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="866b0-224">L'applicazione non imposta le intestazioni di richiesta diverso da Accept, Accept-Language, Content-Language, Content-Type o ID di evento Last, e</span><span class="sxs-lookup"><span data-stu-id="866b0-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="866b0-225">L'intestazione Content-Type (se impostato) è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="866b0-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="866b0-226">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="866b0-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="866b0-227">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="866b0-227">multipart/form-data</span></span>

  * <span data-ttu-id="866b0-228">Testo</span><span class="sxs-lookup"><span data-stu-id="866b0-228">text/plain</span></span>

<span data-ttu-id="866b0-229">Si applica la regola sulle intestazioni di richiesta per le intestazioni che l'applicazione imposta chiamando setRequestHeader sull'oggetto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="866b0-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="866b0-230">(Specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni in cui è possibile impostare il browser, ad esempio User-Agent, l'Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="866b0-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="866b0-231">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="866b0-231">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="866b0-232">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="866b0-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="866b0-233">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="866b0-233">It includes two special headers:</span></span>

* <span data-ttu-id="866b0-234">Access-Control-Request-Method: Il metodo HTTP utilizzato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="866b0-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="866b0-235">Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che l'applicazione è impostata su richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="866b0-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="866b0-236">(Nuovamente, questo non include le intestazioni che imposta il browser.)</span><span class="sxs-lookup"><span data-stu-id="866b0-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="866b0-237">Di seguito è riportata una risposta di esempio, supponendo che il server consenta la richiesta:</span><span class="sxs-lookup"><span data-stu-id="866b0-237">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="866b0-238">La risposta include un'intestazione Access-controllo-Allow-Methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="866b0-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="866b0-239">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="866b0-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
