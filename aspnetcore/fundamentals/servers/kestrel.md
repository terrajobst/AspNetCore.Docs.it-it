---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: guardrex
description: Informazioni su Kestrel, il server Web multipiattaforma per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: beaf6ac49359adfdc2dc24221eab04cc853646a9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143437"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="098d7-103">Implementazione del server Web Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="098d7-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="098d7-104">Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73)e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="098d7-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="098d7-105">Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="098d7-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="098d7-106">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="098d7-107">Kestrel supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="098d7-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="098d7-108">HTTPS</span></span>
* <span data-ttu-id="098d7-109">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="098d7-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="098d7-110">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="098d7-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="098d7-111">HTTP/2 (tranne che in macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="098d7-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="098d7-112">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="098d7-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="098d7-113">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="098d7-114">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="098d7-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="098d7-115">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="098d7-115">HTTP/2 support</span></span>

<span data-ttu-id="098d7-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è disponibile per le app ASP.NET Core se vengono soddisfatti i requisiti di base seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="098d7-117">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="098d7-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="098d7-118">Windows Server 2016/Windows 10 o versioni successive&Dagger;</span><span class="sxs-lookup"><span data-stu-id="098d7-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="098d7-119">Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="098d7-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="098d7-120">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="098d7-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="098d7-121">Connessione [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="098d7-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="098d7-122">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="098d7-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="098d7-123">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="098d7-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="098d7-124">&Dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="098d7-125">Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato.</span><span class="sxs-lookup"><span data-stu-id="098d7-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="098d7-126">Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="098d7-127">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="098d7-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="098d7-128">HTTP/2 è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="098d7-129">Per altre informazioni sulla configurazione, vedere le sezioni [Opzioni Kestrel](#kestrel-options) e [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="098d7-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="098d7-130">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="098d7-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="098d7-131">È possibile usare Kestrel da solo o in combinazione con un *server proxy inverso*, ad esempio [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="098d7-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="098d7-132">Il server proxy inverso riceve le richieste HTTP dalla rete e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="098d7-133">Kestrel usato come server Web perimetrale (esposto a Internet):</span><span class="sxs-lookup"><span data-stu-id="098d7-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="098d7-135">Kestrel usato in una configurazione proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="098d7-137">La configurazione, con o senza un server proxy inverso, è una configurazione di hosting supportata.</span><span class="sxs-lookup"><span data-stu-id="098d7-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="098d7-138">Se viene utilizzato come server perimetrale in assenza di un server proxy inverso, Kestrel non supporta la condivisione tra più processi dell'IP e della porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="098d7-139">Quando è configurato per restare in ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dalle intestazioni `Host` delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="098d7-140">Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="098d7-141">Anche se la presenza di un server proxy inverso non è necessaria, può risultare utile per i motivi seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="098d7-142">Un proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-142">A reverse proxy:</span></span>

* <span data-ttu-id="098d7-143">Può limitare l'area della superficie di attacco pubblica esposta delle app ospitate.</span><span class="sxs-lookup"><span data-stu-id="098d7-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="098d7-144">Offre un livello aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="098d7-145">Potrebbe offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="098d7-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="098d7-146">Semplifica il bilanciamento del carico e la configurazione di comunicazioni protette (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="098d7-147">Solo il server proxy inverso richiede un certificato X. 509 e il server è in grado di comunicare con i server dell'app nella rete interna tramite HTTP normale.</span><span class="sxs-lookup"><span data-stu-id="098d7-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-148">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="098d7-149">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="098d7-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="098d7-150">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="098d7-151">In *Program.cs* il codice del modello chiama <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> in background.</span><span class="sxs-lookup"><span data-stu-id="098d7-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="098d7-152">Per ulteriori informazioni su `CreateDefaultBuilder` e sulla compilazione dell'host, vedere le sezioni configurare le impostazioni di *un host* e di un *generatore predefinito* di <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="098d7-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="098d7-153">Per fornire una configurazione aggiuntiva dopo aver chiamato `CreateDefaultBuilder` e `ConfigureWebHostDefaults`, utilizzare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="098d7-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="098d7-154">Se l'app non chiama `CreateDefaultBuilder` per configurare l'host, chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **prima** di chiamare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="098d7-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="098d7-155">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="098d7-155">Kestrel options</span></span>

<span data-ttu-id="098d7-156">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="098d7-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="098d7-157">Impostare i vincoli per la proprietà <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="098d7-158">La proprietà `Limits` contiene un'istanza della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="098d7-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="098d7-159">Negli esempi seguenti viene usato lo spazio dei nomi <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="098d7-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="098d7-160">Le opzioni di gheppio, che sono C# configurate nel codice negli esempi seguenti, possono essere impostate anche usando un [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="098d7-161">Il provider di configurazione file, ad esempio, può caricare la configurazione di Gheppio da *appSettings. JSON* o *appSettings. { File Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="098d7-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="098d7-162">Usare **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="098d7-163">Configurare gheppio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="098d7-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="098d7-164">Inserire un'istanza di `IConfiguration` nella classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="098d7-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="098d7-165">Nell'esempio seguente si presuppone che la configurazione inserita venga assegnata alla proprietà `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="098d7-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="098d7-166">In `Startup.ConfigureServices`, caricare la sezione `Kestrel` della configurazione nella configurazione di gheppio.</span><span class="sxs-lookup"><span data-stu-id="098d7-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="098d7-167">Configurare il gheppio durante la compilazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="098d7-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="098d7-168">In *Program.cs*caricare la sezione `Kestrel` della configurazione nella configurazione di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="098d7-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="098d7-169">Entrambi gli approcci precedenti funzionano con qualsiasi [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="098d7-170">Timeout keep-alive</span><span class="sxs-lookup"><span data-stu-id="098d7-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="098d7-171">Ottiene o imposta il [timeout keep-alive](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="098d7-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="098d7-172">Il valore predefinito è 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="098d7-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="098d7-173">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="098d7-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="098d7-174">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="098d7-175">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="098d7-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="098d7-176">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="098d7-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="098d7-177">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="098d7-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="098d7-178">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="098d7-179">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="098d7-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="098d7-180">Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="098d7-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="098d7-181">L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="098d7-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="098d7-182">Eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="098d7-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="098d7-183">Viene generata un'eccezione se l'app configura il limite per una richiesta dopo che l'app ha iniziato a leggere la richiesta.</span><span class="sxs-lookup"><span data-stu-id="098d7-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="098d7-184">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="098d7-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="098d7-185">Quando un'app viene eseguita [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) dietro il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), il limite della dimensione del corpo della richiesta di Kestrel è disabilitato perché viene già impostato da IIS.</span><span class="sxs-lookup"><span data-stu-id="098d7-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="098d7-186">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="098d7-187">Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="098d7-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="098d7-188">Se la frequenza scende sotto il valore minimo, si è verificato il timeout della connessione. Il periodo di tolleranza è la quantità di tempo che il gheppio concede al client di aumentare la velocità di invio fino al minimo. la frequenza non viene controllata durante tale periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="098d7-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="098d7-189">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="098d7-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="098d7-190">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="098d7-191">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="098d7-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="098d7-192">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="098d7-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="098d7-193">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="098d7-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="098d7-194">Ignorare i limiti di velocità minima per ogni richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="098d7-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="098d7-195">La funzionalità <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> a cui si fa riferimento nell'esempio precedente non è presente in `HttpContext.Features` per le richieste HTTP/2, perché la modifica dei limiti di velocità per ogni richiesta in genere non è supportata per HTTP/2 a causa del supporto del protocollo per il multiplexing delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="098d7-196">La funzionalità <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> è tuttavia ancora presente in `HttpContext.Features` per le richieste HTTP/2, perché il limite di velocità di lettura può comunque essere *disabilitato interamente* per ogni singola richiesta impostando `IHttpMinRequestBodyDataRateFeature.MinDataRate` su `null` anche per una richiesta HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="098d7-197">Il tentativo di leggere `IHttpMinRequestBodyDataRateFeature.MinDataRate` o il tentativo di impostazione di un valore diverso da `null` causerà la generazione di una `NotSupportedException` per una richiesta HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="098d7-198">I limiti di velocità a livello di server configurati tramite `KestrelServerOptions.Limits` vengono tuttavia applicati alle connessioni HTTP/1.x e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="098d7-199">Timeout delle intestazioni delle richieste</span><span class="sxs-lookup"><span data-stu-id="098d7-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="098d7-200">Ottiene o imposta la quantità massima di tempo che il server dedica alla ricezione delle intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="098d7-201">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="098d7-202">Numero massimo di flussi per connessione</span><span class="sxs-lookup"><span data-stu-id="098d7-202">Maximum streams per connection</span></span>

<span data-ttu-id="098d7-203">`Http2.MaxStreamsPerConnection` limita il numero di flussi di richieste simultanee per ogni connessione HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="098d7-204">I flussi in eccesso vengono rifiutati.</span><span class="sxs-lookup"><span data-stu-id="098d7-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="098d7-205">Il valore predefinito è 100.</span><span class="sxs-lookup"><span data-stu-id="098d7-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="098d7-206">Dimensioni della tabella delle intestazioni</span><span class="sxs-lookup"><span data-stu-id="098d7-206">Header table size</span></span>

<span data-ttu-id="098d7-207">Il decodificatore HPACK decomprime le intestazioni HTTP per le connessioni HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="098d7-208">`Http2.HeaderTableSize` limita le dimensioni della tabella di compressione delle intestazioni usata dal decodificatore HPACK.</span><span class="sxs-lookup"><span data-stu-id="098d7-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="098d7-209">Il valore viene specificato in ottetti e deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="098d7-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="098d7-210">Il valore predefinito è 4096.</span><span class="sxs-lookup"><span data-stu-id="098d7-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="098d7-211">Dimensione massima del frame</span><span class="sxs-lookup"><span data-stu-id="098d7-211">Maximum frame size</span></span>

<span data-ttu-id="098d7-212">`Http2.MaxFrameSize` indica la dimensione massima consentita di un payload del frame di connessione HTTP/2 ricevuto o inviato dal server.</span><span class="sxs-lookup"><span data-stu-id="098d7-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="098d7-213">Il valore viene specificato in ottetti e deve essere compreso tra 2^14 (16.384) e 2^24-1 (16.777.215).</span><span class="sxs-lookup"><span data-stu-id="098d7-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="098d7-214">Il valore predefinito è 2^14 (16.384).</span><span class="sxs-lookup"><span data-stu-id="098d7-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="098d7-215">Dimensioni massime dell'intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-215">Maximum request header size</span></span>

<span data-ttu-id="098d7-216">`Http2.MaxRequestHeaderFieldSize` indica le dimensioni massime consentite in ottetti di valori di intestazione di richiesta.</span><span class="sxs-lookup"><span data-stu-id="098d7-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="098d7-217">Questo limite si applica sia al nome che al valore nelle rappresentazioni compresse e non compresse.</span><span class="sxs-lookup"><span data-stu-id="098d7-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="098d7-218">Il valore deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="098d7-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="098d7-219">Il valore predefinito è 8.192.</span><span class="sxs-lookup"><span data-stu-id="098d7-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="098d7-220">Dimensioni della finestra di connessione iniziali</span><span class="sxs-lookup"><span data-stu-id="098d7-220">Initial connection window size</span></span>

<span data-ttu-id="098d7-221">`Http2.InitialConnectionWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta, aggregati in tutte le richieste (flussi), per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="098d7-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="098d7-222">Le richieste sono anche limitate da `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="098d7-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="098d7-223">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="098d7-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="098d7-224">Il valore predefinito è 128 KB (131.072).</span><span class="sxs-lookup"><span data-stu-id="098d7-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="098d7-225">Dimensioni della finestra di flusso iniziali</span><span class="sxs-lookup"><span data-stu-id="098d7-225">Initial stream window size</span></span>

<span data-ttu-id="098d7-226">`Http2.InitialStreamWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta per ogni richiesta (flusso).</span><span class="sxs-lookup"><span data-stu-id="098d7-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="098d7-227">Le richieste sono anche limitate da `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="098d7-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="098d7-228">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="098d7-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="098d7-229">Il valore predefinito è 96 KB (98.304).</span><span class="sxs-lookup"><span data-stu-id="098d7-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="098d7-230">I/O sincrono</span><span class="sxs-lookup"><span data-stu-id="098d7-230">Synchronous IO</span></span>

<span data-ttu-id="098d7-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controlla se l'I/O sincrono è consentito per la richiesta e la risposta.</span><span class="sxs-lookup"><span data-stu-id="098d7-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="098d7-232">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="098d7-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-233">Un numero elevato di operazioni di I/O sincrone bloccanti può portare alla scadenza del pool di thread, a causa della quale l'app smette di rispondere.</span><span class="sxs-lookup"><span data-stu-id="098d7-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="098d7-234">Abilitare solo `AllowSynchronousIO` quando si usa una libreria che non supporta l'I/O asincrono.</span><span class="sxs-lookup"><span data-stu-id="098d7-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="098d7-235">L'esempio seguente abilita l'I/O sincrono:</span><span class="sxs-lookup"><span data-stu-id="098d7-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="098d7-236">Per informazioni su altre opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="098d7-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="098d7-237">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="098d7-237">Endpoint configuration</span></span>

<span data-ttu-id="098d7-238">Per impostazione predefinita, ASP.NET Core è associato a:</span><span class="sxs-lookup"><span data-stu-id="098d7-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="098d7-239">`https://localhost:5001` (quando è presente un certificato di sviluppo locale)</span><span class="sxs-lookup"><span data-stu-id="098d7-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="098d7-240">Specificare gli URL usando gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-240">Specify URLs using the:</span></span>

* <span data-ttu-id="098d7-241">La variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="098d7-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="098d7-242">L'argomento della riga di comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="098d7-243">La chiave di configurazione dell'host `urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="098d7-244">Il metodo di estensione `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="098d7-245">Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="098d7-246">Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="098d7-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="098d7-247">Per altre informazioni su questi approcci, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="098d7-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="098d7-248">Viene creato un certificato di sviluppo nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-248">A development certificate is created:</span></span>

* <span data-ttu-id="098d7-249">Quando viene installato [.NET Core SDK](/dotnet/core/sdk).</span><span class="sxs-lookup"><span data-stu-id="098d7-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="098d7-250">Quando per creare un certificato viene usato [lo strumento dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="098d7-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="098d7-251">Alcuni browser richiedono la concessione di autorizzazioni esplicite per considerare attendibile il certificato di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="098d7-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="098d7-252">I modelli di progetto consentono di configurare le app per l'esecuzione su HTTPS per impostazione predefinita e includono il [reindirizzamento HTTPS e il supporto HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="098d7-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="098d7-253">Chiamare i metodi <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> oppure <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> su <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> per configurare le porte e i prefissi URL per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="098d7-254">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="098d7-255">configurazione `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="098d7-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="098d7-256">ConfigureEndpointDefaults (azione\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="098d7-257">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="098d7-258">Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="098d7-259">ConfigureHttpsDefaults (azione\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="098d7-260">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="098d7-261">Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

### <a name="configureiconfiguration"></a><span data-ttu-id="098d7-262">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="098d7-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="098d7-263">Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento <xref:Microsoft.Extensions.Configuration.IConfiguration> come input.</span><span class="sxs-lookup"><span data-stu-id="098d7-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="098d7-264">L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="098d7-265">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="098d7-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="098d7-266">Configura Kestrel per l'uso di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="098d7-267">Estensioni `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="098d7-268">`UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="098d7-269">Genera un'eccezione se non è stato configurato alcun certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-269">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="098d7-270">Parametri `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="098d7-271">`filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="098d7-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="098d7-272">`password` è la password richiesta per accedere ai dati del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="098d7-273">`configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="098d7-274">Restituisce `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="098d7-275">`storeName` è l'archivio certificati da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="098d7-276">`subject` è il nome dell'oggetto del certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="098d7-277">`allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="098d7-278">`location` è il percorso dell'archivio da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="098d7-279">`serverCertificate` è il certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="098d7-280">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="098d7-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="098d7-281">Come minimo, è necessario specificare un certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="098d7-282">Configurazioni supportate descritte in seguito:</span><span class="sxs-lookup"><span data-stu-id="098d7-282">Supported configurations described next:</span></span>

* <span data-ttu-id="098d7-283">Nessuna configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-283">No configuration</span></span>
* <span data-ttu-id="098d7-284">Sostituire il certificato predefinito della configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="098d7-285">Modificare le impostazioni predefinite nel codice</span><span class="sxs-lookup"><span data-stu-id="098d7-285">Change the defaults in code</span></span>

<span data-ttu-id="098d7-286">*Nessuna configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-286">*No configuration*</span></span>

<span data-ttu-id="098d7-287">Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="098d7-288">*Sostituire il certificato predefinito della configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="098d7-289">`CreateDefaultBuilder` chiama `Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="098d7-290">È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="098d7-291">Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="098d7-292">Nel file *appsettings.json* di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="098d7-293">Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="098d7-294">Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default**  o il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="098d7-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="098d7-295">Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="098d7-296">Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:</span><span class="sxs-lookup"><span data-stu-id="098d7-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="098d7-297">Note di schema:</span><span class="sxs-lookup"><span data-stu-id="098d7-297">Schema notes:</span></span>

* <span data-ttu-id="098d7-298">I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="098d7-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="098d7-299">Ad esempio, sono validi sia `HTTPS` che `Https`.</span><span class="sxs-lookup"><span data-stu-id="098d7-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="098d7-300">Il parametro `Url` è obbligatorio per ogni endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="098d7-301">Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.</span><span class="sxs-lookup"><span data-stu-id="098d7-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="098d7-302">Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono.</span><span class="sxs-lookup"><span data-stu-id="098d7-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="098d7-303">Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="098d7-304">La sezione `Certificate` è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="098d7-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="098d7-305">Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="098d7-306">Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="098d7-307">La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="098d7-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="098d7-308">In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.</span><span class="sxs-lookup"><span data-stu-id="098d7-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="098d7-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, listenOptions => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:</span><span class="sxs-lookup"><span data-stu-id="098d7-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="098d7-310">è possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per continuare a scorrere il caricatore esistente, ad esempio quello fornito da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="098d7-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="098d7-311">La sezione di configurazione per ogni endpoint è disponibile nelle opzioni del metodo `Endpoint` in modo che sia possibile leggere le impostazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="098d7-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="098d7-312">È possibile caricare più configurazioni chiamando ancora `options.Configure(context.Configuration.GetSection("{SECTION}"))` con un'altra sezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="098d7-313">Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="098d7-314">Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.</span><span class="sxs-lookup"><span data-stu-id="098d7-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="098d7-315">`KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="098d7-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="098d7-316">Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="098d7-317">*Modificare le impostazioni predefinite nel codice*</span><span class="sxs-lookup"><span data-stu-id="098d7-317">*Change the defaults in code*</span></span>

<span data-ttu-id="098d7-318">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente.</span><span class="sxs-lookup"><span data-stu-id="098d7-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="098d7-319">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });

    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.SslProtocols = SslProtocols.Tls12;
    });
});
```

<span data-ttu-id="098d7-320">*Supporto kestrel per SNI*</span><span class="sxs-lookup"><span data-stu-id="098d7-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="098d7-321">[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="098d7-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="098d7-322">Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto.</span><span class="sxs-lookup"><span data-stu-id="098d7-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="098d7-323">Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="098d7-324">Kestrel supporta SNI tramite il callback `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="098d7-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="098d7-325">Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="098d7-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="098d7-326">Il supporto SNI richiede:</span><span class="sxs-lookup"><span data-stu-id="098d7-326">SNI support requires:</span></span>

* <span data-ttu-id="098d7-327">In esecuzione nel Framework di destinazione `netcoreapp2.1` o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="098d7-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="098d7-328">In `net461` o versioni successive, il callback viene richiamato, ma il `name` è sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="098d7-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="098d7-329">L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="098d7-330">Esecuzione di tutti i siti Web nella stessa istanza di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="098d7-331">Kestrel supporta la condivisione di un indirizzo IP e di una porta tra più istanze solo con un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="098d7-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="connection-logging"></a><span data-ttu-id="098d7-332">Registrazione connessione</span><span class="sxs-lookup"><span data-stu-id="098d7-332">Connection logging</span></span>

<span data-ttu-id="098d7-333">Chiamare <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> per creare log a livello di debug per la comunicazione a livello di byte in una connessione.</span><span class="sxs-lookup"><span data-stu-id="098d7-333">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="098d7-334">La registrazione delle connessioni è utile per la risoluzione dei problemi nella comunicazione di basso livello, ad esempio durante la crittografia TLS e dietro i proxy.</span><span class="sxs-lookup"><span data-stu-id="098d7-334">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="098d7-335">Se `UseConnectionLogging` viene posizionata prima `UseHttps`, viene registrato il traffico crittografato.</span><span class="sxs-lookup"><span data-stu-id="098d7-335">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="098d7-336">Se `UseConnectionLogging` viene inserito dopo `UseHttps`, il traffico decrittografato viene registrato.</span><span class="sxs-lookup"><span data-stu-id="098d7-336">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="098d7-337">Associazione a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="098d7-337">Bind to a TCP socket</span></span>

<span data-ttu-id="098d7-338">Il metodo <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> esegue l'associazione a un socket TCP e un'espressione lambda per le opzioni consente di configurare un certificato X.509:</span><span class="sxs-lookup"><span data-stu-id="098d7-338">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="098d7-339">L'esempio configura HTTPS per un endpoint con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-339">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="098d7-340">Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="098d7-340">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="098d7-341">Associazione a un socket Unix</span><span class="sxs-lookup"><span data-stu-id="098d7-341">Bind to a Unix socket</span></span>

<span data-ttu-id="098d7-342">Impostare l'ascolto su un socket Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-342">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="098d7-343">Porta 0</span><span class="sxs-lookup"><span data-stu-id="098d7-343">Port 0</span></span>

<span data-ttu-id="098d7-344">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="098d7-344">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="098d7-345">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="098d7-345">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="098d7-346">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="098d7-346">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="098d7-347">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="098d7-347">Limitations</span></span>

<span data-ttu-id="098d7-348">Configurare gli endpoint con i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-348">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="098d7-349">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-349">`--urls` command-line argument</span></span>
* <span data-ttu-id="098d7-350">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-350">`urls` host configuration key</span></span>
* <span data-ttu-id="098d7-351">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="098d7-351">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="098d7-352">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-352">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="098d7-353">Tenere presenti, tuttavia, le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-353">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="098d7-354">HTTPS non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="098d7-354">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="098d7-355">Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-355">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="098d7-356">Configurazione dell'endpoint IIS</span><span class="sxs-lookup"><span data-stu-id="098d7-356">IIS endpoint configuration</span></span>

<span data-ttu-id="098d7-357">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-357">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="098d7-358">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="098d7-358">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="098d7-359">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="098d7-359">ListenOptions.Protocols</span></span>

<span data-ttu-id="098d7-360">La proprietà `Protocols` stabilisce i protocolli HTTP (`HttpProtocols`) abilitati in un endpoint di connessione o per il server.</span><span class="sxs-lookup"><span data-stu-id="098d7-360">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="098d7-361">Assegnare un valore alla proprietà `Protocols` dall'enumerazione `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="098d7-361">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="098d7-362">Valore di enumerazione `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="098d7-362">`HttpProtocols` enum value</span></span> | <span data-ttu-id="098d7-363">Protocollo di connessione consentito</span><span class="sxs-lookup"><span data-stu-id="098d7-363">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="098d7-364">Solo HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-364">HTTP/1.1 only.</span></span> <span data-ttu-id="098d7-365">Può essere usato con o senza TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-365">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="098d7-366">Solo HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-366">HTTP/2 only.</span></span> <span data-ttu-id="098d7-367">Può essere usato senza TLS solo se il client supporta una [modalità di conoscenza pregressa](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="098d7-367">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="098d7-368">HTTP/1.1 e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-368">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="098d7-369">HTTP/2 richiede che il client selezioni HTTP/2 nell'handshake TLS [(Application-Layer Protocol negotiation) ALPN](https://tools.ietf.org/html/rfc7301#section-3) in caso contrario, il valore predefinito per la connessione è HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-369">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="098d7-370">Il valore predefinito `ListenOptions.Protocols` per qualsiasi endpoint è `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="098d7-370">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="098d7-371">Restrizioni relative a TLS per HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="098d7-371">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="098d7-372">TLS versione 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="098d7-372">TLS version 1.2 or later</span></span>
* <span data-ttu-id="098d7-373">Rinegoziazione disabilitata</span><span class="sxs-lookup"><span data-stu-id="098d7-373">Renegotiation disabled</span></span>
* <span data-ttu-id="098d7-374">Compressione disabilitata</span><span class="sxs-lookup"><span data-stu-id="098d7-374">Compression disabled</span></span>
* <span data-ttu-id="098d7-375">Dimensioni minime per lo scambio di chiavi temporanee:</span><span class="sxs-lookup"><span data-stu-id="098d7-375">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="098d7-376">Diffie-Hellman a curva ellittica (ECDH) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="098d7-376">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="098d7-377">Diffie-Hellman a campo finito (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="098d7-377">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="098d7-378">Pacchetto di crittografia consentito</span><span class="sxs-lookup"><span data-stu-id="098d7-378">Cipher suite not blacklisted</span></span>

<span data-ttu-id="098d7-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con curva ellittica P-256 &lbrack;`FIPS186`&rbrack; è supportato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="098d7-380">Nell'esempio seguente sono consentite connessioni HTTP/1.1 e HTTP/2 sulla porta 8000.</span><span class="sxs-lookup"><span data-stu-id="098d7-380">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="098d7-381">Le connessioni sono protette da TLS con un certificato incluso:</span><span class="sxs-lookup"><span data-stu-id="098d7-381">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="098d7-382">Usare il middleware di connessione per filtrare gli handshake TLS in base alla connessione per le crittografie specifiche, se necessario.</span><span class="sxs-lookup"><span data-stu-id="098d7-382">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="098d7-383">Nell'esempio seguente viene generato <xref:System.NotSupportedException> per qualsiasi algoritmo di crittografia non supportato dall'app.</span><span class="sxs-lookup"><span data-stu-id="098d7-383">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="098d7-384">In alternativa, definire e confrontare [ITlsHandshakeFeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) con un elenco di pacchetti di crittografia accettabili.</span><span class="sxs-lookup"><span data-stu-id="098d7-384">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="098d7-385">Non viene utilizzata alcuna crittografia con un algoritmo di crittografia [CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) .</span><span class="sxs-lookup"><span data-stu-id="098d7-385">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="098d7-386">Il filtro della connessione può essere configurato anche tramite un'espressione lambda <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder>:</span><span class="sxs-lookup"><span data-stu-id="098d7-386">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="098d7-387">In Linux è possibile usare <xref:System.Net.Security.CipherSuitesPolicy> per filtrare gli handshake TLS in base alla connessione:</span><span class="sxs-lookup"><span data-stu-id="098d7-387">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="098d7-388">*Impostare il protocollo dalla configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-388">*Set the protocol from configuration*</span></span>

<span data-ttu-id="098d7-389">`CreateDefaultBuilder` chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-389">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="098d7-390">L'esempio *appSettings. JSON* seguente stabilisce http/1.1 come protocollo di connessione predefinito per tutti gli endpoint:</span><span class="sxs-lookup"><span data-stu-id="098d7-390">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="098d7-391">Nell'esempio *appSettings. JSON* seguente viene stabilito il protocollo di connessione HTTP/1.1 per un endpoint specifico:</span><span class="sxs-lookup"><span data-stu-id="098d7-391">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="098d7-392">I protocolli specificati nei valori di override del codice sono impostati dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-392">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="098d7-393">Configurazione del trasporto</span><span class="sxs-lookup"><span data-stu-id="098d7-393">Transport configuration</span></span>

<span data-ttu-id="098d7-394">Per i progetti che richiedono l'uso di libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="098d7-394">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="098d7-395">Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="098d7-395">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="098d7-396">Chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> sul `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="098d7-396">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="098d7-397">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="098d7-397">URL prefixes</span></span>

<span data-ttu-id="098d7-398">Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-398">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="098d7-399">Sono validi solo i prefissi URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="098d7-399">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="098d7-400">Kestrel non supporta HTTPS quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-400">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="098d7-401">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-401">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="098d7-402">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-402">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="098d7-403">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-403">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="098d7-404">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-404">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="098d7-405">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-405">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="098d7-406">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="098d7-406">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="098d7-407">Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-407">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="098d7-408">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="098d7-408">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="098d7-409">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-409">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="098d7-410">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-410">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="098d7-411">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-411">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="098d7-412">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-412">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="098d7-413">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="098d7-413">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="098d7-414">Filtro host</span><span class="sxs-lookup"><span data-stu-id="098d7-414">Host filtering</span></span>

<span data-ttu-id="098d7-415">Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host.</span><span class="sxs-lookup"><span data-stu-id="098d7-415">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="098d7-416">L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback.</span><span class="sxs-lookup"><span data-stu-id="098d7-416">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="098d7-417">Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici.</span><span class="sxs-lookup"><span data-stu-id="098d7-417">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="098d7-418">Le intestazioni `Host` non vengono convalidate.</span><span class="sxs-lookup"><span data-stu-id="098d7-418">`Host` headers aren't validated.</span></span>

<span data-ttu-id="098d7-419">Come soluzione alternativa, usare il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="098d7-419">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="098d7-420">Il middleware di filtro host viene fornito dal pacchetto [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , fornito in modo implicito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-420">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="098d7-421">Il middleware viene aggiunto da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-421">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="098d7-422">Per impostazione predefinita, il middleware di filtro host è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-422">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="098d7-423">Per abilitare il middleware, definire una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*.</span><span class="sxs-lookup"><span data-stu-id="098d7-423">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="098d7-424">Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:</span><span class="sxs-lookup"><span data-stu-id="098d7-424">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="098d7-425">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="098d7-425">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="098d7-426">Il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) include anche un'opzione <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="098d7-426">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="098d7-427">Il middleware di intestazioni inoltrate e il middleware di filtro host hanno funzionalità simili per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="098d7-427">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="098d7-428">L'impostazione di `AllowedHosts` con il middleware delle intestazioni inoltrate è appropriato se l'intestazione `Host` non viene mantenuta durante l'inoltro delle richieste con un server proxy inverso o il bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="098d7-428">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="098d7-429">L'impostazione di `AllowedHosts` con il middleware del filtro host è appropriata se si usa Kestrel come server perimetrale rivolto al pubblico o se l'intestazione `Host` viene inoltrata direttamente.</span><span class="sxs-lookup"><span data-stu-id="098d7-429">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="098d7-430">Per altre informazioni sul middleware delle intestazioni inoltrate, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="098d7-430">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="098d7-431">Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="098d7-431">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="098d7-432">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-432">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="098d7-433">Kestrel supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-433">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="098d7-434">HTTPS</span><span class="sxs-lookup"><span data-stu-id="098d7-434">HTTPS</span></span>
* <span data-ttu-id="098d7-435">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="098d7-435">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="098d7-436">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="098d7-436">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="098d7-437">HTTP/2 (tranne che in macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="098d7-437">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="098d7-438">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="098d7-438">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="098d7-439">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-439">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="098d7-440">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="098d7-440">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="098d7-441">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="098d7-441">HTTP/2 support</span></span>

<span data-ttu-id="098d7-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è disponibile per le app ASP.NET Core se vengono soddisfatti i requisiti di base seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="098d7-443">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="098d7-443">Operating system&dagger;</span></span>
  * <span data-ttu-id="098d7-444">Windows Server 2016/Windows 10 o versioni successive&Dagger;</span><span class="sxs-lookup"><span data-stu-id="098d7-444">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="098d7-445">Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="098d7-445">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="098d7-446">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="098d7-446">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="098d7-447">Connessione [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="098d7-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="098d7-448">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="098d7-448">TLS 1.2 or later connection</span></span>

<span data-ttu-id="098d7-449">&dagger;HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="098d7-449">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="098d7-450">&Dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-450">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="098d7-451">Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato.</span><span class="sxs-lookup"><span data-stu-id="098d7-451">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="098d7-452">Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-452">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="098d7-453">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="098d7-453">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="098d7-454">HTTP/2 è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-454">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="098d7-455">Per altre informazioni sulla configurazione, vedere le sezioni [Opzioni Kestrel](#kestrel-options) e [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="098d7-455">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="098d7-456">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="098d7-456">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="098d7-457">È possibile usare Kestrel da solo o in combinazione con un *server proxy inverso*, ad esempio [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="098d7-457">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="098d7-458">Il server proxy inverso riceve le richieste HTTP dalla rete e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-458">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="098d7-459">Kestrel usato come server Web perimetrale (esposto a Internet):</span><span class="sxs-lookup"><span data-stu-id="098d7-459">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="098d7-461">Kestrel usato in una configurazione proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-461">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="098d7-463">La configurazione, con o senza un server proxy inverso, è una configurazione di hosting supportata.</span><span class="sxs-lookup"><span data-stu-id="098d7-463">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="098d7-464">Se viene utilizzato come server perimetrale in assenza di un server proxy inverso, Kestrel non supporta la condivisione tra più processi dell'IP e della porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-464">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="098d7-465">Quando è configurato per restare in ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dalle intestazioni `Host` delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-465">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="098d7-466">Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-466">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="098d7-467">Anche se la presenza di un server proxy inverso non è necessaria, può risultare utile per i motivi seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-467">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="098d7-468">Un proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-468">A reverse proxy:</span></span>

* <span data-ttu-id="098d7-469">Può limitare l'area della superficie di attacco pubblica esposta delle app ospitate.</span><span class="sxs-lookup"><span data-stu-id="098d7-469">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="098d7-470">Offre un livello aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-470">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="098d7-471">Potrebbe offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="098d7-471">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="098d7-472">Semplifica il bilanciamento del carico e la configurazione di comunicazioni protette (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-472">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="098d7-473">Solo il server proxy inverso richiede un certificato X. 509 e il server è in grado di comunicare con i server dell'app nella rete interna tramite HTTP normale.</span><span class="sxs-lookup"><span data-stu-id="098d7-473">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-474">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-474">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="098d7-475">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="098d7-475">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="098d7-476">Il pacchetto [Microsoft. AspNetCore. Server. gheppio](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="098d7-476">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="098d7-477">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-477">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="098d7-478">In *Program.cs* il codice del modello chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> in background.</span><span class="sxs-lookup"><span data-stu-id="098d7-478">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="098d7-479">Per ulteriori informazioni su `CreateDefaultBuilder` e sulla compilazione dell'host, vedere la sezione *configurare un host* di <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="098d7-479">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="098d7-480">Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, usare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="098d7-480">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="098d7-481">Se l'app non chiama `CreateDefaultBuilder` per configurare l'host, chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **prima** di chiamare `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="098d7-481">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="098d7-482">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="098d7-482">Kestrel options</span></span>

<span data-ttu-id="098d7-483">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="098d7-483">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="098d7-484">Impostare i vincoli per la proprietà <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-484">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="098d7-485">La proprietà `Limits` contiene un'istanza della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="098d7-485">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="098d7-486">Negli esempi seguenti viene usato lo spazio dei nomi <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="098d7-486">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="098d7-487">Le opzioni di gheppio, che sono C# configurate nel codice negli esempi seguenti, possono essere impostate anche usando un [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-487">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="098d7-488">Il provider di configurazione file, ad esempio, può caricare la configurazione di Gheppio da *appSettings. JSON* o *appSettings. { File Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="098d7-488">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="098d7-489">Usare **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-489">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="098d7-490">Configurare gheppio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="098d7-490">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="098d7-491">Inserire un'istanza di `IConfiguration` nella classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="098d7-491">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="098d7-492">Nell'esempio seguente si presuppone che la configurazione inserita venga assegnata alla proprietà `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="098d7-492">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="098d7-493">In `Startup.ConfigureServices`, caricare la sezione `Kestrel` della configurazione nella configurazione di gheppio.</span><span class="sxs-lookup"><span data-stu-id="098d7-493">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="098d7-494">Configurare il gheppio durante la compilazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="098d7-494">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="098d7-495">In *Program.cs*caricare la sezione `Kestrel` della configurazione nella configurazione di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="098d7-495">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="098d7-496">Entrambi gli approcci precedenti funzionano con qualsiasi [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-496">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="098d7-497">Timeout keep-alive</span><span class="sxs-lookup"><span data-stu-id="098d7-497">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="098d7-498">Ottiene o imposta il [timeout keep-alive](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="098d7-498">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="098d7-499">Il valore predefinito è 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="098d7-499">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="098d7-500">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="098d7-500">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="098d7-501">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-501">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="098d7-502">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="098d7-502">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="098d7-503">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="098d7-503">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="098d7-504">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="098d7-504">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="098d7-505">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-505">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="098d7-506">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="098d7-506">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="098d7-507">Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="098d7-507">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="098d7-508">L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="098d7-508">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="098d7-509">Eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="098d7-509">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="098d7-510">Viene generata un'eccezione se l'app configura il limite per una richiesta dopo che l'app ha iniziato a leggere la richiesta.</span><span class="sxs-lookup"><span data-stu-id="098d7-510">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="098d7-511">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="098d7-511">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="098d7-512">Quando un'app viene eseguita [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) dietro il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), il limite della dimensione del corpo della richiesta di Kestrel è disabilitato perché viene già impostato da IIS.</span><span class="sxs-lookup"><span data-stu-id="098d7-512">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="098d7-513">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-513">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="098d7-514">Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="098d7-514">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="098d7-515">Se la frequenza scende sotto il valore minimo, si è verificato il timeout della connessione. Il periodo di tolleranza è la quantità di tempo che il gheppio concede al client di aumentare la velocità di invio fino al minimo. la frequenza non viene controllata durante tale periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="098d7-515">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="098d7-516">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="098d7-516">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="098d7-517">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-517">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="098d7-518">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="098d7-518">A minimum rate also applies to the response.</span></span> <span data-ttu-id="098d7-519">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="098d7-519">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="098d7-520">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="098d7-520">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="098d7-521">Ignorare i limiti di velocità minima per ogni richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="098d7-521">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="098d7-522">Nessuna delle due funzionalità relative alla velocità, a cui si fa riferimento nell'esempio precedente, è presente in `HttpContext.Features` per le richieste HTTP/2 perché la modifica dei limiti di velocità per ogni richiesta non è supportata per HTTP/2 a causa del supporto del protocollo per il multiplexing delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-522">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="098d7-523">I limiti di velocità a livello di server configurati tramite `KestrelServerOptions.Limits` vengono tuttavia applicati alle connessioni HTTP/1.x e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-523">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="098d7-524">Timeout delle intestazioni delle richieste</span><span class="sxs-lookup"><span data-stu-id="098d7-524">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="098d7-525">Ottiene o imposta la quantità massima di tempo che il server dedica alla ricezione delle intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-525">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="098d7-526">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-526">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="098d7-527">Numero massimo di flussi per connessione</span><span class="sxs-lookup"><span data-stu-id="098d7-527">Maximum streams per connection</span></span>

<span data-ttu-id="098d7-528">`Http2.MaxStreamsPerConnection` limita il numero di flussi di richieste simultanee per ogni connessione HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-528">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="098d7-529">I flussi in eccesso vengono rifiutati.</span><span class="sxs-lookup"><span data-stu-id="098d7-529">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="098d7-530">Il valore predefinito è 100.</span><span class="sxs-lookup"><span data-stu-id="098d7-530">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="098d7-531">Dimensioni della tabella delle intestazioni</span><span class="sxs-lookup"><span data-stu-id="098d7-531">Header table size</span></span>

<span data-ttu-id="098d7-532">Il decodificatore HPACK decomprime le intestazioni HTTP per le connessioni HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-532">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="098d7-533">`Http2.HeaderTableSize` limita le dimensioni della tabella di compressione delle intestazioni usata dal decodificatore HPACK.</span><span class="sxs-lookup"><span data-stu-id="098d7-533">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="098d7-534">Il valore viene specificato in ottetti e deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="098d7-534">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="098d7-535">Il valore predefinito è 4096.</span><span class="sxs-lookup"><span data-stu-id="098d7-535">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="098d7-536">Dimensione massima del frame</span><span class="sxs-lookup"><span data-stu-id="098d7-536">Maximum frame size</span></span>

<span data-ttu-id="098d7-537">`Http2.MaxFrameSize` indica le dimensioni massime del payload del frame di connessione HTTP/2 da ricevere.</span><span class="sxs-lookup"><span data-stu-id="098d7-537">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="098d7-538">Il valore viene specificato in ottetti e deve essere compreso tra 2^14 (16.384) e 2^24-1 (16.777.215).</span><span class="sxs-lookup"><span data-stu-id="098d7-538">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="098d7-539">Il valore predefinito è 2^14 (16.384).</span><span class="sxs-lookup"><span data-stu-id="098d7-539">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="098d7-540">Dimensioni massime dell'intestazione della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-540">Maximum request header size</span></span>

<span data-ttu-id="098d7-541">`Http2.MaxRequestHeaderFieldSize` indica le dimensioni massime consentite in ottetti di valori di intestazione di richiesta.</span><span class="sxs-lookup"><span data-stu-id="098d7-541">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="098d7-542">Questo limite si applica insieme sia al nome che al valore nelle rappresentazioni compresse e non compresse.</span><span class="sxs-lookup"><span data-stu-id="098d7-542">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="098d7-543">Il valore deve essere maggiore di zero (0).</span><span class="sxs-lookup"><span data-stu-id="098d7-543">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="098d7-544">Il valore predefinito è 8.192.</span><span class="sxs-lookup"><span data-stu-id="098d7-544">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="098d7-545">Dimensioni della finestra di connessione iniziali</span><span class="sxs-lookup"><span data-stu-id="098d7-545">Initial connection window size</span></span>

<span data-ttu-id="098d7-546">`Http2.InitialConnectionWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta, aggregati in tutte le richieste (flussi), per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="098d7-546">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="098d7-547">Le richieste sono anche limitate da `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="098d7-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="098d7-548">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="098d7-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="098d7-549">Il valore predefinito è 128 KB (131.072).</span><span class="sxs-lookup"><span data-stu-id="098d7-549">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="098d7-550">Dimensioni della finestra di flusso iniziali</span><span class="sxs-lookup"><span data-stu-id="098d7-550">Initial stream window size</span></span>

<span data-ttu-id="098d7-551">`Http2.InitialStreamWindowSize` indica il numero massimo di byte di dati del corpo della richiesta che il server memorizza nel buffer in una volta per ogni richiesta (flusso).</span><span class="sxs-lookup"><span data-stu-id="098d7-551">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="098d7-552">Le richieste sono anche limitate da `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="098d7-552">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="098d7-553">Il valore deve essere maggiore di o uguale a 65.535 e minore di 2^31 (2.147.483.648).</span><span class="sxs-lookup"><span data-stu-id="098d7-553">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="098d7-554">Il valore predefinito è 96 KB (98.304).</span><span class="sxs-lookup"><span data-stu-id="098d7-554">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="098d7-555">I/O sincrono</span><span class="sxs-lookup"><span data-stu-id="098d7-555">Synchronous IO</span></span>

<span data-ttu-id="098d7-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controlla se l'I/O sincrono è consentito per la richiesta e la risposta.</span><span class="sxs-lookup"><span data-stu-id="098d7-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="098d7-557">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="098d7-557">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-558">Un numero elevato di operazioni di I/O sincrone bloccanti può portare alla scadenza del pool di thread, a causa della quale l'app smette di rispondere.</span><span class="sxs-lookup"><span data-stu-id="098d7-558">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="098d7-559">Abilitare solo `AllowSynchronousIO` quando si usa una libreria che non supporta l'I/O asincrono.</span><span class="sxs-lookup"><span data-stu-id="098d7-559">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="098d7-560">L'esempio seguente abilita l'I/O sincrono:</span><span class="sxs-lookup"><span data-stu-id="098d7-560">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="098d7-561">Per informazioni su altre opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="098d7-561">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="098d7-562">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="098d7-562">Endpoint configuration</span></span>

<span data-ttu-id="098d7-563">Per impostazione predefinita, ASP.NET Core è associato a:</span><span class="sxs-lookup"><span data-stu-id="098d7-563">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="098d7-564">`https://localhost:5001` (quando è presente un certificato di sviluppo locale)</span><span class="sxs-lookup"><span data-stu-id="098d7-564">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="098d7-565">Specificare gli URL usando gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-565">Specify URLs using the:</span></span>

* <span data-ttu-id="098d7-566">La variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="098d7-566">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="098d7-567">L'argomento della riga di comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-567">`--urls` command-line argument.</span></span>
* <span data-ttu-id="098d7-568">La chiave di configurazione dell'host `urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-568">`urls` host configuration key.</span></span>
* <span data-ttu-id="098d7-569">Il metodo di estensione `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-569">`UseUrls` extension method.</span></span>

<span data-ttu-id="098d7-570">Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-570">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="098d7-571">Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="098d7-571">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="098d7-572">Per altre informazioni su questi approcci, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="098d7-572">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="098d7-573">Viene creato un certificato di sviluppo nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-573">A development certificate is created:</span></span>

* <span data-ttu-id="098d7-574">Quando viene installato [.NET Core SDK](/dotnet/core/sdk).</span><span class="sxs-lookup"><span data-stu-id="098d7-574">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="098d7-575">Quando per creare un certificato viene usato [lo strumento dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="098d7-575">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="098d7-576">Alcuni browser richiedono la concessione di autorizzazioni esplicite per considerare attendibile il certificato di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="098d7-576">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="098d7-577">I modelli di progetto consentono di configurare le app per l'esecuzione su HTTPS per impostazione predefinita e includono il [reindirizzamento HTTPS e il supporto HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="098d7-577">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="098d7-578">Chiamare i metodi <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> oppure <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> su <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> per configurare le porte e i prefissi URL per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-578">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="098d7-579">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-579">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="098d7-580">configurazione `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="098d7-580">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="098d7-581">ConfigureEndpointDefaults (azione\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="098d7-582">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-582">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="098d7-583">Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-583">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="098d7-584">ConfigureHttpsDefaults (azione\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="098d7-585">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-585">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="098d7-586">Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-586">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="098d7-587">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="098d7-587">Configure(IConfiguration)</span></span>

<span data-ttu-id="098d7-588">Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento <xref:Microsoft.Extensions.Configuration.IConfiguration> come input.</span><span class="sxs-lookup"><span data-stu-id="098d7-588">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="098d7-589">L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-589">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="098d7-590">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="098d7-590">ListenOptions.UseHttps</span></span>

<span data-ttu-id="098d7-591">Configura Kestrel per l'uso di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-591">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="098d7-592">Estensioni `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-592">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="098d7-593">`UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-593">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="098d7-594">Genera un'eccezione se non è stato configurato alcun certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-594">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="098d7-595">Parametri `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-595">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="098d7-596">`filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="098d7-596">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="098d7-597">`password` è la password richiesta per accedere ai dati del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-597">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="098d7-598">`configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-598">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="098d7-599">Restituisce `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-599">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="098d7-600">`storeName` è l'archivio certificati da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-600">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="098d7-601">`subject` è il nome dell'oggetto del certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-601">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="098d7-602">`allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-602">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="098d7-603">`location` è il percorso dell'archivio da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-603">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="098d7-604">`serverCertificate` è il certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-604">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="098d7-605">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="098d7-605">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="098d7-606">Come minimo, è necessario specificare un certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-606">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="098d7-607">Configurazioni supportate descritte in seguito:</span><span class="sxs-lookup"><span data-stu-id="098d7-607">Supported configurations described next:</span></span>

* <span data-ttu-id="098d7-608">Nessuna configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-608">No configuration</span></span>
* <span data-ttu-id="098d7-609">Sostituire il certificato predefinito della configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-609">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="098d7-610">Modificare le impostazioni predefinite nel codice</span><span class="sxs-lookup"><span data-stu-id="098d7-610">Change the defaults in code</span></span>

<span data-ttu-id="098d7-611">*Nessuna configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-611">*No configuration*</span></span>

<span data-ttu-id="098d7-612">Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-612">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="098d7-613">*Sostituire il certificato predefinito della configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-613">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="098d7-614">`CreateDefaultBuilder` chiama `Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-614">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="098d7-615">È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-615">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="098d7-616">Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-616">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="098d7-617">Nel file *appsettings.json* di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-617">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="098d7-618">Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-618">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="098d7-619">Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default**  o il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="098d7-619">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="098d7-620">Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-620">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="098d7-621">Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:</span><span class="sxs-lookup"><span data-stu-id="098d7-621">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="098d7-622">Note di schema:</span><span class="sxs-lookup"><span data-stu-id="098d7-622">Schema notes:</span></span>

* <span data-ttu-id="098d7-623">I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="098d7-623">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="098d7-624">Ad esempio, sono validi sia `HTTPS` che `Https`.</span><span class="sxs-lookup"><span data-stu-id="098d7-624">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="098d7-625">Il parametro `Url` è obbligatorio per ogni endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-625">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="098d7-626">Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.</span><span class="sxs-lookup"><span data-stu-id="098d7-626">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="098d7-627">Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono.</span><span class="sxs-lookup"><span data-stu-id="098d7-627">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="098d7-628">Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-628">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="098d7-629">La sezione `Certificate` è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="098d7-629">The `Certificate` section is optional.</span></span> <span data-ttu-id="098d7-630">Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-630">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="098d7-631">Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-631">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="098d7-632">La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="098d7-632">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="098d7-633">In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.</span><span class="sxs-lookup"><span data-stu-id="098d7-633">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="098d7-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, listenOptions => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:</span><span class="sxs-lookup"><span data-stu-id="098d7-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="098d7-635">è possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per continuare a scorrere il caricatore esistente, ad esempio quello fornito da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="098d7-635">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="098d7-636">La sezione di configurazione per ogni endpoint è disponibile nelle opzioni del metodo `Endpoint` in modo che sia possibile leggere le impostazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="098d7-636">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="098d7-637">È possibile caricare più configurazioni chiamando ancora `options.Configure(context.Configuration.GetSection("{SECTION}"))` con un'altra sezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-637">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="098d7-638">Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-638">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="098d7-639">Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.</span><span class="sxs-lookup"><span data-stu-id="098d7-639">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="098d7-640">`KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="098d7-640">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="098d7-641">Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-641">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="098d7-642">*Modificare le impostazioni predefinite nel codice*</span><span class="sxs-lookup"><span data-stu-id="098d7-642">*Change the defaults in code*</span></span>

<span data-ttu-id="098d7-643">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente.</span><span class="sxs-lookup"><span data-stu-id="098d7-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="098d7-644">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-644">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="098d7-645">*Supporto kestrel per SNI*</span><span class="sxs-lookup"><span data-stu-id="098d7-645">*Kestrel support for SNI*</span></span>

<span data-ttu-id="098d7-646">[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="098d7-646">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="098d7-647">Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto.</span><span class="sxs-lookup"><span data-stu-id="098d7-647">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="098d7-648">Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-648">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="098d7-649">Kestrel supporta SNI tramite il callback `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="098d7-649">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="098d7-650">Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="098d7-650">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="098d7-651">Il supporto SNI richiede:</span><span class="sxs-lookup"><span data-stu-id="098d7-651">SNI support requires:</span></span>

* <span data-ttu-id="098d7-652">In esecuzione nel Framework di destinazione `netcoreapp2.1` o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="098d7-652">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="098d7-653">In `net461` o versioni successive, il callback viene richiamato, ma il `name` è sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="098d7-653">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="098d7-654">L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-654">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="098d7-655">Esecuzione di tutti i siti Web nella stessa istanza di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-655">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="098d7-656">Kestrel supporta la condivisione di un indirizzo IP e di una porta tra più istanze solo con un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="098d7-656">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="connection-logging"></a><span data-ttu-id="098d7-657">Registrazione connessione</span><span class="sxs-lookup"><span data-stu-id="098d7-657">Connection logging</span></span>

<span data-ttu-id="098d7-658">Chiamare <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> per creare log a livello di debug per la comunicazione a livello di byte in una connessione.</span><span class="sxs-lookup"><span data-stu-id="098d7-658">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="098d7-659">La registrazione delle connessioni è utile per la risoluzione dei problemi nella comunicazione di basso livello, ad esempio durante la crittografia TLS e dietro i proxy.</span><span class="sxs-lookup"><span data-stu-id="098d7-659">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="098d7-660">Se `UseConnectionLogging` viene posizionata prima `UseHttps`, viene registrato il traffico crittografato.</span><span class="sxs-lookup"><span data-stu-id="098d7-660">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="098d7-661">Se `UseConnectionLogging` viene inserito dopo `UseHttps`, il traffico decrittografato viene registrato.</span><span class="sxs-lookup"><span data-stu-id="098d7-661">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="098d7-662">Associazione a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="098d7-662">Bind to a TCP socket</span></span>

<span data-ttu-id="098d7-663">Il metodo <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> esegue l'associazione a un socket TCP e un'espressione lambda per le opzioni consente di configurare un certificato X.509:</span><span class="sxs-lookup"><span data-stu-id="098d7-663">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="098d7-664">L'esempio configura HTTPS per un endpoint con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-664">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="098d7-665">Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="098d7-665">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="098d7-666">Associazione a un socket Unix</span><span class="sxs-lookup"><span data-stu-id="098d7-666">Bind to a Unix socket</span></span>

<span data-ttu-id="098d7-667">Impostare l'ascolto su un socket Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-667">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="098d7-668">Porta 0</span><span class="sxs-lookup"><span data-stu-id="098d7-668">Port 0</span></span>

<span data-ttu-id="098d7-669">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="098d7-669">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="098d7-670">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="098d7-670">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="098d7-671">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="098d7-671">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="098d7-672">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="098d7-672">Limitations</span></span>

<span data-ttu-id="098d7-673">Configurare gli endpoint con i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-673">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="098d7-674">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-674">`--urls` command-line argument</span></span>
* <span data-ttu-id="098d7-675">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-675">`urls` host configuration key</span></span>
* <span data-ttu-id="098d7-676">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="098d7-676">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="098d7-677">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-677">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="098d7-678">Tenere presenti, tuttavia, le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-678">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="098d7-679">HTTPS non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="098d7-679">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="098d7-680">Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-680">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="098d7-681">Configurazione dell'endpoint IIS</span><span class="sxs-lookup"><span data-stu-id="098d7-681">IIS endpoint configuration</span></span>

<span data-ttu-id="098d7-682">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-682">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="098d7-683">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="098d7-683">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="098d7-684">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="098d7-684">ListenOptions.Protocols</span></span>

<span data-ttu-id="098d7-685">La proprietà `Protocols` stabilisce i protocolli HTTP (`HttpProtocols`) abilitati in un endpoint di connessione o per il server.</span><span class="sxs-lookup"><span data-stu-id="098d7-685">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="098d7-686">Assegnare un valore alla proprietà `Protocols` dall'enumerazione `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="098d7-686">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="098d7-687">Valore di enumerazione `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="098d7-687">`HttpProtocols` enum value</span></span> | <span data-ttu-id="098d7-688">Protocollo di connessione consentito</span><span class="sxs-lookup"><span data-stu-id="098d7-688">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="098d7-689">Solo HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-689">HTTP/1.1 only.</span></span> <span data-ttu-id="098d7-690">Può essere usato con o senza TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-690">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="098d7-691">Solo HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-691">HTTP/2 only.</span></span> <span data-ttu-id="098d7-692">Può essere usato senza TLS solo se il client supporta una [modalità di conoscenza pregressa](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="098d7-692">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="098d7-693">HTTP/1.1 e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="098d7-693">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="098d7-694">HTTP/2 richiede una connessione TLS e [ALPN (Application-Layer Protocol negotiation)](https://tools.ietf.org/html/rfc7301#section-3) ; in caso contrario, il valore predefinito per la connessione è HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-694">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="098d7-695">Il protocollo predefinito è HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="098d7-695">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="098d7-696">Restrizioni relative a TLS per HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="098d7-696">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="098d7-697">TLS versione 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="098d7-697">TLS version 1.2 or later</span></span>
* <span data-ttu-id="098d7-698">Rinegoziazione disabilitata</span><span class="sxs-lookup"><span data-stu-id="098d7-698">Renegotiation disabled</span></span>
* <span data-ttu-id="098d7-699">Compressione disabilitata</span><span class="sxs-lookup"><span data-stu-id="098d7-699">Compression disabled</span></span>
* <span data-ttu-id="098d7-700">Dimensioni minime per lo scambio di chiavi temporanee:</span><span class="sxs-lookup"><span data-stu-id="098d7-700">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="098d7-701">Diffie-Hellman a curva ellittica (ECDH) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="098d7-701">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="098d7-702">Diffie-Hellman a campo finito (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit (minimo)</span><span class="sxs-lookup"><span data-stu-id="098d7-702">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="098d7-703">Pacchetto di crittografia consentito</span><span class="sxs-lookup"><span data-stu-id="098d7-703">Cipher suite not blacklisted</span></span>

<span data-ttu-id="098d7-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con curva ellittica P-256 &lbrack;`FIPS186`&rbrack; è supportato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="098d7-705">Nell'esempio seguente sono consentite connessioni HTTP/1.1 e HTTP/2 sulla porta 8000.</span><span class="sxs-lookup"><span data-stu-id="098d7-705">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="098d7-706">Le connessioni sono protette da TLS con un certificato incluso:</span><span class="sxs-lookup"><span data-stu-id="098d7-706">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="098d7-707">Facoltativamente, creare un'implementazione `IConnectionAdapter` per filtrare gli handshake TLS in base alla connessione in caso di crittografie specifiche:</span><span class="sxs-lookup"><span data-stu-id="098d7-707">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
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

<span data-ttu-id="098d7-708">*Impostare il protocollo dalla configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-708">*Set the protocol from configuration*</span></span>

<span data-ttu-id="098d7-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="098d7-710">Nell'esempio *appsettings.json* seguente viene stabilito un protocollo di connessione predefinito (HTTP/1.1 e HTTP/2) per tutti gli endpoint Kestrel:</span><span class="sxs-lookup"><span data-stu-id="098d7-710">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="098d7-711">Il file di configurazione di esempio seguente stabilisce un protocollo di connessione per un endpoint specifico:</span><span class="sxs-lookup"><span data-stu-id="098d7-711">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="098d7-712">I protocolli specificati nei valori di override del codice sono impostati dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-712">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="098d7-713">Configurazione del trasporto</span><span class="sxs-lookup"><span data-stu-id="098d7-713">Transport configuration</span></span>

<span data-ttu-id="098d7-714">Con la versione ASP.NET Core 2.1, il trasporto predefinito di Kestrel non si basa più su Libuv, ma su socket gestiti.</span><span class="sxs-lookup"><span data-stu-id="098d7-714">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="098d7-715">Si tratta di una modifica importante per le app ASP.NET 2.0 Core che vengono aggiornate alla versione 2.1, che chiamano <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> e dipendono da uno dei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-715">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="098d7-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (riferimento diretto al pacchetto)</span><span class="sxs-lookup"><span data-stu-id="098d7-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="098d7-717">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="098d7-717">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="098d7-718">Per i progetti che richiedono l'uso di libuv:</span><span class="sxs-lookup"><span data-stu-id="098d7-718">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="098d7-719">Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="098d7-719">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="098d7-720">Chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-720">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="098d7-721">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="098d7-721">URL prefixes</span></span>

<span data-ttu-id="098d7-722">Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-722">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="098d7-723">Sono validi solo i prefissi URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="098d7-723">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="098d7-724">Kestrel non supporta HTTPS quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-724">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="098d7-725">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-725">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="098d7-726">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-726">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="098d7-727">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-727">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="098d7-728">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-728">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="098d7-729">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-729">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="098d7-730">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="098d7-730">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="098d7-731">Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-731">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="098d7-732">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="098d7-732">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="098d7-733">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-733">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="098d7-734">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-734">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="098d7-735">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-735">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="098d7-736">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-736">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="098d7-737">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="098d7-737">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="098d7-738">Filtro host</span><span class="sxs-lookup"><span data-stu-id="098d7-738">Host filtering</span></span>

<span data-ttu-id="098d7-739">Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host.</span><span class="sxs-lookup"><span data-stu-id="098d7-739">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="098d7-740">L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback.</span><span class="sxs-lookup"><span data-stu-id="098d7-740">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="098d7-741">Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici.</span><span class="sxs-lookup"><span data-stu-id="098d7-741">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="098d7-742">Le intestazioni `Host` non vengono convalidate.</span><span class="sxs-lookup"><span data-stu-id="098d7-742">`Host` headers aren't validated.</span></span>

<span data-ttu-id="098d7-743">Come soluzione alternativa, usare il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="098d7-743">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="098d7-744">Il middleware di filtro host viene fornito dal pacchetto [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , incluso nel [metapacchetto Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 o 2,2).</span><span class="sxs-lookup"><span data-stu-id="098d7-744">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="098d7-745">Il middleware viene aggiunto da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-745">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="098d7-746">Per impostazione predefinita, il middleware di filtro host è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-746">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="098d7-747">Per abilitare il middleware, definire una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*.</span><span class="sxs-lookup"><span data-stu-id="098d7-747">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="098d7-748">Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:</span><span class="sxs-lookup"><span data-stu-id="098d7-748">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="098d7-749">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="098d7-749">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="098d7-750">Il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) include anche un'opzione <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="098d7-750">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="098d7-751">Il middleware di intestazioni inoltrate e il middleware di filtro host hanno funzionalità simili per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="098d7-751">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="098d7-752">L'impostazione di `AllowedHosts` con il middleware delle intestazioni inoltrate è appropriato se l'intestazione `Host` non viene mantenuta durante l'inoltro delle richieste con un server proxy inverso o il bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="098d7-752">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="098d7-753">L'impostazione di `AllowedHosts` con il middleware del filtro host è appropriata se si usa Kestrel come server perimetrale rivolto al pubblico o se l'intestazione `Host` viene inoltrata direttamente.</span><span class="sxs-lookup"><span data-stu-id="098d7-753">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="098d7-754">Per altre informazioni sul middleware delle intestazioni inoltrate, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="098d7-754">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="098d7-755">Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="098d7-755">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="098d7-756">Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-756">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="098d7-757">Kestrel supporta gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-757">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="098d7-758">HTTPS</span><span class="sxs-lookup"><span data-stu-id="098d7-758">HTTPS</span></span>
* <span data-ttu-id="098d7-759">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="098d7-759">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="098d7-760">Socket Unix ad alte prestazioni dietro Nginx</span><span class="sxs-lookup"><span data-stu-id="098d7-760">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="098d7-761">Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="098d7-761">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="098d7-762">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="098d7-762">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="098d7-763">Quando usare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="098d7-763">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="098d7-764">È possibile usare Kestrel da solo o in combinazione con un *server proxy inverso*, ad esempio [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="098d7-764">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="098d7-765">Il server proxy inverso riceve le richieste HTTP dalla rete e le inoltra a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-765">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="098d7-766">Kestrel usato come server Web perimetrale (esposto a Internet):</span><span class="sxs-lookup"><span data-stu-id="098d7-766">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="098d7-768">Kestrel usato in una configurazione proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-768">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="098d7-770">La configurazione, con o senza un server proxy inverso, è una configurazione di hosting supportata.</span><span class="sxs-lookup"><span data-stu-id="098d7-770">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="098d7-771">Se viene utilizzato come server perimetrale in assenza di un server proxy inverso, Kestrel non supporta la condivisione tra più processi dell'IP e della porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-771">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="098d7-772">Quando è configurato per restare in ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dalle intestazioni `Host` delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-772">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="098d7-773">Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.</span><span class="sxs-lookup"><span data-stu-id="098d7-773">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="098d7-774">Anche se la presenza di un server proxy inverso non è necessaria, può risultare utile per i motivi seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-774">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="098d7-775">Un proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="098d7-775">A reverse proxy:</span></span>

* <span data-ttu-id="098d7-776">Può limitare l'area della superficie di attacco pubblica esposta delle app ospitate.</span><span class="sxs-lookup"><span data-stu-id="098d7-776">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="098d7-777">Offre un livello aggiuntivo di configurazione e protezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-777">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="098d7-778">Potrebbe offrire un'integrazione migliore con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="098d7-778">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="098d7-779">Semplifica il bilanciamento del carico e la configurazione di comunicazioni protette (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-779">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="098d7-780">Solo il server proxy inverso richiede un certificato X. 509 e il server è in grado di comunicare con i server dell'app nella rete interna tramite HTTP normale.</span><span class="sxs-lookup"><span data-stu-id="098d7-780">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-781">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-781">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="098d7-782">Come usare Kestrel nelle app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="098d7-782">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="098d7-783">Il pacchetto [Microsoft. AspNetCore. Server. gheppio](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="098d7-783">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="098d7-784">I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-784">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="098d7-785">In *Program.cs* il codice del modello chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> in background.</span><span class="sxs-lookup"><span data-stu-id="098d7-785">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="098d7-786">Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-786">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="098d7-787">Per ulteriori informazioni su `CreateDefaultBuilder` e sulla compilazione dell'host, vedere la sezione *configurare un host* di <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="098d7-787">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="098d7-788">Opzioni Kestrel</span><span class="sxs-lookup"><span data-stu-id="098d7-788">Kestrel options</span></span>

<span data-ttu-id="098d7-789">Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="098d7-789">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="098d7-790">Impostare i vincoli per la proprietà <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-790">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="098d7-791">La proprietà `Limits` contiene un'istanza della classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="098d7-791">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="098d7-792">Negli esempi seguenti viene usato lo spazio dei nomi <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="098d7-792">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="098d7-793">Le opzioni di gheppio, che sono C# configurate nel codice negli esempi seguenti, possono essere impostate anche usando un [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-793">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="098d7-794">Il provider di configurazione file, ad esempio, può caricare la configurazione di Gheppio da *appSettings. JSON* o *appSettings. { File Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="098d7-794">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="098d7-795">Usare **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-795">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="098d7-796">Configurare gheppio in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="098d7-796">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="098d7-797">Inserire un'istanza di `IConfiguration` nella classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="098d7-797">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="098d7-798">Nell'esempio seguente si presuppone che la configurazione inserita venga assegnata alla proprietà `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="098d7-798">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="098d7-799">In `Startup.ConfigureServices`, caricare la sezione `Kestrel` della configurazione nella configurazione di gheppio.</span><span class="sxs-lookup"><span data-stu-id="098d7-799">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="098d7-800">Configurare il gheppio durante la compilazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="098d7-800">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="098d7-801">In *Program.cs*caricare la sezione `Kestrel` della configurazione nella configurazione di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="098d7-801">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="098d7-802">Entrambi gli approcci precedenti funzionano con qualsiasi [provider di configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="098d7-802">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="098d7-803">Timeout keep-alive</span><span class="sxs-lookup"><span data-stu-id="098d7-803">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="098d7-804">Ottiene o imposta il [timeout keep-alive](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="098d7-804">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="098d7-805">Il valore predefinito è 2 minuti.</span><span class="sxs-lookup"><span data-stu-id="098d7-805">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="098d7-806">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="098d7-806">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="098d7-807">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-807">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="098d7-808">È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="098d7-808">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="098d7-809">Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="098d7-809">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="098d7-810">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="098d7-810">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="098d7-811">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-811">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="098d7-812">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="098d7-812">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="098d7-813">Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="098d7-813">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="098d7-814">L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="098d7-814">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="098d7-815">Eseguire l'override dell'impostazione per una richiesta specifica nel middleware:</span><span class="sxs-lookup"><span data-stu-id="098d7-815">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="098d7-816">Viene generata un'eccezione se l'app configura il limite per una richiesta dopo che l'app ha iniziato a leggere la richiesta.</span><span class="sxs-lookup"><span data-stu-id="098d7-816">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="098d7-817">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="098d7-817">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="098d7-818">Quando un'app viene eseguita [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) dietro il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), il limite della dimensione del corpo della richiesta di Kestrel è disabilitato perché viene già impostato da IIS.</span><span class="sxs-lookup"><span data-stu-id="098d7-818">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="098d7-819">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="098d7-819">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="098d7-820">Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata.</span><span class="sxs-lookup"><span data-stu-id="098d7-820">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="098d7-821">Se la frequenza scende sotto il valore minimo, si è verificato il timeout della connessione. Il periodo di tolleranza è la quantità di tempo che il gheppio concede al client di aumentare la velocità di invio fino al minimo. la frequenza non viene controllata durante tale periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="098d7-821">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="098d7-822">Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.</span><span class="sxs-lookup"><span data-stu-id="098d7-822">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="098d7-823">La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-823">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="098d7-824">Anche per la risposta è prevista una velocità minima.</span><span class="sxs-lookup"><span data-stu-id="098d7-824">A minimum rate also applies to the response.</span></span> <span data-ttu-id="098d7-825">Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="098d7-825">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="098d7-826">L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="098d7-826">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="098d7-827">Timeout delle intestazioni delle richieste</span><span class="sxs-lookup"><span data-stu-id="098d7-827">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="098d7-828">Ottiene o imposta la quantità massima di tempo che il server dedica alla ricezione delle intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="098d7-828">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="098d7-829">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="098d7-829">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="098d7-830">I/O sincrono</span><span class="sxs-lookup"><span data-stu-id="098d7-830">Synchronous IO</span></span>

<span data-ttu-id="098d7-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controlla se l'I/O sincrono è consentito per la richiesta e la risposta.</span><span class="sxs-lookup"><span data-stu-id="098d7-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="098d7-832">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="098d7-832">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="098d7-833">Un numero elevato di operazioni di I/O sincrone bloccanti può portare alla scadenza del pool di thread, a causa della quale l'app smette di rispondere.</span><span class="sxs-lookup"><span data-stu-id="098d7-833">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="098d7-834">Abilitare solo `AllowSynchronousIO` quando si usa una libreria che non supporta l'I/O asincrono.</span><span class="sxs-lookup"><span data-stu-id="098d7-834">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="098d7-835">L'esempio seguente disabilita l'I/O sincrono:</span><span class="sxs-lookup"><span data-stu-id="098d7-835">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="098d7-836">Per informazioni su altre opzioni e limiti di Kestrel, vedere:</span><span class="sxs-lookup"><span data-stu-id="098d7-836">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="098d7-837">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="098d7-837">Endpoint configuration</span></span>

<span data-ttu-id="098d7-838">Per impostazione predefinita, ASP.NET Core è associato a:</span><span class="sxs-lookup"><span data-stu-id="098d7-838">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="098d7-839">`https://localhost:5001` (quando è presente un certificato di sviluppo locale)</span><span class="sxs-lookup"><span data-stu-id="098d7-839">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="098d7-840">Specificare gli URL usando gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-840">Specify URLs using the:</span></span>

* <span data-ttu-id="098d7-841">La variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="098d7-841">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="098d7-842">L'argomento della riga di comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-842">`--urls` command-line argument.</span></span>
* <span data-ttu-id="098d7-843">La chiave di configurazione dell'host `urls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-843">`urls` host configuration key.</span></span>
* <span data-ttu-id="098d7-844">Il metodo di estensione `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-844">`UseUrls` extension method.</span></span>

<span data-ttu-id="098d7-845">Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-845">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="098d7-846">Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="098d7-846">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="098d7-847">Per altre informazioni su questi approcci, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="098d7-847">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="098d7-848">Viene creato un certificato di sviluppo nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-848">A development certificate is created:</span></span>

* <span data-ttu-id="098d7-849">Quando viene installato [.NET Core SDK](/dotnet/core/sdk).</span><span class="sxs-lookup"><span data-stu-id="098d7-849">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="098d7-850">Quando per creare un certificato viene usato [lo strumento dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="098d7-850">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="098d7-851">Alcuni browser richiedono la concessione di autorizzazioni esplicite per considerare attendibile il certificato di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="098d7-851">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="098d7-852">I modelli di progetto consentono di configurare le app per l'esecuzione su HTTPS per impostazione predefinita e includono il [reindirizzamento HTTPS e il supporto HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="098d7-852">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="098d7-853">Chiamare i metodi <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> oppure <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> su <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> per configurare le porte e i prefissi URL per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-853">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="098d7-854">Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).</span><span class="sxs-lookup"><span data-stu-id="098d7-854">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="098d7-855">configurazione `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="098d7-855">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="098d7-856">ConfigureEndpointDefaults (azione\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="098d7-857">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-857">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="098d7-858">Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-858">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="098d7-859">ConfigureHttpsDefaults (azione\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="098d7-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="098d7-860">Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-860">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="098d7-861">Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-861">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="098d7-862">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="098d7-862">Configure(IConfiguration)</span></span>

<span data-ttu-id="098d7-863">Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento <xref:Microsoft.Extensions.Configuration.IConfiguration> come input.</span><span class="sxs-lookup"><span data-stu-id="098d7-863">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="098d7-864">L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-864">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="098d7-865">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="098d7-865">ListenOptions.UseHttps</span></span>

<span data-ttu-id="098d7-866">Configura Kestrel per l'uso di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="098d7-866">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="098d7-867">Estensioni `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-867">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="098d7-868">`UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-868">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="098d7-869">Genera un'eccezione se non è stato configurato alcun certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-869">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="098d7-870">Parametri `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="098d7-870">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="098d7-871">`filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="098d7-871">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="098d7-872">`password` è la password richiesta per accedere ai dati del certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-872">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="098d7-873">`configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-873">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="098d7-874">Restituisce `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="098d7-874">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="098d7-875">`storeName` è l'archivio certificati da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-875">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="098d7-876">`subject` è il nome dell'oggetto del certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-876">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="098d7-877">`allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-877">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="098d7-878">`location` è il percorso dell'archivio da cui caricare il certificato.</span><span class="sxs-lookup"><span data-stu-id="098d7-878">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="098d7-879">`serverCertificate` è il certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="098d7-879">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="098d7-880">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="098d7-880">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="098d7-881">Come minimo, è necessario specificare un certificato predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-881">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="098d7-882">Configurazioni supportate descritte in seguito:</span><span class="sxs-lookup"><span data-stu-id="098d7-882">Supported configurations described next:</span></span>

* <span data-ttu-id="098d7-883">Nessuna configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-883">No configuration</span></span>
* <span data-ttu-id="098d7-884">Sostituire il certificato predefinito della configurazione</span><span class="sxs-lookup"><span data-stu-id="098d7-884">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="098d7-885">Modificare le impostazioni predefinite nel codice</span><span class="sxs-lookup"><span data-stu-id="098d7-885">Change the defaults in code</span></span>

<span data-ttu-id="098d7-886">*Nessuna configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-886">*No configuration*</span></span>

<span data-ttu-id="098d7-887">Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).</span><span class="sxs-lookup"><span data-stu-id="098d7-887">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="098d7-888">*Sostituire il certificato predefinito della configurazione*</span><span class="sxs-lookup"><span data-stu-id="098d7-888">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="098d7-889">`CreateDefaultBuilder` chiama `Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-889">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="098d7-890">È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito.</span><span class="sxs-lookup"><span data-stu-id="098d7-890">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="098d7-891">Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-891">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="098d7-892">Nel file *appsettings.json* di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-892">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="098d7-893">Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.</span><span class="sxs-lookup"><span data-stu-id="098d7-893">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="098d7-894">Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default**  o il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="098d7-894">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="098d7-895">Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="098d7-895">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="098d7-896">Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:</span><span class="sxs-lookup"><span data-stu-id="098d7-896">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="098d7-897">Note di schema:</span><span class="sxs-lookup"><span data-stu-id="098d7-897">Schema notes:</span></span>

* <span data-ttu-id="098d7-898">I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="098d7-898">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="098d7-899">Ad esempio, sono validi sia `HTTPS` che `Https`.</span><span class="sxs-lookup"><span data-stu-id="098d7-899">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="098d7-900">Il parametro `Url` è obbligatorio per ogni endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-900">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="098d7-901">Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.</span><span class="sxs-lookup"><span data-stu-id="098d7-901">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="098d7-902">Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono.</span><span class="sxs-lookup"><span data-stu-id="098d7-902">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="098d7-903">Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-903">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="098d7-904">La sezione `Certificate` è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="098d7-904">The `Certificate` section is optional.</span></span> <span data-ttu-id="098d7-905">Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-905">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="098d7-906">Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-906">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="098d7-907">La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="098d7-907">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="098d7-908">In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.</span><span class="sxs-lookup"><span data-stu-id="098d7-908">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="098d7-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, listenOptions => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:</span><span class="sxs-lookup"><span data-stu-id="098d7-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="098d7-910">è possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per continuare a scorrere il caricatore esistente, ad esempio quello fornito da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="098d7-910">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="098d7-911">La sezione di configurazione per ogni endpoint è disponibile nelle opzioni del metodo `Endpoint` in modo che sia possibile leggere le impostazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="098d7-911">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="098d7-912">È possibile caricare più configurazioni chiamando ancora `options.Configure(context.Configuration.GetSection("{SECTION}"))` con un'altra sezione.</span><span class="sxs-lookup"><span data-stu-id="098d7-912">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="098d7-913">Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-913">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="098d7-914">Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.</span><span class="sxs-lookup"><span data-stu-id="098d7-914">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="098d7-915">`KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione.</span><span class="sxs-lookup"><span data-stu-id="098d7-915">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="098d7-916">Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.</span><span class="sxs-lookup"><span data-stu-id="098d7-916">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="098d7-917">*Modificare le impostazioni predefinite nel codice*</span><span class="sxs-lookup"><span data-stu-id="098d7-917">*Change the defaults in code*</span></span>

<span data-ttu-id="098d7-918">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente.</span><span class="sxs-lookup"><span data-stu-id="098d7-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="098d7-919">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.</span><span class="sxs-lookup"><span data-stu-id="098d7-919">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="098d7-920">*Supporto kestrel per SNI*</span><span class="sxs-lookup"><span data-stu-id="098d7-920">*Kestrel support for SNI*</span></span>

<span data-ttu-id="098d7-921">[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="098d7-921">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="098d7-922">Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto.</span><span class="sxs-lookup"><span data-stu-id="098d7-922">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="098d7-923">Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-923">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="098d7-924">Kestrel supporta SNI tramite il callback `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="098d7-924">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="098d7-925">Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="098d7-925">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="098d7-926">Il supporto SNI richiede:</span><span class="sxs-lookup"><span data-stu-id="098d7-926">SNI support requires:</span></span>

* <span data-ttu-id="098d7-927">In esecuzione nel Framework di destinazione `netcoreapp2.1` o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="098d7-927">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="098d7-928">In `net461` o versioni successive, il callback viene richiamato, ma il `name` è sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="098d7-928">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="098d7-929">L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="098d7-929">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="098d7-930">Esecuzione di tutti i siti Web nella stessa istanza di Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-930">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="098d7-931">Kestrel supporta la condivisione di un indirizzo IP e di una porta tra più istanze solo con un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="098d7-931">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="connection-logging"></a><span data-ttu-id="098d7-932">Registrazione connessione</span><span class="sxs-lookup"><span data-stu-id="098d7-932">Connection logging</span></span>

<span data-ttu-id="098d7-933">Chiamare <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> per creare log a livello di debug per la comunicazione a livello di byte in una connessione.</span><span class="sxs-lookup"><span data-stu-id="098d7-933">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="098d7-934">La registrazione delle connessioni è utile per la risoluzione dei problemi nella comunicazione di basso livello, ad esempio durante la crittografia TLS e dietro i proxy.</span><span class="sxs-lookup"><span data-stu-id="098d7-934">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="098d7-935">Se `UseConnectionLogging` viene posizionata prima `UseHttps`, viene registrato il traffico crittografato.</span><span class="sxs-lookup"><span data-stu-id="098d7-935">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="098d7-936">Se `UseConnectionLogging` viene inserito dopo `UseHttps`, il traffico decrittografato viene registrato.</span><span class="sxs-lookup"><span data-stu-id="098d7-936">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="098d7-937">Associazione a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="098d7-937">Bind to a TCP socket</span></span>

<span data-ttu-id="098d7-938">Il metodo <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> esegue l'associazione a un socket TCP e un'espressione lambda per le opzioni consente di configurare un certificato X.509:</span><span class="sxs-lookup"><span data-stu-id="098d7-938">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="098d7-939">L'esempio configura HTTPS per un endpoint con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="098d7-939">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="098d7-940">Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.</span><span class="sxs-lookup"><span data-stu-id="098d7-940">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="098d7-941">Associazione a un socket Unix</span><span class="sxs-lookup"><span data-stu-id="098d7-941">Bind to a Unix socket</span></span>

<span data-ttu-id="098d7-942">Impostare l'ascolto su un socket Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="098d7-942">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="098d7-943">Porta 0</span><span class="sxs-lookup"><span data-stu-id="098d7-943">Port 0</span></span>

<span data-ttu-id="098d7-944">Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="098d7-944">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="098d7-945">L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="098d7-945">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="098d7-946">Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:</span><span class="sxs-lookup"><span data-stu-id="098d7-946">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="098d7-947">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="098d7-947">Limitations</span></span>

<span data-ttu-id="098d7-948">Configurare gli endpoint con i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-948">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="098d7-949">L'argomento della riga di comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-949">`--urls` command-line argument</span></span>
* <span data-ttu-id="098d7-950">La chiave di configurazione dell'host `urls`</span><span class="sxs-lookup"><span data-stu-id="098d7-950">`urls` host configuration key</span></span>
* <span data-ttu-id="098d7-951">La variabile di ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="098d7-951">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="098d7-952">Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="098d7-952">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="098d7-953">Tenere presenti, tuttavia, le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-953">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="098d7-954">HTTPS non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="098d7-954">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="098d7-955">Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-955">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="098d7-956">Configurazione dell'endpoint IIS</span><span class="sxs-lookup"><span data-stu-id="098d7-956">IIS endpoint configuration</span></span>

<span data-ttu-id="098d7-957">Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-957">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="098d7-958">Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="098d7-958">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="098d7-959">Configurazione del trasporto</span><span class="sxs-lookup"><span data-stu-id="098d7-959">Transport configuration</span></span>

<span data-ttu-id="098d7-960">Con la versione ASP.NET Core 2.1, il trasporto predefinito di Kestrel non si basa più su Libuv, ma su socket gestiti.</span><span class="sxs-lookup"><span data-stu-id="098d7-960">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="098d7-961">Si tratta di una modifica importante per le app ASP.NET 2.0 Core che vengono aggiornate alla versione 2.1, che chiamano <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> e dipendono da uno dei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="098d7-961">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="098d7-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (riferimento diretto al pacchetto)</span><span class="sxs-lookup"><span data-stu-id="098d7-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="098d7-963">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="098d7-963">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="098d7-964">Per i progetti che richiedono l'uso di libuv:</span><span class="sxs-lookup"><span data-stu-id="098d7-964">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="098d7-965">Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="098d7-965">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="098d7-966">Chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-966">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="098d7-967">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="098d7-967">URL prefixes</span></span>

<span data-ttu-id="098d7-968">Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="098d7-968">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="098d7-969">Sono validi solo i prefissi URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="098d7-969">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="098d7-970">Kestrel non supporta HTTPS quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="098d7-970">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="098d7-971">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-971">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="098d7-972">`0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-972">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="098d7-973">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-973">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="098d7-974">`[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.</span><span class="sxs-lookup"><span data-stu-id="098d7-974">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="098d7-975">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-975">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="098d7-976">I nomi host, `*` e `+` non sono casi particolari.</span><span class="sxs-lookup"><span data-stu-id="098d7-976">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="098d7-977">Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-977">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="098d7-978">Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="098d7-978">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="098d7-979">Una configurazione che prevede un proxy inverso richiede il [filtro host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="098d7-979">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="098d7-980">Nome host `localhost` con numero di porta o IP di loopback con numero di porta</span><span class="sxs-lookup"><span data-stu-id="098d7-980">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="098d7-981">Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6.</span><span class="sxs-lookup"><span data-stu-id="098d7-981">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="098d7-982">Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="098d7-982">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="098d7-983">Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="098d7-983">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="098d7-984">Filtro host</span><span class="sxs-lookup"><span data-stu-id="098d7-984">Host filtering</span></span>

<span data-ttu-id="098d7-985">Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host.</span><span class="sxs-lookup"><span data-stu-id="098d7-985">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="098d7-986">L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback.</span><span class="sxs-lookup"><span data-stu-id="098d7-986">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="098d7-987">Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici.</span><span class="sxs-lookup"><span data-stu-id="098d7-987">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="098d7-988">Le intestazioni `Host` non vengono convalidate.</span><span class="sxs-lookup"><span data-stu-id="098d7-988">`Host` headers aren't validated.</span></span>

<span data-ttu-id="098d7-989">Come soluzione alternativa, usare il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="098d7-989">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="098d7-990">Il middleware di filtro host viene fornito dal pacchetto [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , incluso nel [metapacchetto Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 o 2,2).</span><span class="sxs-lookup"><span data-stu-id="098d7-990">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="098d7-991">Il middleware viene aggiunto da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, che chiama <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="098d7-991">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="098d7-992">Per impostazione predefinita, il middleware di filtro host è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="098d7-992">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="098d7-993">Per abilitare il middleware, definire una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*.</span><span class="sxs-lookup"><span data-stu-id="098d7-993">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="098d7-994">Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:</span><span class="sxs-lookup"><span data-stu-id="098d7-994">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="098d7-995">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="098d7-995">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="098d7-996">Il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) include anche un'opzione <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="098d7-996">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="098d7-997">Il middleware di intestazioni inoltrate e il middleware di filtro host hanno funzionalità simili per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="098d7-997">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="098d7-998">L'impostazione di `AllowedHosts` con il middleware delle intestazioni inoltrate è appropriato se l'intestazione `Host` non viene mantenuta durante l'inoltro delle richieste con un server proxy inverso o il bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="098d7-998">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="098d7-999">L'impostazione di `AllowedHosts` con il middleware del filtro host è appropriata se si usa Kestrel come server perimetrale rivolto al pubblico o se l'intestazione `Host` viene inoltrata direttamente.</span><span class="sxs-lookup"><span data-stu-id="098d7-999">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="098d7-1000">Per altre informazioni sul middleware delle intestazioni inoltrate, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="098d7-1000">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="098d7-1001">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="098d7-1001">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* <span data-ttu-id="098d7-1002">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Sintassi e routing dei messaggi (sezione 5.4: Host))</span><span class="sxs-lookup"><span data-stu-id="098d7-1002">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
