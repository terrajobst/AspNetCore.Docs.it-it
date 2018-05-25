---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: rick-anderson
description: Informazioni su Kestrel, il server Web multipiattaforma per ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 251385b268e75cfadb815c293be52176297ed3e4
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9deba-103">Implementazione del server Web Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9deba-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9deba-104">Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="9deba-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="9deba-105">Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="9deba-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="9deba-106">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9deba-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="9deba-107">Kestrel supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="9deba-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9deba-108">HTTPS</span></span>
* <span data-ttu-id="9deba-109">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="9deba-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="9deba-110">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="9deba-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="9deba-111">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9deba-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="9deba-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9deba-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="9deba-113">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="9deba-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9deba-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9deba-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9deba-115">È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="9deba-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="9deba-116">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="9deba-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="9deba-119">È consigliabile usare Kestrel con un server proxy inverso, a meno che non venga esposto solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="9deba-119">We recommend using Kestrel with a reverse proxy server unless Kestrel is only exposed to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9deba-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9deba-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9deba-121">Se un'app accetta solo le richieste provenienti da una rete interna, è possibile usare Kestrel direttamente come server dell'app.</span><span class="sxs-lookup"><span data-stu-id="9deba-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="9deba-123">Se si espone l'app a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="9deba-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="9deba-124">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="9deba-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="9deba-126">Per motivi di sicurezza, un proxy inverso è obbligatorio per le distribuzioni perimetrali, ovvero le distribuzioni esposte al traffico Internet.</span><span class="sxs-lookup"><span data-stu-id="9deba-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="9deba-127">Le versioni 1.x di Kestrel non hanno una struttura completa di difesa contro gli attacchi, ad esempio timeout appropriati, limiti di dimensioni e limiti di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="9deba-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="9deba-128">Esiste uno scenario con proxy inverso quando diverse app condividono lo stesso IP e la stessa porta in un unico server.</span><span class="sxs-lookup"><span data-stu-id="9deba-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="9deba-129">Kestrel non supporta questo scenario perché non supporta la condivisione dello stesso IP e della stessa porta tra più processi.</span><span class="sxs-lookup"><span data-stu-id="9deba-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="9deba-130">Quando Kestrel è configurato per restare in ascolto su una porta, gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host delle richieste.</span><span class="sxs-lookup"><span data-stu-id="9deba-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="9deba-131">Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="9deba-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="9deba-132">Anche se un server proxy inverso non è necessario, può risultare utile per i motivi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="9deba-133">Limita l'area di superficie di attacco pubblica esposta delle app ospitate.</span><span class="sxs-lookup"><span data-stu-id="9deba-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="9deba-134">Offre un livello aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="9deba-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="9deba-135">Può offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="9deba-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="9deba-136">Semplifica il bilanciamento del carico e la configurazione SSL.</span><span class="sxs-lookup"><span data-stu-id="9deba-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="9deba-137">Solo il server proxy inverso richiede un certificato SSL e tale server può comunicare con i server delle applicazioni sulla rete interna mediante protocollo HTTP semplice.</span><span class="sxs-lookup"><span data-stu-id="9deba-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="9deba-138">Se non si usa un proxy inverso con filtro host abilitato, è necessario abilitate il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="9deba-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="9deba-139">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9deba-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9deba-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9deba-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9deba-141">Il pacchetto [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="9deba-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="9deba-142">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9deba-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="9deba-143">In *Program.cs* il codice modello chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), che a sua volta chiama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) in background.</span><span class="sxs-lookup"><span data-stu-id="9deba-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9deba-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9deba-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9deba-145">Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="9deba-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="9deba-146">Chiamare il metodo di estensione [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) nel metodo `Main`, specificando le [opzioni Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) richieste, come visualizzato nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="9deba-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="9deba-147">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="9deba-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9deba-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9deba-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9deba-149">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="9deba-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="9deba-150">Alcuni limiti importanti che possono essere personalizzati:</span><span class="sxs-lookup"><span data-stu-id="9deba-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="9deba-151">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="9deba-151">Maximum client connections</span></span>
* <span data-ttu-id="9deba-152">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="9deba-152">Maximum request body size</span></span>
* <span data-ttu-id="9deba-153">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="9deba-153">Minimum request body data rate</span></span>

<span data-ttu-id="9deba-154">Impostare questi e altri vincoli nella proprietà [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) della classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="9deba-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="9deba-155">La proprietà `Limits` contiene un'istanza della classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="9deba-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="9deba-156">**Numero massimo di connessioni client**</span><span class="sxs-lookup"><span data-stu-id="9deba-156">**Maximum client connections**</span></span>

[<span data-ttu-id="9deba-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="9deba-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="9deba-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="9deba-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="9deba-159">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9deba-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="9deba-160">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="9deba-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="9deba-161">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="9deba-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="9deba-162">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="9deba-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="9deba-163">**Dimensione massima del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="9deba-163">**Maximum request body size**</span></span>

[<span data-ttu-id="9deba-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="9deba-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="9deba-165">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="9deba-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="9deba-166">Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="9deba-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="9deba-167">L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="9deba-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="9deba-168">È possibile eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="9deba-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="9deba-169">Se si tenta di configurare il limite per una richiesta dopo che l'app ha avviato la lettura della richiesta stessa, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9deba-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="9deba-170">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="9deba-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="9deba-171">**Velocità minima dei dati del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="9deba-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="9deba-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="9deba-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="9deba-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="9deba-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="9deba-174">Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="9deba-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="9deba-175">Se la velocità scende sotto il valore minimo, la connessione raggiunge il timeout. Il periodo di tolleranza è la quantità di tempo che Kestrel concede al client per aumentare la velocità di trasmissione fino al valore minimo. Durante tale periodo la velocità non viene rilevata.</span><span class="sxs-lookup"><span data-stu-id="9deba-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="9deba-176">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="9deba-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="9deba-177">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="9deba-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="9deba-178">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="9deba-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="9deba-179">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="9deba-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="9deba-180">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9deba-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="9deba-181">È possibile configurare la velocità per ogni singola richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="9deba-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="9deba-182">Per informazioni su altre opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="9deba-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="9deba-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="9deba-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="9deba-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="9deba-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="9deba-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="9deba-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9deba-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9deba-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9deba-187">Per informazioni su opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="9deba-187">For information about Kestrel options and limits, see:</span></span>

* <span data-ttu-id="9deba-188">[KestrelServerOptions class](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) (Classe KestrelServerOptions)</span><span class="sxs-lookup"><span data-stu-id="9deba-188">[KestrelServerOptions class](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)</span></span>
* [<span data-ttu-id="9deba-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="9deba-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="9deba-190">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="9deba-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9deba-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9deba-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9deba-192">Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9deba-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9deba-193">Chiamare i metodi [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oppure [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) su [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) per configurare i prefissi URL e le porte per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9deba-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="9deba-194">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione.</span><span class="sxs-lookup"><span data-stu-id="9deba-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="9deba-195">La chiave di configurazione dell'host `urls` deve provenire dalla configurazione dell'host, non dalla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="9deba-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="9deba-196">L'aggiunta di una chiave `urls` e di un valore a *appsettings.json* non influisce sulla configurazione dell'host perché l'host viene completamente inizializzato prima che la configurazione venga letta dal file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="9deba-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="9deba-197">Tuttavia, una chiave `urls` in *appsettings.json* può essere usata con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) nel generatore di host per configurare l'host:</span><span class="sxs-lookup"><span data-stu-id="9deba-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9deba-198">Per impostazione predefinita, ASP.NET Core è associato a:</span><span class="sxs-lookup"><span data-stu-id="9deba-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="9deba-199">`https://localhost:5001` (quando è presente un certificato di sviluppo locale)</span><span class="sxs-lookup"><span data-stu-id="9deba-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="9deba-200">Viene creato un certificato di sviluppo nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-200">A development certificate is created:</span></span>

* <span data-ttu-id="9deba-201">Quando viene installato [.NET Core SDK](/dotnet/core/sdk).</span><span class="sxs-lookup"><span data-stu-id="9deba-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="9deba-202">Quando per creare un certificato viene usato [lo strumento dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs).</span><span class="sxs-lookup"><span data-stu-id="9deba-202">The [dev-certs tool](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) is used to create a certificate.</span></span>

<span data-ttu-id="9deba-203">Alcuni browser richiedono un'autorizzazione esplicita per considerare attendibile il certificato di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="9deba-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="9deba-204">ASP.NET Core 2.1 e le versioni successive dei modelli di progetto configurano le app da eseguire su HTTPS per impostazione predefinita e includono [il reindirizzamento a HTTPS e il supporto di HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="9deba-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="9deba-205">Chiamare i metodi [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oppure [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) su [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) per configurare i prefissi URL e le porte per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9deba-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="9deba-206">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).</span><span class="sxs-lookup"><span data-stu-id="9deba-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="9deba-207">Configurazione di `KestrelServerOptions` in ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="9deba-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="9deba-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="9deba-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="9deba-209">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="9deba-210">Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="9deba-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="9deba-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="9deba-212">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9deba-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="9deba-213">Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="9deba-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="9deba-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="9deba-215">Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) come input.</span><span class="sxs-lookup"><span data-stu-id="9deba-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="9deba-216">L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9deba-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="9deba-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="9deba-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="9deba-218">Configura Kestrel per l'uso di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9deba-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="9deba-219">Estensioni `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="9deba-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="9deba-220">`UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="9deba-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="9deba-221">Genera un'eccezione se non è stato configurato alcun certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="9deba-221">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="9deba-222">Parametri `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="9deba-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="9deba-223">`filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="9deba-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="9deba-224">`password` è la password richiesta per accedere ai dati del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="9deba-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="9deba-225">`configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="9deba-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="9deba-226">Restituisce `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="9deba-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="9deba-227">`storeName` è l'archivio certificati da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="9deba-228">`subject` è il nome dell'oggetto del certificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="9deba-229">`allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="9deba-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="9deba-230">`location` è il percorso dell'archivio da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="9deba-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="9deba-231">`serverCertificate` è il certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="9deba-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="9deba-232">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="9deba-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="9deba-233">Come minimo, è necessario specificare un certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="9deba-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="9deba-234">Configurazioni supportate descritte in seguito:</span><span class="sxs-lookup"><span data-stu-id="9deba-234">Supported configurations described next:</span></span>

* <span data-ttu-id="9deba-235">Nessuna configurazione</span><span class="sxs-lookup"><span data-stu-id="9deba-235">No configuration</span></span>
* <span data-ttu-id="9deba-236">Sostituire il certificato predefinito della configurazione</span><span class="sxs-lookup"><span data-stu-id="9deba-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="9deba-237">Modificare le impostazioni predefinite nel codice</span><span class="sxs-lookup"><span data-stu-id="9deba-237">Change the defaults in code</span></span>

<span data-ttu-id="9deba-238">*Nessuna configurazione*</span><span class="sxs-lookup"><span data-stu-id="9deba-238">*No configuration*</span></span>

<span data-ttu-id="9deba-239">Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="9deba-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="9deba-240">Specificare gli URL usando gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-240">Specify URLs using the:</span></span>

* <span data-ttu-id="9deba-241">La variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9deba-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="9deba-242">L'argomento della riga di comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="9deba-243">La chiave di configurazione dell'host `urls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="9deba-244">Il metodo di estensione `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="9deba-245">Per altre informazioni, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="9deba-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="9deba-246">Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="9deba-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="9deba-247">Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="9deba-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="9deba-248">*Sostituire il certificato predefinito della configurazione*</span><span class="sxs-lookup"><span data-stu-id="9deba-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="9deba-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9deba-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="9deba-250">È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito.</span><span class="sxs-lookup"><span data-stu-id="9deba-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="9deba-251">Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="9deba-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="9deba-252">Nel file *appsettings.json* di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9deba-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="9deba-253">Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="9deba-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="9deba-254">Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default**  o il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9deba-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="9deba-255">Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="9deba-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="9deba-256">Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:</span><span class="sxs-lookup"><span data-stu-id="9deba-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="9deba-257">Note di schema:</span><span class="sxs-lookup"><span data-stu-id="9deba-257">Schema notes:</span></span>

* <span data-ttu-id="9deba-258">I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="9deba-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="9deba-259">Ad esempio, sono validi sia `HTTPS` che `Https`.</span><span class="sxs-lookup"><span data-stu-id="9deba-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="9deba-260">Il parametro `Url` è obbligatorio per ogni endpoint.</span><span class="sxs-lookup"><span data-stu-id="9deba-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="9deba-261">Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.</span><span class="sxs-lookup"><span data-stu-id="9deba-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="9deba-262">Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono.</span><span class="sxs-lookup"><span data-stu-id="9deba-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="9deba-263">Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="9deba-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="9deba-264">La sezione `Certificate` è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="9deba-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="9deba-265">Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti.</span><span class="sxs-lookup"><span data-stu-id="9deba-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="9deba-266">Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="9deba-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="9deba-267">La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="9deba-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="9deba-268">In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.</span><span class="sxs-lookup"><span data-stu-id="9deba-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="9deba-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, options => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:</span><span class="sxs-lookup"><span data-stu-id="9deba-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="9deba-270">È anche possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per mantenere l'iterazione nel caricatore esistente, ad esempio quello specificato da `WebHost.CreatedDeafaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9deba-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by `WebHost.CreatedDeafaultBuilder`.</span></span>

* <span data-ttu-id="9deba-271">La sezione di configurazione per ogni endpoint è disponibile sulle opzioni nel metodo `Endpoint` in modo che le impostazioni personalizzate possano essere lette.</span><span class="sxs-lookup"><span data-stu-id="9deba-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="9deba-272">È possibile caricare più configurazioni chiamando ancora `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` con un'altra sezione.</span><span class="sxs-lookup"><span data-stu-id="9deba-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="9deba-273">Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti.</span><span class="sxs-lookup"><span data-stu-id="9deba-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="9deba-274">Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.</span><span class="sxs-lookup"><span data-stu-id="9deba-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="9deba-275">`KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="9deba-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="9deba-276">Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.</span><span class="sxs-lookup"><span data-stu-id="9deba-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="9deba-277">*Modificare le impostazioni predefinite nel codice*</span><span class="sxs-lookup"><span data-stu-id="9deba-277">*Change the defaults in code*</span></span>

<span data-ttu-id="9deba-278">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente.</span><span class="sxs-lookup"><span data-stu-id="9deba-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="9deba-279">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.</span><span class="sxs-lookup"><span data-stu-id="9deba-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="9deba-280">*Supporto kestrel per SNI*</span><span class="sxs-lookup"><span data-stu-id="9deba-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="9deba-281">[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="9deba-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="9deba-282">Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto.</span><span class="sxs-lookup"><span data-stu-id="9deba-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="9deba-283">Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="9deba-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="9deba-284">Kestrel supporta SNI tramite il callback `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="9deba-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="9deba-285">Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="9deba-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="9deba-286">Il supporto di SNI richiede che l'esecuzione avvenga nel framework di destinazione `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="9deba-286">SNI support requires running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="9deba-287">In `netcoreapp2.0` e `net461` il callback viene richiamato, ma l'elemento `name` è sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="9deba-287">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="9deba-288">L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="9deba-288">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

<span data-ttu-id="9deba-289">**Associazione a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="9deba-289">**Bind to a TCP socket**</span></span>

<span data-ttu-id="9deba-290">Il metodo [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) esegue l'associazione a un socket TCP. Un'espressione lambda per le opzioni consente di configurare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="9deba-290">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="9deba-291">Si noti come in questo esempio SSL viene configurato per un endpoint specifico mediante [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="9deba-291">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="9deba-292">Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="9deba-292">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="9deba-293">**Associazione a un socket Unix**</span><span class="sxs-lookup"><span data-stu-id="9deba-293">**Bind to a Unix socket**</span></span>

<span data-ttu-id="9deba-294">Impostare l'ascolto su un socket Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9deba-294">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="9deba-295">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="9deba-295">**Port 0**</span></span>

<span data-ttu-id="9deba-296">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="9deba-296">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="9deba-297">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="9deba-297">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="9deba-298">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="9deba-298">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="9deba-299">**UseUrls, argomento della riga di comando --urls, chiave di configurazione dell'host urls e limitazioni della variabile di ambiente ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="9deba-299">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="9deba-300">Configurare gli endpoint con i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-300">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="9deba-301">UseUrls</span><span class="sxs-lookup"><span data-stu-id="9deba-301">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="9deba-302">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="9deba-302">`--urls` command-line argument</span></span>
* <span data-ttu-id="9deba-303">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="9deba-303">`urls` host configuration key</span></span>
* <span data-ttu-id="9deba-304">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="9deba-304">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="9deba-305">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9deba-305">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="9deba-306">Tenere tuttavia presenti queste limitazioni:</span><span class="sxs-lookup"><span data-stu-id="9deba-306">However, be aware of these limitations:</span></span>

* <span data-ttu-id="9deba-307">SSL non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="9deba-307">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="9deba-308">Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-308">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="9deba-309">**Configurazione degli endpoint IIS**</span><span class="sxs-lookup"><span data-stu-id="9deba-309">**IIS endpoint configuration**</span></span>

<span data-ttu-id="9deba-310">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-310">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="9deba-311">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9deba-311">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9deba-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9deba-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9deba-313">Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9deba-313">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9deba-314">Configurare i prefissi URL e le porte per l'uso di Kestrel:</span><span class="sxs-lookup"><span data-stu-id="9deba-314">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="9deba-315">Il metodo di estensione [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="9deba-315">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="9deba-316">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="9deba-316">`--urls` command-line argument</span></span>
* <span data-ttu-id="9deba-317">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="9deba-317">`urls` host configuration key</span></span>
* <span data-ttu-id="9deba-318">Il sistema di configurazione di ASP.NET Core, inclusa la variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="9deba-318">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="9deba-319">Per altre informazioni su questi metodi, vedere [Hosting](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="9deba-319">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="9deba-320">**Configurazione degli endpoint IIS**</span><span class="sxs-lookup"><span data-stu-id="9deba-320">**IIS endpoint configuration**</span></span>

<span data-ttu-id="9deba-321">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-321">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="9deba-322">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9deba-322">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="9deba-323">Configurazione del trasporto</span><span class="sxs-lookup"><span data-stu-id="9deba-323">Transport configuration</span></span>

<span data-ttu-id="9deba-324">Con la versione ASP.NET Core 2.1, il trasporto predefinito di Kestrel non si basa più su Libuv, ma su socket gestiti.</span><span class="sxs-lookup"><span data-stu-id="9deba-324">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="9deba-325">Si tratta di una modifica importante per le app ASP.NET 2.0 Core che vengono aggiornate alla versione 2.1, che chiamano [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) e dipendono da uno dei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9deba-325">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="9deba-326">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (riferimento diretto al pacchetto)</span><span class="sxs-lookup"><span data-stu-id="9deba-326">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="9deba-327">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="9deba-327">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="9deba-328">Per i progetti ASP.NET Core 2.1 o versioni successive che usano il metapacchetto `Microsoft.AspNetCore.App` e richiedono l'uso di Libuv:</span><span class="sxs-lookup"><span data-stu-id="9deba-328">For ASP.NET Core 2.1 or later projects that use the `Microsoft.AspNetCore.App` metapackage and require the use of Libuv:</span></span>

* <span data-ttu-id="9deba-329">Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="9deba-329">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                    Version="2.1.0" />
    ```

* <span data-ttu-id="9deba-330">Chiamare [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="9deba-330">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="9deba-331">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="9deba-331">URL prefixes</span></span>

<span data-ttu-id="9deba-332">Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="9deba-332">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9deba-333">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9deba-333">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9deba-334">Sono validi solo i prefissi URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="9deba-334">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="9deba-335">Kestrel non supporta SSL quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9deba-335">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="9deba-336">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-336">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="9deba-337">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="9deba-337">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="9deba-338">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-338">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="9deba-339">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="9deba-339">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="9deba-340">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-340">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="9deba-341">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="9deba-341">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="9deba-342">Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="9deba-342">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="9deba-343">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="9deba-343">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="9deba-344">Se non si usa un proxy inverso con filtro host abilitato, abilitare il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="9deba-344">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="9deba-345">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-345">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="9deba-346">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="9deba-346">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="9deba-347">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="9deba-347">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="9deba-348">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="9deba-348">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9deba-349">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9deba-349">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="9deba-350">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-350">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="9deba-351">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="9deba-351">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="9deba-352">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-352">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="9deba-353">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="9deba-353">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="9deba-354">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-354">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="9deba-355">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="9deba-355">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="9deba-356">Tutto ciò che non è un indirizzo IP o un elemento `localhost` riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="9deba-356">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="9deba-357">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [WebListener](xref:fundamentals/servers/weblistener) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="9deba-357">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="9deba-358">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="9deba-358">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="9deba-359">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="9deba-359">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="9deba-360">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="9deba-360">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="9deba-361">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="9deba-361">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="9deba-362">Socket UNIX</span><span class="sxs-lookup"><span data-stu-id="9deba-362">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="9deba-363">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="9deba-363">**Port 0**</span></span>

<span data-ttu-id="9deba-364">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="9deba-364">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="9deba-365">L'associazione alla porta `0` è consentita per qualsiasi nome host o IP ad eccezione di `localhost`.</span><span class="sxs-lookup"><span data-stu-id="9deba-365">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="9deba-366">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="9deba-366">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="9deba-367">**Prefissi URL per SSL**</span><span class="sxs-lookup"><span data-stu-id="9deba-367">**URL prefixes for SSL**</span></span>

<span data-ttu-id="9deba-368">Se si chiama il metodo di estensione `UseHttps`, assicurarsi di includere i prefissi URL con `https:`:</span><span class="sxs-lookup"><span data-stu-id="9deba-368">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="9deba-369">HTTPS e HTTP non possono essere ospitati sulla stessa porta.</span><span class="sxs-lookup"><span data-stu-id="9deba-369">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="9deba-370">Filtro host</span><span class="sxs-lookup"><span data-stu-id="9deba-370">Host filtering</span></span>

<span data-ttu-id="9deba-371">Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host.</span><span class="sxs-lookup"><span data-stu-id="9deba-371">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="9deba-372">L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback.</span><span class="sxs-lookup"><span data-stu-id="9deba-372">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="9deba-373">Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici.</span><span class="sxs-lookup"><span data-stu-id="9deba-373">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="9deba-374">Nessuna di queste informazioni viene usata per convalidare le intestazioni `Host` della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9deba-374">None of this information is used to validate request `Host` headers.</span></span>

<span data-ttu-id="9deba-375">Esistono due soluzioni alternative:</span><span class="sxs-lookup"><span data-stu-id="9deba-375">There are two workarounds:</span></span>

* <span data-ttu-id="9deba-376">Eseguire l'host in un proxy inverso con filtro delle intestazioni host.</span><span class="sxs-lookup"><span data-stu-id="9deba-376">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="9deba-377">Questo era l'unico scenario supportato per Kestrel in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9deba-377">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="9deba-378">Usare un middleware per filtrare le richieste in base all'intestazione `Host`.</span><span class="sxs-lookup"><span data-stu-id="9deba-378">Use a middleware to filter requests by the `Host` header.</span></span> <span data-ttu-id="9deba-379">Di seguito viene riportato un tipo di middleware di esempio:</span><span class="sxs-lookup"><span data-stu-id="9deba-379">A sample middleware follows:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="9deba-380">Registrare l'elemento `HostFilteringMiddleware` precedente in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9deba-380">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="9deba-381">Si noti che l'[ordinamento della registrazione del middleware](xref:fundamentals/middleware/index#ordering) è importante.</span><span class="sxs-lookup"><span data-stu-id="9deba-381">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="9deba-382">La registrazione deve verificarsi immediatamente dopo la registrazione del middleware di diagnostica (ad esempio, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="9deba-382">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="9deba-383">Per il middleware precedente è prevista una chiave `AllowedHosts` in *appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="9deba-383">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="9deba-384">Il valore della chiave è un elenco delimitato da punto e virgola di nomi host senza numeri di porta.</span><span class="sxs-lookup"><span data-stu-id="9deba-384">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="9deba-385">Includere la coppia chiave-valore `AllowedHosts` in *appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="9deba-385">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="9deba-386">*appsettings.Development.json* (file di configurazione di localhost):</span><span class="sxs-lookup"><span data-stu-id="9deba-386">*appsettings.Development.json* (localhost configuration file):</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a><span data-ttu-id="9deba-387">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9deba-387">Additional resources</span></span>

* [<span data-ttu-id="9deba-388">Imporre HTTPS</span><span class="sxs-lookup"><span data-stu-id="9deba-388">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="9deba-389">Codice sorgente di Kestrel</span><span class="sxs-lookup"><span data-stu-id="9deba-389">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
