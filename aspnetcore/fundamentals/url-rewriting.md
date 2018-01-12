---
title: Middleware in ASP.NET Core di riscrittura URL
author: guardrex
description: Informazioni sull'URL di riscrittura e il reindirizzamento tramite il Middleware di riscrittura URL nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, la riscrittura URL, URL rewrite, URL di reindirizzamento, reindirizzamento dell'URL, middleware, apache_mod
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: e07634a6d7ad97bf8735029b5c28d6935b71eb52
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="8046a-104">Middleware in ASP.NET Core di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="8046a-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="8046a-105">Da [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="8046a-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="8046a-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8046a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8046a-107">La riscrittura URL è l'azione di modifica richiesta URL in base a uno o più regole predefinite.</span><span class="sxs-lookup"><span data-stu-id="8046a-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="8046a-108">La riscrittura URL crea un'astrazione tra i percorsi delle risorse e i relativi indirizzi in modo che i percorsi e gli indirizzi non sono strettamente collegati.</span><span class="sxs-lookup"><span data-stu-id="8046a-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="8046a-109">Esistono diversi scenari in cui la riscrittura URL è utile:</span><span class="sxs-lookup"><span data-stu-id="8046a-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="8046a-110">Spostando o sostituendo le risorse del server, temporaneamente o definitivamente, mantenendo i localizzatori stabili per le risorse</span><span class="sxs-lookup"><span data-stu-id="8046a-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="8046a-111">La suddivisione in diverse App o in aree di un'applicazione di elaborazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="8046a-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="8046a-112">La rimozione, l'aggiunta o la riorganizzazione di segmenti di URL delle richieste in ingresso</span><span class="sxs-lookup"><span data-stu-id="8046a-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="8046a-113">L'ottimizzazione di un URL pubblico per Search Engine Optimization (SEO)</span><span class="sxs-lookup"><span data-stu-id="8046a-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="8046a-114">Permette l'utilizzo di un URL pubblico brevi per consentire agli utenti di prevedere il contenuto che individuano seguendo un collegamento</span><span class="sxs-lookup"><span data-stu-id="8046a-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="8046a-115">Reindirizzamento delle richieste non protette per la protezione di endpoint</span><span class="sxs-lookup"><span data-stu-id="8046a-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="8046a-116">Impedendo hotlinking immagine</span><span class="sxs-lookup"><span data-stu-id="8046a-116">Preventing image hotlinking</span></span>

<span data-ttu-id="8046a-117">È possibile definire regole per la modifica dell'URL in diversi modi, tra cui regex, le regole di modulo mod_rewrite Apache, le regole di modulo di IIS di riscrittura e l'utilizzo di logica della regola personalizzata.</span><span class="sxs-lookup"><span data-stu-id="8046a-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="8046a-118">Questo documento introduce la riscrittura URL con le istruzioni su come utilizzare il Middleware di riscrittura URL nelle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8046a-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="8046a-119">La riscrittura URL può ridurre le prestazioni di un'app.</span><span class="sxs-lookup"><span data-stu-id="8046a-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="8046a-120">Ove possibile, è necessario limitare il numero e la complessità delle regole.</span><span class="sxs-lookup"><span data-stu-id="8046a-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="8046a-121">Reindirizzamento dell'URL e l'URL di riscrittura</span><span class="sxs-lookup"><span data-stu-id="8046a-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="8046a-122">La differenza nella formulazione tra *reindirizzamento dell'URL* e *URL rewrite* potrebbe sembrare complesso in primo ma ha implicazioni molto importanti per le risorse necessarie per i client.</span><span class="sxs-lookup"><span data-stu-id="8046a-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="8046a-123">URL di riscrittura Middleware dell'ASP.NET di base è in grado di soddisfare la necessità di entrambi.</span><span class="sxs-lookup"><span data-stu-id="8046a-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="8046a-124">Oggetto *reindirizzamento dell'URL* è un'operazione lato client, in cui il client viene richiesto di accedere a una risorsa in un altro indirizzo.</span><span class="sxs-lookup"><span data-stu-id="8046a-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="8046a-125">Questa operazione richiede un round trip al server e l'URL di reindirizzamento restituito al client viene visualizzata nella barra degli indirizzi del browser quando il client effettua una nuova richiesta per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="8046a-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="8046a-126">Se `/resource` è *reindirizzato* a `/different-resource`, le richieste del client `/resource`, e il server risponde che il client deve ottenere la risorsa a `/different-resource` con un codice di stato che indica che il reindirizzamento è temporaneo o permanente.</span><span class="sxs-lookup"><span data-stu-id="8046a-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="8046a-127">Il client esegue una nuova richiesta per la risorsa dell'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="8046a-127">The client executes a new request for the resource at the redirect URL.</span></span>

![Un endpoint del servizio WebAPI è stato modificato temporaneamente dalla versione 1 (v1) alla versione 2 (v2) nel server.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="8046a-133">Quando si reindirizzano le richieste a un URL diverso, indicare se il reindirizzamento è temporaneo o permanente.</span><span class="sxs-lookup"><span data-stu-id="8046a-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="8046a-134">301 (spostato permanentemente) codice di stato viene utilizzato in cui la risorsa ha un nuovo URL permanente e si desidera per il client che tutte le richieste successive per la risorsa devono usare il nuovo URL.</span><span class="sxs-lookup"><span data-stu-id="8046a-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="8046a-135">*Il client può memorizzare nella cache la risposta quando viene ricevuto un codice di 301 stato.*</span><span class="sxs-lookup"><span data-stu-id="8046a-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="8046a-136">Il codice di stato 302 (trovato) viene utilizzato in cui il reindirizzamento è temporaneo o in genere l'oggetto da modificare, in modo che il client non deve archiviare e riutilizzare l'URL di reindirizzamento in futuro.</span><span class="sxs-lookup"><span data-stu-id="8046a-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="8046a-137">Per ulteriori informazioni, vedere [RFC 2616: le definizioni dei codici di stato](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="8046a-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="8046a-138">Oggetto *URL rewrite* è un'operazione lato server per fornire una risorsa da un indirizzo di risorse diverso.</span><span class="sxs-lookup"><span data-stu-id="8046a-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="8046a-139">La riscrittura URL non richiede un round trip al server.</span><span class="sxs-lookup"><span data-stu-id="8046a-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="8046a-140">L'URL riscritto non viene restituito al client e non sarà più visualizzato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="8046a-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="8046a-141">Quando `/resource` è *riscritto* a `/different-resource`, le richieste del client `/resource`e il server *internamente* recupera la risorsa a livello `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="8046a-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="8046a-142">Anche se il client potrebbe essere in grado di recuperare la risorsa dell'URL riscritto, il client non verrà indicato che la risorsa esista nell'URL riscritto quando si effettua la richiesta e riceve la risposta.</span><span class="sxs-lookup"><span data-stu-id="8046a-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Un endpoint del servizio WebAPI è stato modificato dalla versione 1 (v1) alla versione 2 (v2) nel server.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="8046a-147">App di esempio di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="8046a-147">URL rewriting sample app</span></span>
<span data-ttu-id="8046a-148">È possibile esplorare le funzionalità di Middleware di riscrittura URL con il [app di esempio di riscrittura URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="8046a-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="8046a-149">Riscrittura si applica l'app e regole di reindirizzamento e viene visualizzato l'URL riscritto o reindirizzato.</span><span class="sxs-lookup"><span data-stu-id="8046a-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="8046a-150">Quando utilizzare il Middleware di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="8046a-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="8046a-151">Utilizzare il Middleware di riscrittura URL quando non è possibile utilizzare il [modulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS in Windows Server, il [modulo mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) nel Server di Apache, [la riscrittura URL su Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), o l'app è ospitato in [server HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="8046a-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="8046a-152">Motivi principali per utilizzare il server tecnologie basate su URL riscrittura in IIS, Apache o Nginx sono che il middleware non supporta le funzionalità complete di questi moduli e le prestazioni del middleware probabilmente non corrisponderanno a quello dei moduli.</span><span class="sxs-lookup"><span data-stu-id="8046a-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="8046a-153">Esistono tuttavia alcune funzionalità dei moduli server che non funzionano con i progetti ASP.NET Core, ad esempio il `IsFile` e `IsDirectory` vincoli del modulo IIS di riscrittura.</span><span class="sxs-lookup"><span data-stu-id="8046a-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="8046a-154">In questi scenari, utilizzare il middleware.</span><span class="sxs-lookup"><span data-stu-id="8046a-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="8046a-155">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="8046a-155">Package</span></span>
<span data-ttu-id="8046a-156">Per includere il middleware nel progetto, aggiungere un riferimento di [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8046a-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="8046a-157">Questa funzionalità è disponibile per le app destinate a ASP.NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="8046a-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="8046a-158">Estensione e le opzioni</span><span class="sxs-lookup"><span data-stu-id="8046a-158">Extension and options</span></span>
<span data-ttu-id="8046a-159">Stabilire la riscrittura URL e le regole di reindirizzamento creando un'istanza del `RewriteOptions` classe con metodi di estensione per ognuna delle proprie regole.</span><span class="sxs-lookup"><span data-stu-id="8046a-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="8046a-160">Concatenare più regole nell'ordine in cui si desidera vengano elaborati.</span><span class="sxs-lookup"><span data-stu-id="8046a-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="8046a-161">Il `RewriteOptions` vengono passati nel Middleware di riscrittura URL quando viene aggiunto alla pipeline delle richieste con `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="8046a-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="8046a-164">Reindirizzamento dell'URL</span><span class="sxs-lookup"><span data-stu-id="8046a-164">URL redirect</span></span>
<span data-ttu-id="8046a-165">Utilizzare `AddRedirect` per reindirizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="8046a-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="8046a-166">Il primo parametro contiene l'espressione regolare per la corrispondenza nel percorso dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="8046a-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="8046a-167">Il secondo parametro è la stringa di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="8046a-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="8046a-168">Il terzo parametro, se presente, specifica il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="8046a-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="8046a-169">Se non si specifica il codice di stato, per impostazione predefinita in 302 (trovato), che indica che la risorsa è temporaneamente spostata o sostituita.</span><span class="sxs-lookup"><span data-stu-id="8046a-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="8046a-172">In un browser con gli strumenti di sviluppo abilitato, effettuare una richiesta per l'app di esempio con il percorso `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="8046a-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="8046a-173">L'espressione regolare corrisponde al percorso di richiesta in `redirect-rule/(.*)`, e il percorso viene sostituito con `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="8046a-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="8046a-174">Il reindirizzamento dell'URL viene inviato al client con codice di stato 302 (trovato).</span><span class="sxs-lookup"><span data-stu-id="8046a-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="8046a-175">Il browser invia una nuova richiesta all'URL di reindirizzamento, verrà visualizzato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="8046a-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="8046a-176">Poiché nessuna regola nell'app di esempio corrispondente all'URL di reindirizzamento, la seconda richiesta riceve una risposta di 200 (OK) dall'app e il corpo della risposta viene visualizzato l'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="8046a-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="8046a-177">Un round trip viene effettuato al server quando è un URL *reindirizzato*.</span><span class="sxs-lookup"><span data-stu-id="8046a-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="8046a-178">Prestare attenzione quando si stabiliscono le regole di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="8046a-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="8046a-179">Le regole di reindirizzamento vengono valutate in ogni richiesta per l'app, ad esempio dopo un reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="8046a-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="8046a-180">È facile per errore un ciclo di reindirizzamento infinito.</span><span class="sxs-lookup"><span data-stu-id="8046a-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="8046a-181">Richiesta originale:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="8046a-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="8046a-183">La parte dell'espressione racchiusa tra parentesi viene chiamata un *gruppo capture*.</span><span class="sxs-lookup"><span data-stu-id="8046a-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="8046a-184">Il punto (`.`) dell'espressione significa *corrisponde a qualsiasi carattere*.</span><span class="sxs-lookup"><span data-stu-id="8046a-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="8046a-185">L'asterisco (`*`) indica *corrisponde al carattere precedente zero o più volte*.</span><span class="sxs-lookup"><span data-stu-id="8046a-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="8046a-186">Di conseguenza, i segmenti di percorso ultime due dell'URL, `1234/5678`, vengono acquisite dal gruppo di acquisizione `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="8046a-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="8046a-187">Qualsiasi valore fornito nell'URL della richiesta dopo `redirect-rule/` viene acquisito da questo gruppo di acquisizione singola.</span><span class="sxs-lookup"><span data-stu-id="8046a-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="8046a-188">Nella stringa di sostituzione, i gruppi acquisiti vengono inseriti nella stringa con il segno di dollaro (`$`) seguito dal numero di sequenza dell'acquisizione.</span><span class="sxs-lookup"><span data-stu-id="8046a-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="8046a-189">Il valore del primo gruppo di acquisizione viene ottenuto con `$1`, il secondo con `$2`, continuando in sequenza per i gruppi di acquisizione all'espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="8046a-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="8046a-190">È solo un gruppo acquisito in reindirizzamento regola regex nell'app di esempio, viene visualizzato un solo gruppo inserito nella stringa di sostituzione, ovvero `$1`.</span><span class="sxs-lookup"><span data-stu-id="8046a-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="8046a-191">Quando viene applicata la regola, l'URL diventa `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="8046a-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="8046a-192">Reindirizzamento dell'URL per un endpoint protetto</span><span class="sxs-lookup"><span data-stu-id="8046a-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="8046a-193">Utilizzare `AddRedirectToHttps` per reindirizzare le richieste HTTP nello stesso host e lo stesso percorso utilizzando il protocollo HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="8046a-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="8046a-194">Se non viene fornito il codice di stato, il middleware predefinito 302 (trovato).</span><span class="sxs-lookup"><span data-stu-id="8046a-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="8046a-195">Se la porta non è specificata, il middleware per impostazione predefinita `null`, ovvero il protocollo viene modificato in `https://` e il client accede alla risorsa sulla porta 443.</span><span class="sxs-lookup"><span data-stu-id="8046a-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="8046a-196">Nell'esempio viene illustrato come impostare il codice di stato 301 (spostato permanentemente) e assegnare alla porta 5001.</span><span class="sxs-lookup"><span data-stu-id="8046a-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="8046a-197">Utilizzare `AddRedirectToHttpsPermanent` per reindirizzare le richieste non protette nello stesso host e lo stesso percorso con il protocollo HTTPS sicuro (`https://` sulla porta 443).</span><span class="sxs-lookup"><span data-stu-id="8046a-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="8046a-198">Il middleware imposta il codice di stato per 301 (spostato permanentemente).</span><span class="sxs-lookup"><span data-stu-id="8046a-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="8046a-199">L'app di esempio è in grado di illustrare come utilizzare `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="8046a-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="8046a-200">Aggiungere il metodo di estensione per il `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="8046a-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="8046a-201">Per l'app in qualsiasi URL, effettuare una richiesta non protetta.</span><span class="sxs-lookup"><span data-stu-id="8046a-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="8046a-202">Ignorare l'avviso che non è attendibile il certificato autofirmato di protezione del browser.</span><span class="sxs-lookup"><span data-stu-id="8046a-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="8046a-203">Richiesta originale utilizzando `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="8046a-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="8046a-205">Richiesta originale utilizzando `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="8046a-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="8046a-207">Riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="8046a-207">URL rewrite</span></span>
<span data-ttu-id="8046a-208">Utilizzare `AddRewrite` per creare una regola per la riscrittura URL.</span><span class="sxs-lookup"><span data-stu-id="8046a-208">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="8046a-209">Il primo parametro contiene l'espressione regolare per la corrispondenza sul percorso URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="8046a-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="8046a-210">Il secondo parametro è la stringa di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="8046a-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="8046a-211">Il terzo parametro, `skipRemainingRules: {true|false}`, indica se ignorare le regole di riscrittura aggiuntivo se viene applicata la regola corrente o meno per il middleware.</span><span class="sxs-lookup"><span data-stu-id="8046a-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="8046a-214">Richiesta originale:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="8046a-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Finestra del browser con gli strumenti di sviluppo di rilevamento di richiesta e risposta](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="8046a-216">La prima cosa notare nell'espressione regolare è l'accento circonflesso (`^`) all'inizio dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="8046a-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="8046a-217">Ciò significa che corrispondenza inizia all'inizio del percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="8046a-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="8046a-218">Nell'esempio precedente con la regola di reindirizzamento, `redirect-rule/(.*)`, non c'è alcun marcatore all'inizio dell'espressione regolare; pertanto, possono precedere i caratteri `redirect-rule/` in un percorso di una corrispondenza corretta.</span><span class="sxs-lookup"><span data-stu-id="8046a-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="8046a-219">Path</span><span class="sxs-lookup"><span data-stu-id="8046a-219">Path</span></span>                               | <span data-ttu-id="8046a-220">Corrispondenza con</span><span class="sxs-lookup"><span data-stu-id="8046a-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="8046a-221">Yes</span><span class="sxs-lookup"><span data-stu-id="8046a-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="8046a-222">Sì</span><span class="sxs-lookup"><span data-stu-id="8046a-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="8046a-223">Yes</span><span class="sxs-lookup"><span data-stu-id="8046a-223">Yes</span></span>   |

<span data-ttu-id="8046a-224">La regola di riscrittura `^rewrite-rule/(\d+)/(\d+)`, corrisponde solo percorsi se iniziano con `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="8046a-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="8046a-225">Si noti la differenza nella corrispondenza tra la seguente regola di riscrittura e la relativa regola precedente.</span><span class="sxs-lookup"><span data-stu-id="8046a-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="8046a-226">Path</span><span class="sxs-lookup"><span data-stu-id="8046a-226">Path</span></span>                              | <span data-ttu-id="8046a-227">Corrispondenza con</span><span class="sxs-lookup"><span data-stu-id="8046a-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="8046a-228">Yes</span><span class="sxs-lookup"><span data-stu-id="8046a-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="8046a-229">No</span><span class="sxs-lookup"><span data-stu-id="8046a-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="8046a-230">No</span><span class="sxs-lookup"><span data-stu-id="8046a-230">No</span></span>    |

<span data-ttu-id="8046a-231">Dopo il `^rewrite-rule/` parte dell'espressione, sono presenti due gruppi di acquisizione, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="8046a-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="8046a-232">Il `\d` indica *corrisponde a una cifra (numero)*.</span><span class="sxs-lookup"><span data-stu-id="8046a-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="8046a-233">Il segno più (`+`) significa *corrisponde a uno o più il carattere precedente*.</span><span class="sxs-lookup"><span data-stu-id="8046a-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="8046a-234">Pertanto, l'URL deve contenere un numero seguito da una barra seguita da un altro numero.</span><span class="sxs-lookup"><span data-stu-id="8046a-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="8046a-235">Questi gruppi vengono inseriti nell'URL riscritto come di acquisizione `$1` e `$2`.</span><span class="sxs-lookup"><span data-stu-id="8046a-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="8046a-236">La stringa di sostituzione regola di riscrittura inserisce i gruppi acquisiti nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="8046a-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="8046a-237">Il percorso della richiesto `/rewrite-rule/1234/5678` viene riscritto per ottenere la risorsa a livello `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="8046a-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="8046a-238">Se una stringa di query è presente nella richiesta originale, questi vengono mantenuti quando l'URL è stato riscritto.</span><span class="sxs-lookup"><span data-stu-id="8046a-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="8046a-239">Non vi è alcun round trip al server per ottenere la risorsa.</span><span class="sxs-lookup"><span data-stu-id="8046a-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="8046a-240">Se la risorsa esiste, dispone di recuperata e restituita al client con un codice di 200 stato (OK).</span><span class="sxs-lookup"><span data-stu-id="8046a-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="8046a-241">Poiché il client non è stato reindirizzato, non modifica l'URL nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="8046a-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="8046a-242">Per quanto riguarda il client, l'operazione di riscrittura URL non si è verificato.</span><span class="sxs-lookup"><span data-stu-id="8046a-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="8046a-243">Utilizzare `skipRemainingRules: true` laddove possibile, perché le regole di corrispondenza sono un processo dispendioso e riduce il tempo di risposta dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8046a-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="8046a-244">Per la risposta più veloce di app:</span><span class="sxs-lookup"><span data-stu-id="8046a-244">For the fastest app response:</span></span>
> * <span data-ttu-id="8046a-245">Ordinamento delle regole di riscrittura dalla regola di corrispondenza più di frequente per la regola corrispondente meno frequentemente.</span><span class="sxs-lookup"><span data-stu-id="8046a-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="8046a-246">Quando si verifica una corrispondenza ed non è necessaria alcuna elaborazione regola aggiuntiva, ignorare l'elaborazione delle regole rimanenti.</span><span class="sxs-lookup"><span data-stu-id="8046a-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="8046a-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="8046a-247">Apache mod_rewrite</span></span>
<span data-ttu-id="8046a-248">Applicare le regole di Apache mod_rewrite con `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="8046a-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="8046a-249">Assicurarsi che il file delle regole viene distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="8046a-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="8046a-250">Per ulteriori informazioni ed esempi di regole mod_rewrite, vedere [mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="8046a-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8046a-252">A `StreamReader` viene utilizzato per leggere le regole di *ApacheModRewrite.txt* file delle regole.</span><span class="sxs-lookup"><span data-stu-id="8046a-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8046a-254">Il primo parametro accetta un `IFileProvider`, che viene fornito tramite [Dependency Injection](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="8046a-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="8046a-255">Il `IHostingEnvironment` viene inserito per fornire il `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="8046a-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="8046a-256">Il secondo parametro è il percorso al file di regole, ovvero *ApacheModRewrite.txt* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="8046a-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="8046a-257">L'app di esempio reindirizza le richieste da `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="8046a-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="8046a-258">Il codice di stato della risposta è 302 (trovato).</span><span class="sxs-lookup"><span data-stu-id="8046a-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="8046a-259">Richiesta originale:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="8046a-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="8046a-261">Variabili server supportati</span><span class="sxs-lookup"><span data-stu-id="8046a-261">Supported server variables</span></span>
<span data-ttu-id="8046a-262">Il middleware supporta variabili Apache mod_rewrite server seguenti:</span><span class="sxs-lookup"><span data-stu-id="8046a-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="8046a-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8046a-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="8046a-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="8046a-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="8046a-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="8046a-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="8046a-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="8046a-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="8046a-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="8046a-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="8046a-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="8046a-268">HTTP_HOST</span></span>
* <span data-ttu-id="8046a-269">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="8046a-269">HTTP_REFERER</span></span>
* <span data-ttu-id="8046a-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="8046a-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="8046a-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8046a-271">HTTPS</span></span>
* <span data-ttu-id="8046a-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="8046a-272">IPV6</span></span>
* <span data-ttu-id="8046a-273">STRINGA_QUERY</span><span class="sxs-lookup"><span data-stu-id="8046a-273">QUERY_STRING</span></span>
* <span data-ttu-id="8046a-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8046a-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="8046a-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="8046a-275">REMOTE_PORT</span></span>
* <span data-ttu-id="8046a-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8046a-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="8046a-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="8046a-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="8046a-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="8046a-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="8046a-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="8046a-279">REQUEST_URI</span></span>
* <span data-ttu-id="8046a-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8046a-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="8046a-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="8046a-281">SERVER_ADDR</span></span>
* <span data-ttu-id="8046a-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="8046a-282">SERVER_PORT</span></span>
* <span data-ttu-id="8046a-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="8046a-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="8046a-284">ORA</span><span class="sxs-lookup"><span data-stu-id="8046a-284">TIME</span></span>
* <span data-ttu-id="8046a-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="8046a-285">TIME_DAY</span></span>
* <span data-ttu-id="8046a-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="8046a-286">TIME_HOUR</span></span>
* <span data-ttu-id="8046a-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="8046a-287">TIME_MIN</span></span>
* <span data-ttu-id="8046a-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="8046a-288">TIME_MON</span></span>
* <span data-ttu-id="8046a-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="8046a-289">TIME_SEC</span></span>
* <span data-ttu-id="8046a-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="8046a-290">TIME_WDAY</span></span>
* <span data-ttu-id="8046a-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="8046a-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="8046a-292">Regole di IIS URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="8046a-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="8046a-293">Per utilizzare le regole che si applicano a IIS URL Rewrite Module, utilizzare `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="8046a-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="8046a-294">Assicurarsi che il file delle regole viene distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="8046a-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="8046a-295">Non indirizzare il middleware di utilizzare il *Web. config* file quando è in esecuzione in Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="8046a-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="8046a-296">Con IIS, queste regole devono essere archiviate di fuori del *Web. config* per evitare conflitti con il modulo IIS di riscrittura.</span><span class="sxs-lookup"><span data-stu-id="8046a-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="8046a-297">Per ulteriori informazioni ed esempi di regole di IIS URL Rewrite Module, vedere [utilizzando Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [URL riscrivere configurazione riferimento al modulo](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="8046a-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8046a-299">A `StreamReader` viene utilizzato per leggere le regole di *IISUrlRewrite.xml* file delle regole.</span><span class="sxs-lookup"><span data-stu-id="8046a-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-300">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8046a-301">Il primo parametro accetta un `IFileProvider`, mentre il secondo parametro è il percorso al file di regole XML, ovvero *IISUrlRewrite.xml* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="8046a-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="8046a-302">L'app di esempio riscrive le richieste provenienti da `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="8046a-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="8046a-303">La risposta viene inviata al client con un codice di stato 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="8046a-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="8046a-304">Richiesta originale:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="8046a-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Finestra del browser con gli strumenti di sviluppo di rilevamento di richiesta e risposta](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="8046a-306">Se si dispone di un modulo di riscrivere IIS attivo con le regole a livello di server configurate che avrebbe sulle app in modi indesiderati, è possibile disabilitare il modulo IIS di riscrittura per un'app.</span><span class="sxs-lookup"><span data-stu-id="8046a-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="8046a-307">Per ulteriori informazioni, vedere [moduli IIS disabilitazione](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="8046a-307">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="8046a-308">Funzionalità non supportate</span><span class="sxs-lookup"><span data-stu-id="8046a-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8046a-310">Il middleware rilasciato con ASP.NET Core 2. x non supporta le seguenti funzionalità di IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="8046a-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="8046a-311">Regole in uscita</span><span class="sxs-lookup"><span data-stu-id="8046a-311">Outbound Rules</span></span>
* <span data-ttu-id="8046a-312">Variabili Server personalizzato</span><span class="sxs-lookup"><span data-stu-id="8046a-312">Custom Server Variables</span></span>
* <span data-ttu-id="8046a-313">Caratteri jolly</span><span class="sxs-lookup"><span data-stu-id="8046a-313">Wildcards</span></span>
* <span data-ttu-id="8046a-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="8046a-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8046a-316">Il middleware rilasciato con ASP.NET Core 1. x non supporta le seguenti funzionalità di IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="8046a-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="8046a-317">Regole globali</span><span class="sxs-lookup"><span data-stu-id="8046a-317">Global Rules</span></span>
* <span data-ttu-id="8046a-318">Regole in uscita</span><span class="sxs-lookup"><span data-stu-id="8046a-318">Outbound Rules</span></span>
* <span data-ttu-id="8046a-319">Mappe di riscrittura.</span><span class="sxs-lookup"><span data-stu-id="8046a-319">Rewrite Maps</span></span>
* <span data-ttu-id="8046a-320">Azione CustomResponse</span><span class="sxs-lookup"><span data-stu-id="8046a-320">CustomResponse Action</span></span>
* <span data-ttu-id="8046a-321">Variabili Server personalizzato</span><span class="sxs-lookup"><span data-stu-id="8046a-321">Custom Server Variables</span></span>
* <span data-ttu-id="8046a-322">Caratteri jolly</span><span class="sxs-lookup"><span data-stu-id="8046a-322">Wildcards</span></span>
* <span data-ttu-id="8046a-323">Azione: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="8046a-323">Action:CustomResponse</span></span>
* <span data-ttu-id="8046a-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="8046a-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="8046a-325">Variabili server supportati</span><span class="sxs-lookup"><span data-stu-id="8046a-325">Supported server variables</span></span>
<span data-ttu-id="8046a-326">Il middleware supporta le seguenti variabili server IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="8046a-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="8046a-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="8046a-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="8046a-328">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="8046a-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="8046a-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="8046a-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="8046a-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="8046a-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="8046a-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="8046a-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="8046a-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="8046a-332">HTTP_HOST</span></span>
* <span data-ttu-id="8046a-333">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="8046a-333">HTTP_REFERER</span></span>
* <span data-ttu-id="8046a-334">URL_HTTP</span><span class="sxs-lookup"><span data-stu-id="8046a-334">HTTP_URL</span></span>
* <span data-ttu-id="8046a-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="8046a-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="8046a-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8046a-336">HTTPS</span></span>
* <span data-ttu-id="8046a-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="8046a-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="8046a-338">STRINGA_QUERY</span><span class="sxs-lookup"><span data-stu-id="8046a-338">QUERY_STRING</span></span>
* <span data-ttu-id="8046a-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="8046a-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="8046a-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="8046a-340">REMOTE_PORT</span></span>
* <span data-ttu-id="8046a-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="8046a-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="8046a-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="8046a-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="8046a-343">È anche possibile ottenere un `IFileProvider` tramite un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="8046a-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="8046a-344">Questo approccio può fornire una maggiore flessibilità per la posizione della riscrittura file delle regole.</span><span class="sxs-lookup"><span data-stu-id="8046a-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="8046a-345">Assicurarsi che i file di regole di riscrittura vengono distribuiti nel server in corrispondenza del percorso che è fornire.</span><span class="sxs-lookup"><span data-stu-id="8046a-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="8046a-346">Regole basate su metodo</span><span class="sxs-lookup"><span data-stu-id="8046a-346">Method-based rule</span></span>
<span data-ttu-id="8046a-347">Utilizzare `Add(Action<RewriteContext> applyRule)` per implementare la propria logica della regola in un metodo.</span><span class="sxs-lookup"><span data-stu-id="8046a-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="8046a-348">Il `RewriteContext` espone il `HttpContext` per l'utilizzo del metodo.</span><span class="sxs-lookup"><span data-stu-id="8046a-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="8046a-349">Il `context.Result` determina le modalità pipeline l'elaborazione viene gestita.</span><span class="sxs-lookup"><span data-stu-id="8046a-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="8046a-350">contesto. Risultato</span><span class="sxs-lookup"><span data-stu-id="8046a-350">context.Result</span></span>                       | <span data-ttu-id="8046a-351">Operazione</span><span class="sxs-lookup"><span data-stu-id="8046a-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="8046a-352">`RuleResult.ContinueRules` (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="8046a-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="8046a-353">Continuare ad applicare le regole</span><span class="sxs-lookup"><span data-stu-id="8046a-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="8046a-354">Arrestare l'applicazione di regole e inviare la risposta</span><span class="sxs-lookup"><span data-stu-id="8046a-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="8046a-355">Arrestare l'applicazione di regole e inviare il contesto al middleware successivo</span><span class="sxs-lookup"><span data-stu-id="8046a-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-356">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="8046a-358">L'app di esempio viene illustrato un metodo che reindirizza le richieste per i percorsi che terminano con *XML*.</span><span class="sxs-lookup"><span data-stu-id="8046a-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="8046a-359">Se si effettua una richiesta `/file.xml`, viene reindirizzata a `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="8046a-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="8046a-360">Il codice di stato è impostato su 301 (spostato permanentemente).</span><span class="sxs-lookup"><span data-stu-id="8046a-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="8046a-361">Per un tipo di reindirizzamento, è necessario impostare in modo esplicito il codice di stato della risposta; in caso contrario, viene restituito un codice di stato 200 (OK) e non verrà eseguito il reindirizzamento sul client.</span><span class="sxs-lookup"><span data-stu-id="8046a-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-362">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="8046a-364">Richiesta originale:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="8046a-364">Original Request: `/file.xml`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte relative a file. Xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="8046a-366">Regola di IRule</span><span class="sxs-lookup"><span data-stu-id="8046a-366">IRule-based rule</span></span>
<span data-ttu-id="8046a-367">Utilizzare `Add(IRule)` per implementare la propria logica della regola in una classe che deriva da `IRule`.</span><span class="sxs-lookup"><span data-stu-id="8046a-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="8046a-368">Utilizzando un `IRule` offre una maggiore flessibilità rispetto all'uso dell'approccio di regole basate su metodo.</span><span class="sxs-lookup"><span data-stu-id="8046a-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="8046a-369">La classe derivata può includere un costruttore, in cui è possibile passare parametri per il `ApplyRule` metodo.</span><span class="sxs-lookup"><span data-stu-id="8046a-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="8046a-372">I valori dei parametri nell'applicazione di esempio per il `extension` e `newPath` vengono controllati per soddisfare diverse condizioni.</span><span class="sxs-lookup"><span data-stu-id="8046a-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="8046a-373">Il `extension` deve contenere un valore, e il valore deve essere *PNG*, *jpg*, o *GIF*.</span><span class="sxs-lookup"><span data-stu-id="8046a-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="8046a-374">Se il `newPath` non è valido, un `ArgumentException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="8046a-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="8046a-375">Se si effettua una richiesta *image.png*, viene reindirizzata a `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="8046a-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="8046a-376">Se si effettua una richiesta *image.jpg*, viene reindirizzata a `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="8046a-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="8046a-377">Il codice di stato è impostato su 301 (spostato permanentemente) e `context.Result` è impostata per arrestare le regole di elaborazione e invio della risposta.</span><span class="sxs-lookup"><span data-stu-id="8046a-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8046a-378">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8046a-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8046a-379">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8046a-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="8046a-380">Richiesta originale:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="8046a-380">Original Request: `/image.png`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte per image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="8046a-382">Richiesta originale:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="8046a-382">Original Request: `/image.jpg`</span></span>

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte per image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="8046a-384">Esempi di espressioni regolari</span><span class="sxs-lookup"><span data-stu-id="8046a-384">Regex examples</span></span>

| <span data-ttu-id="8046a-385">Obiettivo</span><span class="sxs-lookup"><span data-stu-id="8046a-385">Goal</span></span> | <span data-ttu-id="8046a-386">Stringa Regex &</span><span class="sxs-lookup"><span data-stu-id="8046a-386">Regex String &</span></span><br><span data-ttu-id="8046a-387">Esempio di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="8046a-387">Match Example</span></span> | <span data-ttu-id="8046a-388">Stringa di sostituzione &</span><span class="sxs-lookup"><span data-stu-id="8046a-388">Replacement String &</span></span><br><span data-ttu-id="8046a-389">Esempio di output</span><span class="sxs-lookup"><span data-stu-id="8046a-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="8046a-390">Percorso di riscrittura nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="8046a-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="8046a-391">Rimuovere una barra finale</span><span class="sxs-lookup"><span data-stu-id="8046a-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="8046a-392">Applicare la barra finale</span><span class="sxs-lookup"><span data-stu-id="8046a-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="8046a-393">Evitare la riscrittura di richieste specifiche</span><span class="sxs-lookup"><span data-stu-id="8046a-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="8046a-394">Sì:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="8046a-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="8046a-395">No:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="8046a-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="8046a-396">Ridisporre i segmenti di URL</span><span class="sxs-lookup"><span data-stu-id="8046a-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="8046a-397">Sostituire un segmento di URL</span><span class="sxs-lookup"><span data-stu-id="8046a-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="8046a-398">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8046a-398">Additional resources</span></span>
* [<span data-ttu-id="8046a-399">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="8046a-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="8046a-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="8046a-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="8046a-401">Espressioni regolari in .NET</span><span class="sxs-lookup"><span data-stu-id="8046a-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="8046a-402">Linguaggio di espressioni regolari - Riferimento rapido</span><span class="sxs-lookup"><span data-stu-id="8046a-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="8046a-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="8046a-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="8046a-404">Using Url Rewrite Module 2.0 (IIS)</span><span class="sxs-lookup"><span data-stu-id="8046a-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="8046a-405">Riferimento di configurazione modulo riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="8046a-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="8046a-406">Forum IIS URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="8046a-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="8046a-407">Mantenere una struttura semplice di URL</span><span class="sxs-lookup"><span data-stu-id="8046a-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="8046a-408">La riscrittura URL 10 suggerimenti e consigli</span><span class="sxs-lookup"><span data-stu-id="8046a-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="8046a-409">Non a barre o barre</span><span class="sxs-lookup"><span data-stu-id="8046a-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
