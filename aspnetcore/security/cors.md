---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2018
ms.locfileid: "41837485"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="ee722-103">Abilitare le richieste Multiorigine (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee722-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="ee722-104">Dal [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ee722-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ee722-105">Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="ee722-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="ee722-106">Questa restrizione viene chiamata il *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="ee722-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ee722-107">Tuttavia, in alcuni casi si potrebbe voler consentire ad altri siti di effettuare richieste multiorigine nell'API web.</span><span class="sxs-lookup"><span data-stu-id="ee722-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="ee722-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="ee722-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="ee722-109">Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="ee722-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="ee722-110">CORS è più sicuri e flessibili rispetto a tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="ee722-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="ee722-111">Questo argomento illustra come abilitare CORS in un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee722-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="ee722-112">Che cos'è "origin stesso"?</span><span class="sxs-lookup"><span data-stu-id="ee722-112">What is "same origin"?</span></span>

<span data-ttu-id="ee722-113">Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici.</span><span class="sxs-lookup"><span data-stu-id="ee722-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="ee722-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="ee722-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="ee722-115">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="ee722-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="ee722-116">Questi URL sono le entità Origin diversa rispetto al precedente due:</span><span class="sxs-lookup"><span data-stu-id="ee722-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="ee722-117">`http://example.net` -Altro dominio</span><span class="sxs-lookup"><span data-stu-id="ee722-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="ee722-118">`http://www.example.com/foo.html` -Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="ee722-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="ee722-119">`https://example.com/foo.html` -Schema differente</span><span class="sxs-lookup"><span data-stu-id="ee722-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="ee722-120">`http://example.com:9000/foo.html` -Porta diversa</span><span class="sxs-lookup"><span data-stu-id="ee722-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="ee722-121">Internet Explorer non considera la porta quando si confrontano le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="ee722-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="ee722-122">Abilitare CORS</span><span class="sxs-lookup"><span data-stu-id="ee722-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ee722-123">Per configurare CORS per l'applicazione, aggiungere il `Microsoft.AspNetCore.Cors` pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="ee722-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="ee722-124">Chiamare [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ee722-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="ee722-125">L'abilitazione di CORS con il middleware</span><span class="sxs-lookup"><span data-stu-id="ee722-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="ee722-126">Per abilitare CORS, aggiungere il middleware CORS alla pipeline richiesta utilizzando il `UseCors` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="ee722-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="ee722-127">Il middleware CORS deve precedere qualsiasi endpoint definito nell'app in cui si desidera supportare richieste multiorigine (ad esempio, prima di qualsiasi chiamata a `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="ee722-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="ee722-128">È possibile specificare un criteri multiorigine quando si aggiunge il middleware CORS usando la [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) classe.</span><span class="sxs-lookup"><span data-stu-id="ee722-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="ee722-129">È possibile ottenere questo risultato in due modi.</span><span class="sxs-lookup"><span data-stu-id="ee722-129">There are two ways to do this.</span></span> <span data-ttu-id="ee722-130">Il primo consiste nel chiamare `UseCors` con un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="ee722-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="ee722-131">**Nota:** deve specificare l'URL senza una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="ee722-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="ee722-132">Se l'URL termina con `/`, verrà restituito il confronto `false` e non verrà restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="ee722-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="ee722-133">L'operatore lambda accetta un `CorsPolicyBuilder` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ee722-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="ee722-134">Troverai un elenco del [opzioni di configurazione](#cors-policy-options) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="ee722-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="ee722-135">In questo esempio, il criterio consente le richieste multiorigine dal `http://example.com` e non le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="ee722-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="ee722-136">CorsPolicyBuilder ha un'API fluent, pertanto è possibile concatenare chiamate al metodo:</span><span class="sxs-lookup"><span data-stu-id="ee722-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="ee722-137">Il secondo approccio consiste nel definire uno o più criteri CORS denominati e quindi selezionare il criterio in base al nome in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ee722-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="ee722-138">Questo esempio viene aggiunto un criterio CORS denominato "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="ee722-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="ee722-139">Per selezionare i criteri, passare il nome da `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="ee722-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="ee722-140">Abilitazione della condivisione CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="ee722-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="ee722-141">In alternativa, è possibile usare MVC per applicare CORS specifici per ogni azione, per ogni controller o a livello globale per tutti i controller.</span><span class="sxs-lookup"><span data-stu-id="ee722-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="ee722-142">Quando si usa MVC per abilitare CORS vengono usati gli stessi servizi CORS, ma non è il middleware CORS.</span><span class="sxs-lookup"><span data-stu-id="ee722-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="ee722-143">Per ogni azione</span><span class="sxs-lookup"><span data-stu-id="ee722-143">Per action</span></span>

<span data-ttu-id="ee722-144">Per specificare un criterio CORS per un'azione specifica, aggiungere il `[EnableCors]` attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="ee722-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="ee722-145">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="ee722-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="ee722-146">Per ogni controller</span><span class="sxs-lookup"><span data-stu-id="ee722-146">Per controller</span></span>

<span data-ttu-id="ee722-147">Per specificare i criteri CORS per un controller specifico, aggiungere il `[EnableCors]` attributo alla classe controller.</span><span class="sxs-lookup"><span data-stu-id="ee722-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="ee722-148">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="ee722-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="ee722-149">A livello globale</span><span class="sxs-lookup"><span data-stu-id="ee722-149">Globally</span></span>

<span data-ttu-id="ee722-150">È possibile abilitare a livello globale CORS per tutti i controller mediante l'aggiunta di `CorsAuthorizationFilterFactory` filtro alla raccolta di filtri globali:</span><span class="sxs-lookup"><span data-stu-id="ee722-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="ee722-151">L'ordine di precedenza è: azione, controller, globale.</span><span class="sxs-lookup"><span data-stu-id="ee722-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="ee722-152">I criteri a livello di azione hanno la precedenza sui criteri a livello di controller e i criteri a livello di controller hanno la precedenza sui criteri globali.</span><span class="sxs-lookup"><span data-stu-id="ee722-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="ee722-153">Disabilitare la condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="ee722-153">Disable CORS</span></span>

<span data-ttu-id="ee722-154">Per disabilitare CORS per un controller o azione, usare il `[DisableCors]` attributo.</span><span class="sxs-lookup"><span data-stu-id="ee722-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="ee722-155">Opzioni dei criteri CORS</span><span class="sxs-lookup"><span data-stu-id="ee722-155">CORS policy options</span></span>

<span data-ttu-id="ee722-156">Questa sezione descrive le varie opzioni che è possibile impostare in un criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="ee722-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="ee722-157">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="ee722-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="ee722-158">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="ee722-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="ee722-159">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="ee722-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="ee722-160">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="ee722-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="ee722-161">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="ee722-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="ee722-162">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="ee722-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="ee722-163">Per alcune opzioni, potrebbe essere utile leggere [funziona come CORS](#how-cors-works) prima.</span><span class="sxs-lookup"><span data-stu-id="ee722-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="ee722-164">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="ee722-164">Set the allowed origins</span></span>

<span data-ttu-id="ee722-165">Per consentire a uno o più origini specifiche:</span><span class="sxs-lookup"><span data-stu-id="ee722-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="ee722-166">Consentire tutte le origini:</span><span class="sxs-lookup"><span data-stu-id="ee722-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="ee722-167">Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="ee722-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="ee722-168">Significa che letteralmente qualsiasi sito Web può eseguire chiamate AJAX all'API.</span><span class="sxs-lookup"><span data-stu-id="ee722-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="ee722-169">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="ee722-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="ee722-170">Per consentire a tutti i metodi HTTP:</span><span class="sxs-lookup"><span data-stu-id="ee722-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="ee722-171">Questo influisce sulle richieste preliminari e dell'intestazione Access-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="ee722-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="ee722-172">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="ee722-172">Set the allowed request headers</span></span>

<span data-ttu-id="ee722-173">Una richiesta CORS preliminare può includere un'intestazione Access-Control-Request-Headers, elenca le intestazioni HTTP impostate dall'applicazione (la cosiddetta "creare le intestazioni delle richieste").</span><span class="sxs-lookup"><span data-stu-id="ee722-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="ee722-174">Alle intestazioni specifiche whitelist:</span><span class="sxs-lookup"><span data-stu-id="ee722-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="ee722-175">Per consentire tutti gli autori il intestazioni della richiesta:</span><span class="sxs-lookup"><span data-stu-id="ee722-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="ee722-176">I browser non sono completamente coerenti nel modo in cui siano impostate Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="ee722-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="ee722-177">Se si imposta le intestazioni a qualsiasi elemento diverso da "\*", è necessario includere almeno "accept", "content-type" e "origin", oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="ee722-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="ee722-178">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="ee722-178">Set the exposed response headers</span></span>

<span data-ttu-id="ee722-179">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ee722-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="ee722-180">(Vedere [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="ee722-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="ee722-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="ee722-181">Cache-Control</span></span>

* <span data-ttu-id="ee722-182">Content-Language</span><span class="sxs-lookup"><span data-stu-id="ee722-182">Content-Language</span></span>

* <span data-ttu-id="ee722-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="ee722-183">Content-Type</span></span>

* <span data-ttu-id="ee722-184">Alla scadenza</span><span class="sxs-lookup"><span data-stu-id="ee722-184">Expires</span></span>

* <span data-ttu-id="ee722-185">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="ee722-185">Last-Modified</span></span>

* <span data-ttu-id="ee722-186">(Pragma)</span><span class="sxs-lookup"><span data-stu-id="ee722-186">Pragma</span></span>

<span data-ttu-id="ee722-187">La specifica CORS chiama questi *intestazioni di risposta semplice*.</span><span class="sxs-lookup"><span data-stu-id="ee722-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="ee722-188">Per rendere disponibili per l'applicazione altre intestazioni:</span><span class="sxs-lookup"><span data-stu-id="ee722-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="ee722-189">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="ee722-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="ee722-190">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="ee722-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="ee722-191">Per impostazione predefinita, il browser non invia credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="ee722-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="ee722-192">Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee722-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="ee722-193">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare XMLHttpRequest.withCredentials su true.</span><span class="sxs-lookup"><span data-stu-id="ee722-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="ee722-194">Uso diretto di XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="ee722-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="ee722-195">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="ee722-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="ee722-196">Inoltre, il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="ee722-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="ee722-197">Per consentire le credenziali di cross-origin:</span><span class="sxs-lookup"><span data-stu-id="ee722-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="ee722-198">Ora la risposta HTTP includerà un'intestazione di accesso-controllo-Allow-Credentials, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="ee722-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="ee722-199">Se il browser invia le credenziali, ma la risposta non include un'intestazione di accesso-controllo-Allow-Credentials valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="ee722-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="ee722-200">Prestare attenzione quando si consente le credenziali di cross-origin.</span><span class="sxs-lookup"><span data-stu-id="ee722-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="ee722-201">Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente ha eseguito l'accesso all'app per conto dell'utente senza il consenso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ee722-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="ee722-202">La specifica CORS indica anche che l'impostazione origini `"*"` (tutte le origini) non è valido se la `Access-Control-Allow-Credentials` intestazione è presente.</span><span class="sxs-lookup"><span data-stu-id="ee722-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="ee722-203">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="ee722-203">Set the preflight expiration time</span></span>

<span data-ttu-id="ee722-204">L'intestazione di accesso-controllo-Max-Age specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare.</span><span class="sxs-lookup"><span data-stu-id="ee722-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="ee722-205">Per impostare questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="ee722-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="ee722-206">Funzionamento della condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="ee722-206">How CORS works</span></span>

<span data-ttu-id="ee722-207">In questa sezione viene descritto cosa succede in una richiesta CORS a livello di messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee722-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="ee722-208">È importante comprendere il funzionamento di CORS in modo che il criterio CORS può essere configurato correttamente e il debug quando si verificano comportamenti imprevisti.</span><span class="sxs-lookup"><span data-stu-id="ee722-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="ee722-209">La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="ee722-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="ee722-210">Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="ee722-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="ee722-211">Il codice JavaScript personalizzato non è necessario abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="ee722-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="ee722-212">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="ee722-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="ee722-213">Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:</span><span class="sxs-lookup"><span data-stu-id="ee722-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="ee722-214">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin nella risposta.</span><span class="sxs-lookup"><span data-stu-id="ee722-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="ee722-215">Il valore di questa intestazione corrisponde all'intestazione di origine della richiesta o è il valore del carattere jolly "\*", che significa che è consentita qualsiasi origine:</span><span class="sxs-lookup"><span data-stu-id="ee722-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="ee722-216">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="ee722-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="ee722-217">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="ee722-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="ee722-218">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="ee722-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="ee722-219">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="ee722-219">Preflight Requests</span></span>

<span data-ttu-id="ee722-220">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, definita "richiesta preliminare," prima dell'invio della richiesta effettiva per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="ee722-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="ee722-221">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ee722-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="ee722-222">Il metodo di richiesta è GET, HEAD o POST, e</span><span class="sxs-lookup"><span data-stu-id="ee722-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="ee722-223">L'applicazione non imposta le intestazioni di richiesta diversa da Accept, Accept-Language, Content-Language, Content-Type o Last-eventi-ID, e</span><span class="sxs-lookup"><span data-stu-id="ee722-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="ee722-224">L'intestazione Content-Type (se impostato) è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="ee722-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="ee722-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="ee722-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="ee722-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="ee722-226">multipart/form-data</span></span>

  * <span data-ttu-id="ee722-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="ee722-227">text/plain</span></span>

<span data-ttu-id="ee722-228">Si applica la regola sulle intestazioni di richiesta alle intestazioni che l'applicazione imposta chiamando setRequestHeader sull'oggetto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="ee722-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="ee722-229">(Specifica CORS chiama questi "intestazioni di richiesta autore"). La regola non si applica alle intestazioni in cui è possibile impostare il browser, ad esempio User-Agent, Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="ee722-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="ee722-230">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="ee722-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="ee722-231">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee722-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="ee722-232">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="ee722-232">It includes two special headers:</span></span>

* <span data-ttu-id="ee722-233">Access-Control-Request-Method: Metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="ee722-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="ee722-234">Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che l'applicazione è impostata su richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="ee722-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="ee722-235">(Anche in questo caso, ciò non include le intestazioni che il browser imposta).</span><span class="sxs-lookup"><span data-stu-id="ee722-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="ee722-236">Ecco un esempio di risposta, presupponendo che il server consente la richiesta:</span><span class="sxs-lookup"><span data-stu-id="ee722-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="ee722-237">La risposta include un'intestazione Access-Control-Allow-Methods in cui sono elencati i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="ee722-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="ee722-238">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ee722-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
