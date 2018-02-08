---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: tdykstra
description: Presentazione di Kestrel, il server Web multipiattaforma per ASP.NET Core basato su libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a8da0-103">Introduzione all'implementazione del server Web Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8da0-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a8da0-104">Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="a8da0-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="a8da0-105">Kestrel è un [server Web per ASP.NET Core](index.md) multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="a8da0-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="a8da0-106">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8da0-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="a8da0-107">Kestrel supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="a8da0-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="a8da0-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a8da0-108">HTTPS</span></span>
  * <span data-ttu-id="a8da0-109">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="a8da0-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="a8da0-110">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="a8da0-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="a8da0-111">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8da0-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-113">[Visualizzare o scaricare il codice di esempio per 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8da0-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8da0-115">[Visualizzare o scaricare il codice di esempio per 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8da0-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a8da0-116">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="a8da0-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-118">È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="a8da0-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="a8da0-119">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="a8da0-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a8da0-122">È possibile usare una delle due configurazioni &mdash; con o senza un server proxy inverso &mdash; anche se Kestrel viene esposto solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="a8da0-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8da0-124">Se l'applicazione accetta le richieste solo da una rete interna, è possibile usare Kestrel da solo.</span><span class="sxs-lookup"><span data-stu-id="a8da0-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="a8da0-126">Se si espone l'applicazione a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="a8da0-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="a8da0-127">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="a8da0-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a8da0-129">Per motivi di sicurezza, un proxy inverso è obbligatorio per le distribuzioni perimetrali, ovvero le distribuzioni esposte al traffico Internet.</span><span class="sxs-lookup"><span data-stu-id="a8da0-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="a8da0-130">Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="a8da0-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="a8da0-131">Questo include, senza limitazioni, timeout appropriati, limiti delle dimensioni e limiti delle connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="a8da0-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="a8da0-132">Uno scenario che richiede un proxy inverso è il caso in cui varie applicazioni che condividono lo stesso IP e la stessa porta sono in esecuzione in un singolo server.</span><span class="sxs-lookup"><span data-stu-id="a8da0-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="a8da0-133">Questo scenario non è direttamente applicabile a Kestrel, perché Kestrel non supporta la condivisione dello stesso IP e della porta tra più processi.</span><span class="sxs-lookup"><span data-stu-id="a8da0-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="a8da0-134">Quando lo si configura per l'ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host.</span><span class="sxs-lookup"><span data-stu-id="a8da0-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="a8da0-135">Un proxy inverso che può condividere porte deve quindi eseguire l'inoltro a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="a8da0-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a8da0-136">Anche se un server proxy inverso non è necessario, può risultare utile per altri motivi:</span><span class="sxs-lookup"><span data-stu-id="a8da0-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="a8da0-137">Può limitare la superficie di attacco esposta.</span><span class="sxs-lookup"><span data-stu-id="a8da0-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="a8da0-138">Offre un livello facoltativo aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="a8da0-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a8da0-139">Può offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="a8da0-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a8da0-140">Semplifica il bilanciamento del carico e la configurazione SSL.</span><span class="sxs-lookup"><span data-stu-id="a8da0-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="a8da0-141">Solo il server proxy inverso richiede un certificato SSL e tale server può comunicare con i server applicazioni sulla rete interna mediante HTTP semplice.</span><span class="sxs-lookup"><span data-stu-id="a8da0-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a8da0-142">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8da0-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-144">Il pacchetto [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="a8da0-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="a8da0-145">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a8da0-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a8da0-146">In *Program.cs* il codice modello chiama `CreateDefaultBuilder`, che chiama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) in background.</span><span class="sxs-lookup"><span data-stu-id="a8da0-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="a8da0-147">Se è necessario configurare le opzioni di Kestrel, chiamare `UseKestrel` in *Program.cs* come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a8da0-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8da0-149">Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="a8da0-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="a8da0-150">Chiamare il metodo di estensione [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) su `WebHostBuilder` nel metodo `Main`, specificando le [opzioni Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) desiderate come visualizzato nelle sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="a8da0-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="a8da0-151">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8da0-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-153">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="a8da0-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="a8da0-154">Ecco alcuni limiti che è possibile impostare:</span><span class="sxs-lookup"><span data-stu-id="a8da0-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="a8da0-155">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="a8da0-155">Maximum client connections</span></span>
- <span data-ttu-id="a8da0-156">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="a8da0-156">Maximum request body size</span></span>
- <span data-ttu-id="a8da0-157">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="a8da0-157">Minimum request body data rate</span></span>

<span data-ttu-id="a8da0-158">Questi ed altri vincoli sono disponibili nella proprietà `Limits` della classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="a8da0-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="a8da0-159">La proprietà `Limits` contiene un'istanza della classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs).</span><span class="sxs-lookup"><span data-stu-id="a8da0-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="a8da0-160">**Numero massimo di connessioni client**</span><span class="sxs-lookup"><span data-stu-id="a8da0-160">**Maximum client connections**</span></span>

<span data-ttu-id="a8da0-161">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera applicazione con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="a8da0-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="a8da0-162">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="a8da0-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a8da0-163">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="a8da0-164">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="a8da0-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="a8da0-165">**Dimensione massima del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="a8da0-165">**Maximum request body size**</span></span>

<span data-ttu-id="a8da0-166">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="a8da0-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="a8da0-167">Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="a8da0-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a8da0-168">L'esempio seguente visualizza come configurare il vincolo per l'intera applicazione e ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="a8da0-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="a8da0-169">È possibile eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="a8da0-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="a8da0-170">Se si prova a configurare il limite per una richiesta dopo che l'applicazione ha avviato la lettura della richiesta stessa, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a8da0-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="a8da0-171">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="a8da0-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="a8da0-172">**Velocità minima dei dati del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="a8da0-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="a8da0-173">Kestrel controlla ogni secondo se i dati vengono ricevuto alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="a8da0-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="a8da0-174">Se la velocità scende sotto il valore minimo, la connessione raggiunge il timeout. Il periodo di tolleranza è la quantità di tempo che Kestrel concede al client per aumentare la velocità di trasmissione fino al valore minimo. Durante tale periodo la velocità non viene rilevata.</span><span class="sxs-lookup"><span data-stu-id="a8da0-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a8da0-175">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="a8da0-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a8da0-176">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="a8da0-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="a8da0-177">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="a8da0-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a8da0-178">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="a8da0-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="a8da0-179">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8da0-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="a8da0-180">È possibile configurare la velocità per ogni singola richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="a8da0-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="a8da0-181">Per informazioni su altre opzioni di Kestrel, vedere le classi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a8da0-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="a8da0-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="a8da0-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="a8da0-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="a8da0-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="a8da0-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="a8da0-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8da0-186">Per informazioni sulle opzioni di Kestrel, vedere la [classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="a8da0-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="a8da0-187">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="a8da0-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-189">Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a8da0-190">È possibile configurare i prefissi URL e le porte sulle quali è in ascolto Kestrel chiamando i metodi `Listen` o `ListenUnixSocket` su `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="a8da0-191">Anche `UseUrls`, l'argomento della riga di comando `urls` e la variabile di ambiente ASPNETCORE_URLS funzionano, ma con le limitazioni indicate [più avanti in questo articolo](#useurls-limitations).</span><span class="sxs-lookup"><span data-stu-id="a8da0-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="a8da0-192">**Associazione a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="a8da0-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="a8da0-193">Il metodo `Listen` esegue l'associazione a un socket TCP. Un'espressione lambda per le opzioni consente di configurare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="a8da0-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="a8da0-194">Si noti come in questo esempio SSL viene configurato per un endpoint specifico mediante [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="a8da0-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="a8da0-195">È possibile usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="a8da0-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="a8da0-196">**Associazione a un socket Unix**</span><span class="sxs-lookup"><span data-stu-id="a8da0-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="a8da0-197">È possibile impostare l'ascolto su un socket Unix per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a8da0-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="a8da0-198">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="a8da0-198">**Port 0**</span></span>

<span data-ttu-id="a8da0-199">Se si specifica il numero di porta 0, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="a8da0-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a8da0-200">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="a8da0-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="a8da0-201">**Limitazioni di UseUrls**</span><span class="sxs-lookup"><span data-stu-id="a8da0-201">**UseUrls limitations**</span></span>

<span data-ttu-id="a8da0-202">È possibile configurare gli endpoint chiamando il metodo `UseUrls` o usando l'argomento della riga di comando `urls` o la variabile di ambiente ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="a8da0-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="a8da0-203">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a8da0-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="a8da0-204">Tenere tuttavia presenti queste limitazioni:</span><span class="sxs-lookup"><span data-stu-id="a8da0-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="a8da0-205">Non è possibile usare SSL con questi metodi.</span><span class="sxs-lookup"><span data-stu-id="a8da0-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="a8da0-206">Se si usa sia il metodo `Listen` sia `UseUrls`, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="a8da0-207">**Configurazione endpoint per IIS**</span><span class="sxs-lookup"><span data-stu-id="a8da0-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="a8da0-208">Se si usa IIS i binding URL per IIS sostituiscono tutti i binding impostati chiamando `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a8da0-209">Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="a8da0-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8da0-211">Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a8da0-212">È possibile configurare le porte e i prefissi URL sui quali è in ascolto Kestrel usando il metodo di estensione `UseUrls`, l'argomento della riga di comando `urls` o il sistema di configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8da0-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="a8da0-213">Per altre informazioni su questi metodi, vedere [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="a8da0-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="a8da0-214">Per informazioni sul funzionamento dell'associazione URL quando si usa IIS come proxy inverso, vedere [Modulo ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="a8da0-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="a8da0-215">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="a8da0-215">URL prefixes</span></span>

<span data-ttu-id="a8da0-216">Se si chiama `UseUrls` o si usa l'argomento della riga di comando `urls` o la variabile di ambiente ASPNETCORE_URLS, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="a8da0-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8da0-218">Sono validi solo i prefissi URL HTTP. Kestrel non supporta SSL quando le associazioni vengono configurate URL usando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="a8da0-219">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a8da0-220">0.0.0.0 è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="a8da0-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="a8da0-221">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="a8da0-222">[::] è l'equivalente IPv6 di 0.0.0.0 per IPv4.</span><span class="sxs-lookup"><span data-stu-id="a8da0-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="a8da0-223">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a8da0-224">I nomi host, \* e + non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="a8da0-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="a8da0-225">Tutto ciò che non è un indirizzo IP o un elemento "localhost" riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="a8da0-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a8da0-226">Se è necessario associare nomi host diversi ad applicazioni ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](httpsys.md) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="a8da0-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="a8da0-227">Nome "localhost" con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a8da0-228">Se è specificato `localhost`, Kestrel prova l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="a8da0-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a8da0-229">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="a8da0-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a8da0-230">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="a8da0-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="a8da0-232">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="a8da0-233">0.0.0.0 è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="a8da0-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="a8da0-234">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="a8da0-235">[::] è l'equivalente IPv6 di 0.0.0.0 per IPv4.</span><span class="sxs-lookup"><span data-stu-id="a8da0-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="a8da0-236">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="a8da0-237">I nomi host, \* e + non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="a8da0-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="a8da0-238">Tutto ciò che non è un indirizzo IP o un elemento "localhost" riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="a8da0-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a8da0-239">Se è necessario associare nomi host diversi ad applicazioni ASP.NET Core diverse sulla stessa porta, usare [WebListener](weblistener.md) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="a8da0-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="a8da0-240">Nome "localhost" con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="a8da0-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a8da0-241">Se è specificato `localhost`, Kestrel prova l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="a8da0-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a8da0-242">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="a8da0-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a8da0-243">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="a8da0-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="a8da0-244">Socket UNIX</span><span class="sxs-lookup"><span data-stu-id="a8da0-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="a8da0-245">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="a8da0-245">**Port 0**</span></span>

<span data-ttu-id="a8da0-246">Se si specifica il numero di porta 0, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="a8da0-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a8da0-247">L'associazione alla porta 0 è consentita per qualsiasi nome host o IP ad eccezione del nome `localhost`.</span><span class="sxs-lookup"><span data-stu-id="a8da0-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="a8da0-248">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="a8da0-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="a8da0-249">**Prefissi URL per SSL**</span><span class="sxs-lookup"><span data-stu-id="a8da0-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="a8da0-250">Assicurarsi di includere i prefissi URL con `https:` se si chiama il metodo di estensione `UseHttps`, come visualizzato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a8da0-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="a8da0-251">HTTPS e HTTP non possono essere ospitati sulla stessa porta.</span><span class="sxs-lookup"><span data-stu-id="a8da0-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="a8da0-252">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a8da0-252">Next steps</span></span>

<span data-ttu-id="a8da0-253">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="a8da0-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8da0-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="a8da0-255">App di esempio per 2.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="a8da0-256">Codice sorgente di Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8da0-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8da0-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="a8da0-258">App di esempio per 1.x</span><span class="sxs-lookup"><span data-stu-id="a8da0-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="a8da0-259">Codice sorgente di Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8da0-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
