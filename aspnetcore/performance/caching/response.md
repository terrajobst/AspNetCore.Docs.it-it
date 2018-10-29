---
title: La memorizzazione nella cache di risposta in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare la memorizzazione nella cache delle risposte per ridurre i requisiti di larghezza di banda e migliorare le prestazioni delle app ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207368"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="6069a-103">La memorizzazione nella cache di risposta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6069a-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="6069a-104">Dal [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6069a-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="6069a-105">La memorizzazione nella cache di risposta in Razor Pages è disponibile in ASP.NET Core 2.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6069a-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="6069a-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6069a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6069a-107">Risposta di memorizzazione nella cache riduce il numero di richieste di che un client o un proxy effettua a un server web.</span><span class="sxs-lookup"><span data-stu-id="6069a-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="6069a-108">Risposta di memorizzazione nella cache riduce anche la quantità di lavoro esegue il server web per generare una risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="6069a-109">La memorizzazione nella cache di risposta viene controllato dalle intestazioni che specificano la modalità client, proxy e middleware per memorizzare le risposte.</span><span class="sxs-lookup"><span data-stu-id="6069a-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="6069a-110">Il server web può memorizzare nella cache le risposte quando si aggiungono [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="6069a-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="6069a-111">La memorizzazione nella cache di risposta basato su HTTP</span><span class="sxs-lookup"><span data-stu-id="6069a-111">HTTP-based response caching</span></span>

<span data-ttu-id="6069a-112">Il [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234) descrive il comportamento delle cache di Internet.</span><span class="sxs-lookup"><span data-stu-id="6069a-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="6069a-113">L'intestazione HTTP principale usato per la memorizzazione nella cache [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), che consente di specificare la cache *direttive*.</span><span class="sxs-lookup"><span data-stu-id="6069a-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="6069a-114">Le direttive controllano il comportamento di memorizzazione nella cache come richieste arrivino dai client ai server e come risposte arrivino dal server ai client.</span><span class="sxs-lookup"><span data-stu-id="6069a-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="6069a-115">Richieste e risposte spostano tra i server proxy e server proxy devono inoltre essere conforme alla specifica la memorizzazione nella cache di HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="6069a-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="6069a-116">Common `Cache-Control` direttive vengono visualizzate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="6069a-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="6069a-117">Direttiva</span><span class="sxs-lookup"><span data-stu-id="6069a-117">Directive</span></span>                                                       | <span data-ttu-id="6069a-118">Operazione</span><span class="sxs-lookup"><span data-stu-id="6069a-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="6069a-119">public</span><span class="sxs-lookup"><span data-stu-id="6069a-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="6069a-120">Una cache può archiviare la risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="6069a-121">private</span><span class="sxs-lookup"><span data-stu-id="6069a-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="6069a-122">La risposta non deve essere archiviata da una cache condivisa.</span><span class="sxs-lookup"><span data-stu-id="6069a-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="6069a-123">Una cache privata può archiviare e riutilizzare la risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="6069a-124">max-age</span><span class="sxs-lookup"><span data-stu-id="6069a-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="6069a-125">Il client non accetta una risposta cui età è maggiore del numero specificato di secondi.</span><span class="sxs-lookup"><span data-stu-id="6069a-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="6069a-126">Esempi: `max-age=60` (60 secondi), `max-age=2592000` (mese)</span><span class="sxs-lookup"><span data-stu-id="6069a-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="6069a-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="6069a-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="6069a-128">**Nelle richieste**: una cache non deve utilizzare una risposta memorizzata per soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6069a-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="6069a-129">Nota: Il server di origine genera nuovamente la risposta per il client e il middleware Aggiorna la risposta memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6069a-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="6069a-130">**Nelle risposte**: la risposta non deve essere usata per una richiesta successiva senza convalida nel server di origine.</span><span class="sxs-lookup"><span data-stu-id="6069a-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="6069a-131">no-store</span><span class="sxs-lookup"><span data-stu-id="6069a-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="6069a-132">**Nelle richieste**: una cache non deve memorizzare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6069a-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="6069a-133">**Nelle risposte**: una cache non deve memorizzare qualsiasi parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="6069a-134">Nella tabella seguente vengono visualizzate altre intestazioni di cache che svolgono un ruolo di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="6069a-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="6069a-135">Intestazione</span><span class="sxs-lookup"><span data-stu-id="6069a-135">Header</span></span>                                                     | <span data-ttu-id="6069a-136">Funzione</span><span class="sxs-lookup"><span data-stu-id="6069a-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="6069a-137">Età</span><span class="sxs-lookup"><span data-stu-id="6069a-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="6069a-138">Stima della quantità di tempo in secondi, poiché la risposta è stata generata o convalidata nel server di origine.</span><span class="sxs-lookup"><span data-stu-id="6069a-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="6069a-139">Alla scadenza</span><span class="sxs-lookup"><span data-stu-id="6069a-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="6069a-140">La data/ora dopo il quale la risposta viene considerata non aggiornata.</span><span class="sxs-lookup"><span data-stu-id="6069a-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="6069a-141">(Pragma)</span><span class="sxs-lookup"><span data-stu-id="6069a-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="6069a-142">Per le versioni precedenti la compatibilità con HTTP/1.0 vengono memorizzati nella cache per l'impostazione esiste `no-cache` comportamento.</span><span class="sxs-lookup"><span data-stu-id="6069a-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="6069a-143">Se il `Cache-Control` intestazione è presente, il `Pragma` intestazione viene ignorata.</span><span class="sxs-lookup"><span data-stu-id="6069a-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="6069a-144">Variare</span><span class="sxs-lookup"><span data-stu-id="6069a-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="6069a-145">Specifica che una risposta memorizzata nella cache non deve essere inviata a meno che non tutti del `Vary` corrispondano i campi di intestazione nella richiesta originale della risposta memorizzata nella cache sia la nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="6069a-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="6069a-146">Gli aspetti di memorizzazione nella cache basata su HTTP richiesta delle direttive Cache-Control</span><span class="sxs-lookup"><span data-stu-id="6069a-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="6069a-147">Il [specifica la memorizzazione nella cache di HTTP 1.1 per l'intestazione Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) richiede una cache a rispettare un valido `Cache-Control` intestazione inviato dal client.</span><span class="sxs-lookup"><span data-stu-id="6069a-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="6069a-148">Un client può eseguire richieste con un `no-cache` valore dell'intestazione e forza il server a generare una nuova risposta per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="6069a-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="6069a-149">Rispetta sempre client `Cache-Control` intestazioni della richiesta ha senso se si prende in considerazione l'obiettivo della memorizzazione nella cache HTTP.</span><span class="sxs-lookup"><span data-stu-id="6069a-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="6069a-150">Con la specifica ufficiale, la memorizzazione nella cache è progettata per ridurre il sovraccarico di latenza e la rete di soddisfare le richieste attraverso una rete di client, proxy e server.</span><span class="sxs-lookup"><span data-stu-id="6069a-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="6069a-151">Non è necessariamente un modo per controllare il carico su un server di origine.</span><span class="sxs-lookup"><span data-stu-id="6069a-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="6069a-152">Non vi è alcun controllo per gli sviluppatori corrente su questo comportamento di memorizzazione nella cache quando si usa la [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware) poiché il middleware è conforme a ufficiale la memorizzazione nella cache specifica.</span><span class="sxs-lookup"><span data-stu-id="6069a-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="6069a-153">[I miglioramenti futuri al middleware](https://github.com/aspnet/ResponseCaching/issues/96) permetterà di configurare il middleware per ignorare una richiesta `Cache-Control` intestazione quando si decide di gestire una risposta memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="6069a-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="6069a-154">Ciò offrirà un'opportunità per controllare meglio il carico sul server quando si usa il middleware.</span><span class="sxs-lookup"><span data-stu-id="6069a-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="6069a-155">Altre tecnologie di memorizzazione nella cache in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6069a-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="6069a-156">La memorizzazione nella cache in memoria</span><span class="sxs-lookup"><span data-stu-id="6069a-156">In-memory caching</span></span>

<span data-ttu-id="6069a-157">La memorizzazione nella cache in memoria utilizza la memoria del server per archiviare i dati memorizzati nella cache.</span><span class="sxs-lookup"><span data-stu-id="6069a-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="6069a-158">Questo tipo di memorizzazione nella cache è adatto per uno o più server utilizzando *sessioni permanenti*.</span><span class="sxs-lookup"><span data-stu-id="6069a-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="6069a-159">Le sessioni permanenti significa che le richieste da un client vengano sempre indirizzate allo stesso server per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="6069a-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="6069a-160">Per altre informazioni, vedere [memorizza nella Cache in memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="6069a-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="6069a-161">Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="6069a-161">Distributed Cache</span></span>

<span data-ttu-id="6069a-162">Usare una cache distribuita per archiviare i dati in memoria quando l'app è ospitata in una farm di server o cloud.</span><span class="sxs-lookup"><span data-stu-id="6069a-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="6069a-163">La cache viene condivisa tra i server di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="6069a-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="6069a-164">Un client può inviare una richiesta che viene gestita da qualsiasi server nel gruppo, se i dati memorizzati nella cache per il client sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="6069a-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="6069a-165">ASP.NET Core offre SQL Server e le cache distribuite Redis.</span><span class="sxs-lookup"><span data-stu-id="6069a-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="6069a-166">Per altre informazioni, vedere <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="6069a-166">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="6069a-167">Helper Tag di cache</span><span class="sxs-lookup"><span data-stu-id="6069a-167">Cache Tag Helper</span></span>

<span data-ttu-id="6069a-168">È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o una pagina Razor con l'Helper Tag di Cache.</span><span class="sxs-lookup"><span data-stu-id="6069a-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="6069a-169">L'Helper Tag di Cache Usa la memorizzazione nella cache in memoria per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="6069a-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="6069a-170">Per altre informazioni, vedere [Helper Tag di Cache in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="6069a-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="6069a-171">Helper tag di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="6069a-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="6069a-172">È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o una pagina Razor in scenari web farm o cloud distribuite con l'Helper Tag di Cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="6069a-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="6069a-173">L'Helper Tag di Cache distribuita Usa Redis o SQL Server per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="6069a-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="6069a-174">Per altre informazioni, vedere [Helper Tag di Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="6069a-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="6069a-175">Attributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="6069a-175">ResponseCache attribute</span></span>

<span data-ttu-id="6069a-176">Il [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="6069a-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="6069a-177">Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni relative ai client autenticati.</span><span class="sxs-lookup"><span data-stu-id="6069a-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="6069a-178">La memorizzazione nella cache deve essere abilitata solo per contenuto che non cambia in base a identità di un utente o se un utente è connesso.</span><span class="sxs-lookup"><span data-stu-id="6069a-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="6069a-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varia in base alla risposta stored dai valori di elenco specificato di chiavi di query.</span><span class="sxs-lookup"><span data-stu-id="6069a-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="6069a-180">Quando un singolo valore di `*` è specificato, il middleware varia le risposte da tutti i parametri della stringa di query di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6069a-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="6069a-181">`VaryByQueryKeys` richiede ASP.NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6069a-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="6069a-182">È necessario abilitare il Middleware di memorizzazione nella cache della risposta per impostare il `VaryByQueryKeys` proprietà; in caso contrario, viene generata un'eccezione di runtime.</span><span class="sxs-lookup"><span data-stu-id="6069a-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="6069a-183">Non c'è un'intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6069a-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="6069a-184">La proprietà è una funzionalità HTTP gestita dal Middleware di memorizzazione nella cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="6069a-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="6069a-185">Per il middleware servire una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente.</span><span class="sxs-lookup"><span data-stu-id="6069a-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="6069a-186">Si consideri, ad esempio, la sequenza di richieste e i risultati mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="6069a-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="6069a-187">Richiesta</span><span class="sxs-lookup"><span data-stu-id="6069a-187">Request</span></span>                          | <span data-ttu-id="6069a-188">Risultato</span><span class="sxs-lookup"><span data-stu-id="6069a-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="6069a-189">Restituito dal server</span><span class="sxs-lookup"><span data-stu-id="6069a-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="6069a-190">Restituito dal middleware</span><span class="sxs-lookup"><span data-stu-id="6069a-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="6069a-191">Restituito dal server</span><span class="sxs-lookup"><span data-stu-id="6069a-191">Returned from server</span></span>     |

<span data-ttu-id="6069a-192">La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware.</span><span class="sxs-lookup"><span data-stu-id="6069a-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="6069a-193">La seconda richiesta viene restituita dal middleware in quanto la stringa di query corrisponde alla richiesta precedente.</span><span class="sxs-lookup"><span data-stu-id="6069a-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="6069a-194">La terza richiesta non è nella cache del middleware in quanto il valore di stringa di query non corrisponde una richiesta precedente.</span><span class="sxs-lookup"><span data-stu-id="6069a-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="6069a-195">Il `ResponseCacheAttribute` viene usato per configurare e creare (tramite `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="6069a-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="6069a-196">Il `ResponseCacheFilter` esegua l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="6069a-197">Il filtro:</span><span class="sxs-lookup"><span data-stu-id="6069a-197">The filter:</span></span>

* <span data-ttu-id="6069a-198">Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="6069a-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="6069a-199">Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="6069a-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="6069a-200">Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.</span><span class="sxs-lookup"><span data-stu-id="6069a-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="6069a-201">Variare</span><span class="sxs-lookup"><span data-stu-id="6069a-201">Vary</span></span>

<span data-ttu-id="6069a-202">Questa intestazione viene scritto solo quando il `VaryByHeader` è impostata.</span><span class="sxs-lookup"><span data-stu-id="6069a-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="6069a-203">È impostato sul `Vary` valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="6069a-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="6069a-204">L'esempio seguente usa il `VaryByHeader` proprietà:</span><span class="sxs-lookup"><span data-stu-id="6069a-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="6069a-205">È possibile visualizzare le intestazioni della risposta con gli strumenti di rete del browser.</span><span class="sxs-lookup"><span data-stu-id="6069a-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="6069a-206">L'immagine seguente mostra il F12 Edge in uscita nel **Network** scheda quando il `About2` viene aggiornato il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="6069a-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Bordo F12 output nella scheda rete quando viene chiamato il metodo di azione About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="6069a-208">NoStore e Location.None</span><span class="sxs-lookup"><span data-stu-id="6069a-208">NoStore and Location.None</span></span>

<span data-ttu-id="6069a-209">`NoStore` esegue l'override la maggior parte delle altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="6069a-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="6069a-210">Quando questa proprietà è impostata su `true`, il `Cache-Control` intestazione è impostata su `no-store`.</span><span class="sxs-lookup"><span data-stu-id="6069a-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="6069a-211">Se `Location` è impostata su `None`:</span><span class="sxs-lookup"><span data-stu-id="6069a-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="6069a-212">`Cache-Control` è impostato su `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="6069a-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="6069a-213">`Pragma` è impostato su `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="6069a-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="6069a-214">Se `NoStore` viene `false` e `Location` viene `None`, `Cache-Control` e `Pragma` sono impostati su `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="6069a-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="6069a-215">In genere impostata `NoStore` a `true` nelle pagine di errore.</span><span class="sxs-lookup"><span data-stu-id="6069a-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="6069a-216">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6069a-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="6069a-217">Ciò comporta le intestazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6069a-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="6069a-218">Percorso e la durata</span><span class="sxs-lookup"><span data-stu-id="6069a-218">Location and Duration</span></span>

<span data-ttu-id="6069a-219">Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (predefinito) o `Client`.</span><span class="sxs-lookup"><span data-stu-id="6069a-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="6069a-220">In questo caso, il `Cache-Control` intestazione è impostata sul valore di percorso aggiungendo la `max-age` della risposta.</span><span class="sxs-lookup"><span data-stu-id="6069a-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="6069a-221">`Location`di opzioni di `Any` e `Client` tradurre in `Cache-Control` valori di intestazione `public` e `private`rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="6069a-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="6069a-222">Come indicato in precedenza, l'impostazione `Location` al `None` imposta entrambe `Cache-Control` e `Pragma` intestazioni `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="6069a-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="6069a-223">Di seguito è riportato un esempio che mostra le intestazioni di prodotto impostando `Duration` e lasciare il valore predefinito `Location` valore:</span><span class="sxs-lookup"><span data-stu-id="6069a-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="6069a-224">Questo codice produce l'intestazione seguente:</span><span class="sxs-lookup"><span data-stu-id="6069a-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="6069a-225">Profili della cache</span><span class="sxs-lookup"><span data-stu-id="6069a-225">Cache profiles</span></span>

<span data-ttu-id="6069a-226">Anziché ripetere `ResponseCache` impostazioni negli attributi di molte azioni di controller, sui profili cache possono essere configurate come opzioni quando si configura MVC nel `ConfigureServices` metodo `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6069a-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="6069a-227">Vengono usati valori trovati in un profilo della cache di cui viene fatto riferimento come le impostazioni predefinite per il `ResponseCache` attributo e vengono sovrascritte dalle eventuali proprietà specificate per l'attributo.</span><span class="sxs-lookup"><span data-stu-id="6069a-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="6069a-228">Configurazione di un profilo della cache:</span><span class="sxs-lookup"><span data-stu-id="6069a-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6069a-229">Che fanno riferimento a un profilo della cache:</span><span class="sxs-lookup"><span data-stu-id="6069a-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="6069a-230">Il `ResponseCache` attributo può essere applicato sia alle azioni (metodi) e controller (classi).</span><span class="sxs-lookup"><span data-stu-id="6069a-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="6069a-231">Gli attributi a livello di metodo sostituiscono le impostazioni specificate negli attributi a livello di classe.</span><span class="sxs-lookup"><span data-stu-id="6069a-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="6069a-232">Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre un attributo a livello di metodo fa riferimento a un profilo della cache con una durata di 60 secondi.</span><span class="sxs-lookup"><span data-stu-id="6069a-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="6069a-233">L'intestazione risulta:</span><span class="sxs-lookup"><span data-stu-id="6069a-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="6069a-234">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6069a-234">Additional resources</span></span>

* [<span data-ttu-id="6069a-235">La memorizzazione delle risposte nella cache</span><span class="sxs-lookup"><span data-stu-id="6069a-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="6069a-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="6069a-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
