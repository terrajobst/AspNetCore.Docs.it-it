---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: guardrex
description: Informazioni su Kestrel, il server Web multipiattaforma per ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/11/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: a85403468d64b35ac5b6754139f78a12ad3fc386
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249529"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="96290-103">Implementazione del server Web Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96290-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="96290-104">Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="96290-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="96290-105">Per la versione 1.1 di questo argomento, scaricare [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf) (Implementazione del server Web Kestrel in ASP.NET Core - Versione 1.1, PDF).</span><span class="sxs-lookup"><span data-stu-id="96290-105">For the 1.1 version of this topic, download [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="96290-106">Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="96290-106">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="96290-107">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96290-107">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="96290-108">Kestrel supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-108">Kestrel supports the following scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="96290-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="96290-109">HTTPS</span></span>
* <span data-ttu-id="96290-110">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="96290-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="96290-111">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="96290-111">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="96290-112">HTTP/2 (tranne che in macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="96290-112">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="96290-113">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="96290-113">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="96290-114">HTTPS</span><span class="sxs-lookup"><span data-stu-id="96290-114">HTTPS</span></span>
* <span data-ttu-id="96290-115">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="96290-115">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="96290-116">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="96290-116">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="96290-117">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="96290-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="96290-118">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96290-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="96290-119">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="96290-119">HTTP/2 support</span></span>

<span data-ttu-id="96290-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è disponibile per le app ASP.NET Core se vengono soddisfatti i requisiti di base seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="96290-121">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="96290-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="96290-122">Windows Server 2016/Windows 10 o versioni successive&Dagger;</span><span class="sxs-lookup"><span data-stu-id="96290-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="96290-123">Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="96290-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="96290-124">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="96290-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="96290-125">Connessione [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="96290-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="96290-126">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="96290-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="96290-127">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="96290-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="96290-128">&Dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="96290-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="96290-129">Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato.</span><span class="sxs-lookup"><span data-stu-id="96290-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="96290-130">Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="96290-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="96290-131">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="96290-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="96290-132">HTTP/2 è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="96290-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="96290-133">Per altre informazioni sulla configurazione, vedere le sezioni [Opzioni Kestrel](#kestrel-options) e [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="96290-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="96290-134">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="96290-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="96290-135">È possibile usare Kestrel da solo o in combinazione con un *server proxy inverso*, ad esempio [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="96290-135">You can use Kestrel by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="96290-136">Il server proxy inverso riceve le richieste HTTP dalla rete e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-136">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="96290-137">Kestrel usato come server Web perimetrale (esposto a Internet):</span><span class="sxs-lookup"><span data-stu-id="96290-137">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="96290-139">Kestrel usato in una configurazione proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="96290-139">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="96290-141">Entrambe le configurazioni &mdash;con o senza server proxy inverso&mdash; sono configurazioni di hosting supportate per le app ASP.NET Core 2.1 o versioni successive che ricevono richieste da Internet.</span><span class="sxs-lookup"><span data-stu-id="96290-141">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.1 or later apps that receive requests from the Internet.</span></span>

<span data-ttu-id="96290-142">Se viene utilizzato come server perimetrale in assenza di un server proxy inverso, Kestrel non supporta la condivisione tra più processi dell'IP e della porta.</span><span class="sxs-lookup"><span data-stu-id="96290-142">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="96290-143">Quando è configurato per restare in ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dalle intestazioni `Host` delle richieste.</span><span class="sxs-lookup"><span data-stu-id="96290-143">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="96290-144">Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="96290-144">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="96290-145">Anche se la presenza di un server proxy inverso non è necessaria, può risultare utile per i motivi seguenti.</span><span class="sxs-lookup"><span data-stu-id="96290-145">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="96290-146">Un proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="96290-146">A reverse proxy:</span></span>

* <span data-ttu-id="96290-147">Può limitare l'area della superficie di attacco pubblica esposta delle app ospitate.</span><span class="sxs-lookup"><span data-stu-id="96290-147">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="96290-148">Offre un livello aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="96290-148">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="96290-149">Potrebbe offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="96290-149">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="96290-150">Semplifica il bilanciamento del carico e la configurazione di comunicazioni protette (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="96290-150">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="96290-151">Solo il server proxy inverso richiede un certificato X.509 e tale server può comunicare con i server delle applicazioni sulla rete interna mediante il protocollo HTTP semplice.</span><span class="sxs-lookup"><span data-stu-id="96290-151">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="96290-152">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="96290-152">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="96290-153">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96290-153">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="96290-154">Il pacchetto [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="96290-154">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="96290-155">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="96290-155">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="96290-156">In *Program.cs* il codice modello chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), che a sua volta chiama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) in background.</span><span class="sxs-lookup"><span data-stu-id="96290-156">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96290-157">Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, usare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="96290-157">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

<span data-ttu-id="96290-158">Se l'app non chiama `CreateDefaultBuilder` per configurare l'host, chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **prima** di chiamare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="96290-158">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="96290-159">Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, chiamare [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="96290-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="96290-160">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="96290-160">Kestrel options</span></span>

<span data-ttu-id="96290-161">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="96290-161">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="96290-162">Impostare i vincoli nella proprietà [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) della classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="96290-162">Set constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="96290-163">La proprietà `Limits` contiene un'istanza della classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="96290-163">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="96290-164">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="96290-164">Maximum client connections</span></span>

[<span data-ttu-id="96290-165">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="96290-165">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="96290-166">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="96290-166">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="96290-167">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="96290-167">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="96290-168">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="96290-168">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="96290-169">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="96290-169">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="96290-170">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="96290-170">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="96290-171">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="96290-171">Maximum request body size</span></span>

[<span data-ttu-id="96290-172">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="96290-172">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="96290-173">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="96290-173">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="96290-174">Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="96290-174">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="96290-175">L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="96290-175">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="96290-176">È possibile eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="96290-176">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

<span data-ttu-id="96290-177">Se si tenta di configurare il limite per una richiesta dopo che l'app ha avviato la lettura della richiesta stessa, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="96290-177">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="96290-178">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="96290-178">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="96290-179">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="96290-179">Minimum request body data rate</span></span>

[<span data-ttu-id="96290-180">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="96290-180">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="96290-181">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="96290-181">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="96290-182">Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="96290-182">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="96290-183">Se la velocità scende sotto il valore minimo, la connessione raggiunge il timeout. Il periodo di tolleranza è la quantità di tempo che Kestrel concede al client per aumentare la velocità di trasmissione fino al valore minimo. Durante tale periodo la velocità non viene rilevata.</span><span class="sxs-lookup"><span data-stu-id="96290-183">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="96290-184">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="96290-184">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="96290-185">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="96290-185">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="96290-186">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="96290-186">A minimum rate also applies to the response.</span></span> <span data-ttu-id="96290-187">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="96290-187">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="96290-188">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="96290-188">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

<span data-ttu-id="96290-189">È possibile eseguire l'override dei limiti di velocità minima per ogni richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="96290-189">You can override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96290-190">Nessuna delle due funzionalità relative alla velocità, a cui si fa riferimento nell'esempio precedente, è presente in `HttpContext.Features` per le richieste HTTP/2 perché la modifica dei limiti di velocità per ogni richiesta non è supportata per HTTP/2 a causa del supporto del protocollo per il multiplexing delle richieste.</span><span class="sxs-lookup"><span data-stu-id="96290-190">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="96290-191">I limiti di velocità a livello di server configurati tramite `KestrelServerOptions.Limits` vengono tuttavia applicati alle connessioni HTTP/1.x e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="96290-191">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="96290-192">Numero massimo di flussi per connessione</span><span class="sxs-lookup"><span data-stu-id="96290-192">Maximum streams per connection</span></span>

<span data-ttu-id="96290-193">`Http2.MaxStreamsPerConnection` limita il numero di flussi di richieste simultanee per ogni connessione HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="96290-193">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="96290-194">I flussi in eccesso vengono rifiutati.</span><span class="sxs-lookup"><span data-stu-id="96290-194">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="96290-195">Il valore predefinito è 100.</span><span class="sxs-lookup"><span data-stu-id="96290-195">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="96290-196">Dimensioni della tabella delle intestazioni</span><span class="sxs-lookup"><span data-stu-id="96290-196">Header table size</span></span>

<span data-ttu-id="96290-197">Il decodificatore HPACK decomprime le intestazioni HTTP per le connessioni HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="96290-197">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="96290-198">`Http2.HeaderTableSize` limita le dimensioni della tabella di compressione delle intestazioni usata dal decodificatore HPACK.</span><span class="sxs-lookup"><span data-stu-id="96290-198">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="96290-199">Il valore viene specificato in ottetti e deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="96290-199">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="96290-200">Il valore predefinito è 4096.</span><span class="sxs-lookup"><span data-stu-id="96290-200">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="96290-201">Dimensione massima del frame</span><span class="sxs-lookup"><span data-stu-id="96290-201">Maximum frame size</span></span>

<span data-ttu-id="96290-202">`Http2.MaxFrameSize` indica le dimensioni massime del payload del frame di connessione HTTP/2 da ricevere.</span><span class="sxs-lookup"><span data-stu-id="96290-202">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="96290-203">Il valore viene specificato in ottetti e deve essere compreso tra 2^14 (16.384) e 2^24-1 (16.777.215).</span><span class="sxs-lookup"><span data-stu-id="96290-203">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="96290-204">Il valore predefinito è 2^14 (16.384).</span><span class="sxs-lookup"><span data-stu-id="96290-204">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="96290-205">Dimensioni massime dell'intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="96290-205">Maximum request header size</span></span>

<span data-ttu-id="96290-206">`Http2.MaxRequestHeaderFieldSize` indica le dimensioni massime consentite in ottetti di valori di intestazione di richiesta.</span><span class="sxs-lookup"><span data-stu-id="96290-206">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="96290-207">Questo limite si applica insieme sia al nome che al valore nelle rappresentazioni compresse e non compresse.</span><span class="sxs-lookup"><span data-stu-id="96290-207">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="96290-208">Il valore deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="96290-208">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="96290-209">Il valore predefinito è 8.192.</span><span class="sxs-lookup"><span data-stu-id="96290-209">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="96290-210">Dimensioni della finestra di connessione iniziali</span><span class="sxs-lookup"><span data-stu-id="96290-210">Initial connection window size</span></span>

<span data-ttu-id="96290-211">`Http2.InitialConnectionWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta, aggregati in tutte le richieste (flussi), per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="96290-211">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="96290-212">Le richieste sono anche limitate da `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="96290-212">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="96290-213">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="96290-213">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="96290-214">Il valore predefinito è 128 KB (131.072).</span><span class="sxs-lookup"><span data-stu-id="96290-214">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="96290-215">Dimensioni della finestra di flusso iniziali</span><span class="sxs-lookup"><span data-stu-id="96290-215">Initial stream window size</span></span>

<span data-ttu-id="96290-216">`Http2.InitialStreamWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta per ogni richiesta (flusso).</span><span class="sxs-lookup"><span data-stu-id="96290-216">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="96290-217">Le richieste sono anche limitate da `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="96290-217">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="96290-218">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="96290-218">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="96290-219">Il valore predefinito è 96 KB (98.304).</span><span class="sxs-lookup"><span data-stu-id="96290-219">The default value is 96 KB (98,304).</span></span>

::: moniker-end

<span data-ttu-id="96290-220">Per informazioni su altre opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="96290-220">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="96290-221">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="96290-221">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="96290-222">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="96290-222">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="96290-223">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="96290-223">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

## <a name="endpoint-configuration"></a><span data-ttu-id="96290-224">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="96290-224">Endpoint configuration</span></span>

<span data-ttu-id="96290-225">Per impostazione predefinita, ASP.NET Core è associato a:</span><span class="sxs-lookup"><span data-stu-id="96290-225">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="96290-226">`https://localhost:5001` (quando è presente un certificato di sviluppo locale)</span><span class="sxs-lookup"><span data-stu-id="96290-226">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="96290-227">Viene creato un certificato di sviluppo nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-227">A development certificate is created:</span></span>

* <span data-ttu-id="96290-228">Quando viene installato [.NET Core SDK](/dotnet/core/sdk).</span><span class="sxs-lookup"><span data-stu-id="96290-228">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="96290-229">Quando per creare un certificato viene usato [lo strumento dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="96290-229">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="96290-230">Alcuni browser richiedono un'autorizzazione esplicita per considerare attendibile il certificato di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="96290-230">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="96290-231">ASP.NET Core 2.1 e le versioni successive dei modelli di progetto configurano le app da eseguire su HTTPS per impostazione predefinita e includono [il reindirizzamento a HTTPS e il supporto di HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="96290-231">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="96290-232">Chiamare i metodi [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oppure [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) su [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) per configurare i prefissi URL e le porte per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-232">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="96290-233">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).</span><span class="sxs-lookup"><span data-stu-id="96290-233">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="96290-234">Configurazione di `KestrelServerOptions` in ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="96290-234">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="96290-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="96290-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="96290-236">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato.</span><span class="sxs-lookup"><span data-stu-id="96290-236">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="96290-237">Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="96290-237">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="96290-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="96290-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="96290-239">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="96290-239">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="96290-240">Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="96290-240">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="96290-241">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="96290-241">Configure(IConfiguration)</span></span>

<span data-ttu-id="96290-242">Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) come input.</span><span class="sxs-lookup"><span data-stu-id="96290-242">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="96290-243">L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-243">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="96290-244">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="96290-244">ListenOptions.UseHttps</span></span>

<span data-ttu-id="96290-245">Configura Kestrel per l'uso di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="96290-245">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="96290-246">Estensioni `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="96290-246">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="96290-247">`UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="96290-247">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="96290-248">Genera un'eccezione se non è stato configurato alcun certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="96290-248">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="96290-249">Parametri `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="96290-249">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="96290-250">`filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="96290-250">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="96290-251">`password` è la password richiesta per accedere ai dati del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="96290-251">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="96290-252">`configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="96290-252">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="96290-253">Restituisce `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="96290-253">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="96290-254">`storeName` è l'archivio certificati da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="96290-254">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="96290-255">`subject` è il nome dell'oggetto del certificato.</span><span class="sxs-lookup"><span data-stu-id="96290-255">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="96290-256">`allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="96290-256">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="96290-257">`location` è il percorso dell'archivio da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="96290-257">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="96290-258">`serverCertificate` è il certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="96290-258">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="96290-259">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="96290-259">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="96290-260">Come minimo, è necessario specificare un certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="96290-260">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="96290-261">Configurazioni supportate descritte in seguito:</span><span class="sxs-lookup"><span data-stu-id="96290-261">Supported configurations described next:</span></span>

* <span data-ttu-id="96290-262">Nessuna configurazione</span><span class="sxs-lookup"><span data-stu-id="96290-262">No configuration</span></span>
* <span data-ttu-id="96290-263">Sostituire il certificato predefinito della configurazione</span><span class="sxs-lookup"><span data-stu-id="96290-263">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="96290-264">Modificare le impostazioni predefinite nel codice</span><span class="sxs-lookup"><span data-stu-id="96290-264">Change the defaults in code</span></span>

<span data-ttu-id="96290-265">*Nessuna configurazione*</span><span class="sxs-lookup"><span data-stu-id="96290-265">*No configuration*</span></span>

<span data-ttu-id="96290-266">Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="96290-266">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="96290-267">Specificare gli URL usando gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-267">Specify URLs using the:</span></span>

* <span data-ttu-id="96290-268">La variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="96290-268">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="96290-269">L'argomento della riga di comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="96290-269">`--urls` command-line argument.</span></span>
* <span data-ttu-id="96290-270">La chiave di configurazione dell'host `urls`.</span><span class="sxs-lookup"><span data-stu-id="96290-270">`urls` host configuration key.</span></span>
* <span data-ttu-id="96290-271">Il metodo di estensione `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="96290-271">`UseUrls` extension method.</span></span>

<span data-ttu-id="96290-272">Per altre informazioni, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="96290-272">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="96290-273">Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="96290-273">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="96290-274">Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="96290-274">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="96290-275">*Sostituire il certificato predefinito della configurazione*</span><span class="sxs-lookup"><span data-stu-id="96290-275">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="96290-276">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-276">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="96290-277">È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito.</span><span class="sxs-lookup"><span data-stu-id="96290-277">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="96290-278">Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="96290-278">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="96290-279">Nel file *appsettings.json* di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="96290-279">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="96290-280">Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="96290-280">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="96290-281">Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default** o il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="96290-281">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="96290-282">Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="96290-282">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="96290-283">Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:</span><span class="sxs-lookup"><span data-stu-id="96290-283">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="96290-284">Note di schema:</span><span class="sxs-lookup"><span data-stu-id="96290-284">Schema notes:</span></span>

* <span data-ttu-id="96290-285">I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="96290-285">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="96290-286">Ad esempio, sono validi sia `HTTPS` che `Https`.</span><span class="sxs-lookup"><span data-stu-id="96290-286">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="96290-287">Il parametro `Url` è obbligatorio per ogni endpoint.</span><span class="sxs-lookup"><span data-stu-id="96290-287">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="96290-288">Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.</span><span class="sxs-lookup"><span data-stu-id="96290-288">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="96290-289">Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono.</span><span class="sxs-lookup"><span data-stu-id="96290-289">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="96290-290">Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="96290-290">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="96290-291">La sezione `Certificate` è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="96290-291">The `Certificate` section is optional.</span></span> <span data-ttu-id="96290-292">Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti.</span><span class="sxs-lookup"><span data-stu-id="96290-292">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="96290-293">Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="96290-293">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="96290-294">La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="96290-294">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="96290-295">In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.</span><span class="sxs-lookup"><span data-stu-id="96290-295">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="96290-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, options => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:</span><span class="sxs-lookup"><span data-stu-id="96290-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="96290-297">È anche possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per mantenere l'iterazione nel caricatore esistente, ad esempio quello specificato da [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="96290-297">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="96290-298">La sezione di configurazione per ogni endpoint è disponibile sulle opzioni nel metodo `Endpoint` in modo che le impostazioni personalizzate possano essere lette.</span><span class="sxs-lookup"><span data-stu-id="96290-298">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="96290-299">È possibile caricare più configurazioni chiamando ancora `options.Configure(context.Configuration.GetSection("Kestrel"))` con un'altra sezione.</span><span class="sxs-lookup"><span data-stu-id="96290-299">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="96290-300">Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti.</span><span class="sxs-lookup"><span data-stu-id="96290-300">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="96290-301">Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.</span><span class="sxs-lookup"><span data-stu-id="96290-301">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="96290-302">`KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="96290-302">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="96290-303">Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.</span><span class="sxs-lookup"><span data-stu-id="96290-303">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="96290-304">*Modificare le impostazioni predefinite nel codice*</span><span class="sxs-lookup"><span data-stu-id="96290-304">*Change the defaults in code*</span></span>

<span data-ttu-id="96290-305">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente.</span><span class="sxs-lookup"><span data-stu-id="96290-305">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="96290-306">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.</span><span class="sxs-lookup"><span data-stu-id="96290-306">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="96290-307">*Supporto kestrel per SNI*</span><span class="sxs-lookup"><span data-stu-id="96290-307">*Kestrel support for SNI*</span></span>

<span data-ttu-id="96290-308">[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="96290-308">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="96290-309">Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto.</span><span class="sxs-lookup"><span data-stu-id="96290-309">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="96290-310">Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="96290-310">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="96290-311">Kestrel supporta SNI tramite il callback `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="96290-311">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="96290-312">Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="96290-312">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="96290-313">Il supporto SNI richiede:</span><span class="sxs-lookup"><span data-stu-id="96290-313">SNI support requires:</span></span>

* <span data-ttu-id="96290-314">Esecuzione in `netcoreapp2.1` del framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="96290-314">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="96290-315">In `netcoreapp2.0` e `net461` il callback viene richiamato, ma l'elemento `name` è sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="96290-315">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="96290-316">L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="96290-316">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="96290-317">Esecuzione di tutti i siti Web nella stessa istanza di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-317">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="96290-318">Kestrel supporta la condivisione di un indirizzo IP e di una porta tra più istanze solo con un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="96290-318">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
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
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
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

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
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
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
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
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="96290-319">Associazione a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="96290-319">Bind to a TCP socket</span></span>

<span data-ttu-id="96290-320">Il metodo [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) esegue l'associazione a un socket TCP. Un'espressione lambda per le opzioni consente di configurare un certificato X.509:</span><span class="sxs-lookup"><span data-stu-id="96290-320">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

<span data-ttu-id="96290-321">L'esempio configura HTTPS per un endpoint con [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="96290-321">The example configures HTTPS for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="96290-322">Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="96290-322">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="96290-323">Associazione a un socket Unix</span><span class="sxs-lookup"><span data-stu-id="96290-323">Bind to a Unix socket</span></span>

<span data-ttu-id="96290-324">Impostare l'ascolto su un socket Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="96290-324">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a><span data-ttu-id="96290-325">Porta 0</span><span class="sxs-lookup"><span data-stu-id="96290-325">Port 0</span></span>

<span data-ttu-id="96290-326">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="96290-326">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="96290-327">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="96290-327">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="96290-328">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="96290-328">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="96290-329">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="96290-329">Limitations</span></span>

<span data-ttu-id="96290-330">Configurare gli endpoint con i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-330">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="96290-331">UseUrls</span><span class="sxs-lookup"><span data-stu-id="96290-331">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="96290-332">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="96290-332">`--urls` command-line argument</span></span>
* <span data-ttu-id="96290-333">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="96290-333">`urls` host configuration key</span></span>
* <span data-ttu-id="96290-334">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="96290-334">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="96290-335">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-335">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="96290-336">Tenere presenti, tuttavia, le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-336">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="96290-337">HTTPS non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="96290-337">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="96290-338">Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="96290-338">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="96290-339">Configurazione dell'endpoint IIS</span><span class="sxs-lookup"><span data-stu-id="96290-339">IIS endpoint configuration</span></span>

<span data-ttu-id="96290-340">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="96290-340">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="96290-341">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="96290-341">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="96290-342">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="96290-342">ListenOptions.Protocols</span></span>

<span data-ttu-id="96290-343">La proprietà `Protocols` stabilisce i protocolli HTTP (`HttpProtocols`) abilitati in un endpoint di connessione o per il server.</span><span class="sxs-lookup"><span data-stu-id="96290-343">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="96290-344">Assegnare un valore alla proprietà `Protocols` dall'enumerazione `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="96290-344">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="96290-345">Valore di enumerazione `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="96290-345">`HttpProtocols` enum value</span></span> | <span data-ttu-id="96290-346">Protocollo di connessione consentito</span><span class="sxs-lookup"><span data-stu-id="96290-346">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="96290-347">Solo HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="96290-347">HTTP/1.1 only.</span></span> <span data-ttu-id="96290-348">Può essere usato con o senza TLS.</span><span class="sxs-lookup"><span data-stu-id="96290-348">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="96290-349">Solo HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="96290-349">HTTP/2 only.</span></span> <span data-ttu-id="96290-350">Viene essenzialmente usato con TLS.</span><span class="sxs-lookup"><span data-stu-id="96290-350">Primarily used with TLS.</span></span> <span data-ttu-id="96290-351">Può essere usato senza TLS solo se il client supporta una [modalità di conoscenza pregressa](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="96290-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="96290-352">HTTP/1.1 e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="96290-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="96290-353">È necessaria una connessione TLS e [ALPN (Application Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) per negoziare HTTP/2; in caso contrario, la connessione viene automaticamente impostata su HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="96290-353">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="96290-354">Il protocollo predefinito è HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="96290-354">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="96290-355">Restrizioni relative a TLS per HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="96290-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="96290-356">TLS versione 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="96290-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="96290-357">Rinegoziazione disabilitata</span><span class="sxs-lookup"><span data-stu-id="96290-357">Renegotiation disabled</span></span>
* <span data-ttu-id="96290-358">Compressione disabilitata</span><span class="sxs-lookup"><span data-stu-id="96290-358">Compression disabled</span></span>
* <span data-ttu-id="96290-359">Dimensioni minime per lo scambio di chiavi temporanee:</span><span class="sxs-lookup"><span data-stu-id="96290-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="96290-360">Diffie-Hellman a curva ellittica (ECDH) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="96290-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="96290-361">Diffie-Hellman a campo finito (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="96290-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="96290-362">Pacchetto di crittografia consentito</span><span class="sxs-lookup"><span data-stu-id="96290-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="96290-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con curva ellittica P-256 &lbrack;`FIPS186`&rbrack; è supportato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="96290-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="96290-364">Nell'esempio seguente sono consentite connessioni HTTP/1.1 e HTTP/2 sulla porta 8000.</span><span class="sxs-lookup"><span data-stu-id="96290-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="96290-365">Le connessioni sono protette da TLS con un certificato incluso:</span><span class="sxs-lookup"><span data-stu-id="96290-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="96290-366">Facoltativamente, creare un'implementazione `IConnectionAdapter` per filtrare gli handshake TLS in base alla connessione in caso di crittografie specifiche:</span><span class="sxs-lookup"><span data-stu-id="96290-366">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        });
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="96290-367">*Impostare il protocollo dalla configurazione*</span><span class="sxs-lookup"><span data-stu-id="96290-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="96290-368">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione Kestrel.</span><span class="sxs-lookup"><span data-stu-id="96290-368">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="96290-369">Nell'esempio *appsettings.json* seguente viene stabilito un protocollo di connessione predefinito (HTTP/1.1 e HTTP/2) per tutti gli endpoint Kestrel:</span><span class="sxs-lookup"><span data-stu-id="96290-369">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="96290-370">Il file di configurazione di esempio seguente stabilisce un protocollo di connessione per un endpoint specifico:</span><span class="sxs-lookup"><span data-stu-id="96290-370">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="96290-371">I protocolli specificati nei valori di override del codice sono impostati dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="96290-371">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

## <a name="transport-configuration"></a><span data-ttu-id="96290-372">Configurazione del trasporto</span><span class="sxs-lookup"><span data-stu-id="96290-372">Transport configuration</span></span>

<span data-ttu-id="96290-373">Con la versione ASP.NET Core 2.1, il trasporto predefinito di Kestrel non si basa più su Libuv, ma su socket gestiti.</span><span class="sxs-lookup"><span data-stu-id="96290-373">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="96290-374">Si tratta di una modifica importante per le app ASP.NET 2.0 Core che vengono aggiornate alla versione 2.1, che chiamano [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) e dipendono da uno dei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="96290-374">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="96290-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (riferimento diretto al pacchetto)</span><span class="sxs-lookup"><span data-stu-id="96290-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="96290-376">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="96290-376">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="96290-377">Per i progetti ASP.NET Core 2.1 o versioni successive che usano il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e richiedono l'uso di Libuv:</span><span class="sxs-lookup"><span data-stu-id="96290-377">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="96290-378">Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="96290-378">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="96290-379">Chiamare [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="96290-379">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="96290-380">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="96290-380">URL prefixes</span></span>

<span data-ttu-id="96290-381">Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="96290-381">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="96290-382">Sono validi solo i prefissi URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="96290-382">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="96290-383">Kestrel non supporta HTTPS quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="96290-383">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="96290-384">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="96290-384">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="96290-385">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="96290-385">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="96290-386">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="96290-386">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="96290-387">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="96290-387">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="96290-388">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="96290-388">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="96290-389">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="96290-389">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="96290-390">Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="96290-390">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="96290-391">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="96290-391">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="96290-392">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="96290-392">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="96290-393">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="96290-393">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="96290-394">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="96290-394">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="96290-395">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="96290-395">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="96290-396">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="96290-396">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="96290-397">Filtro host</span><span class="sxs-lookup"><span data-stu-id="96290-397">Host filtering</span></span>

<span data-ttu-id="96290-398">Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host.</span><span class="sxs-lookup"><span data-stu-id="96290-398">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="96290-399">L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback.</span><span class="sxs-lookup"><span data-stu-id="96290-399">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="96290-400">Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici.</span><span class="sxs-lookup"><span data-stu-id="96290-400">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="96290-401">Le intestazioni `Host` non vengono convalidate.</span><span class="sxs-lookup"><span data-stu-id="96290-401">`Host` headers aren't validated.</span></span>

<span data-ttu-id="96290-402">Come soluzione alternativa, usare il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="96290-402">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="96290-403">Il middleware di filtro host è disponibile nel pacchetto [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="96290-403">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="96290-404">Il middleware viene aggiunto da [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), che chiama [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="96290-404">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="96290-405">Per impostazione predefinita, il middleware di filtro host è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="96290-405">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="96290-406">Per abilitare il middleware, definire una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*.</span><span class="sxs-lookup"><span data-stu-id="96290-406">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="96290-407">Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:</span><span class="sxs-lookup"><span data-stu-id="96290-407">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="96290-408">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96290-408">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="96290-409">Anche il [middleware di intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) ha un'opzione [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="96290-409">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="96290-410">Il middleware di intestazioni inoltrate e il middleware di filtro host hanno funzionalità simili per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="96290-410">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="96290-411">L'impostazione di `AllowedHosts` con il middleware delle intestazioni inoltrate è appropriato se l'intestazione `Host` non viene mantenuta durante l'inoltro delle richieste con un server proxy inverso o il bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="96290-411">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="96290-412">L'impostazione di `AllowedHosts` con il middleware del filtro host è appropriata se si usa Kestrel come server perimetrale rivolto al pubblico o se l'intestazione `Host` viene inoltrata direttamente.</span><span class="sxs-lookup"><span data-stu-id="96290-412">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="96290-413">Per altre informazioni sul middleware delle intestazioni inoltrate, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="96290-413">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96290-414">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="96290-414">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="96290-415">Codice sorgente di Kestrel</span><span class="sxs-lookup"><span data-stu-id="96290-415">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="96290-416">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span><span class="sxs-lookup"><span data-stu-id="96290-416">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
