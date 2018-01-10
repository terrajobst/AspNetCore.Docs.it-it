---
title: Hosting in ASP.NET Core
author: guardrex
description: "Informazioni sull'host web in ASP.NET Core, che è responsabile per la gestione di app di avvio e durata."
keywords: Web di ASP.NET Core, host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 054b60206cafc3d6dd5775436995638d7f5700cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="465f1-104">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="465f1-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="465f1-105">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="465f1-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="465f1-106">Le applicazioni ASP.NET Core configurare e avviare un *host*.</span><span class="sxs-lookup"><span data-stu-id="465f1-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="465f1-107">L'host è responsabile della gestione di avvio e la durata delle app.</span><span class="sxs-lookup"><span data-stu-id="465f1-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="465f1-108">Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="465f1-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="465f1-109">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="465f1-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="465f1-111">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="465f1-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="465f1-112">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="465f1-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="465f1-113">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="465f1-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="465f1-114">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="465f1-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="465f1-115">`CreateDefaultBuilder`esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="465f1-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="465f1-116">Configura [Kestrel](servers/kestrel.md) come server web.</span><span class="sxs-lookup"><span data-stu-id="465f1-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="465f1-117">Per le opzioni predefinite Kestrel, vedere [il Kestrel Opzioni sezione di implementazione del server web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="465f1-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="465f1-118">Imposta la radice del contenuto per il percorso restituito da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="465f1-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="465f1-119">Configurazione facoltativa di carichi da:</span><span class="sxs-lookup"><span data-stu-id="465f1-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="465f1-120">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="465f1-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="465f1-121">*appSettings. {Ambiente} JSON*.</span><span class="sxs-lookup"><span data-stu-id="465f1-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="465f1-122">[Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="465f1-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="465f1-123">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="465f1-123">Environment variables.</span></span>
  * <span data-ttu-id="465f1-124">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="465f1-124">Command-line arguments.</span></span>
* <span data-ttu-id="465f1-125">Configura [registrazione](xref:fundamentals/logging/index) per l'output di console ed eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="465f1-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="465f1-126">La registrazione include [filtri log](xref:fundamentals/logging/index#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. { Ambiente} JSON* file.</span><span class="sxs-lookup"><span data-stu-id="465f1-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="465f1-127">Quando si esegue dietro IIS, abilita [integrazione IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="465f1-127">When running behind IIS, enables [IIS integration](xref:publishing/iis).</span></span> <span data-ttu-id="465f1-128">Consente di configurare il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="465f1-128">Configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="465f1-129">Il modulo Crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="465f1-129">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="465f1-130">Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="465f1-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="465f1-131">Per le opzioni predefinite IIS, vedere [IIS Opzioni sezione dell'Host ASP.NET Core in Windows con IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="465f1-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="465f1-132">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="465f1-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="465f1-133">Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="465f1-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="465f1-134">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="465f1-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="465f1-135">Per ulteriori informazioni sulla configurazione delle app, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="465f1-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="465f1-136">Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="465f1-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="465f1-137">Per ulteriori informazioni, vedere la scheda di 1. x di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-139">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="465f1-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="465f1-140">Creazione di un host è in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="465f1-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="465f1-141">Nei modelli di progetto, `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="465f1-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="465f1-142">`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="465f1-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="465f1-143">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="465f1-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="465f1-144">In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="465f1-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="465f1-145">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="465f1-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="465f1-146">La radice di contenuto predefinito viene ottenuta per `UseContentRoot` da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="465f1-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="465f1-147">Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="465f1-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="465f1-148">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="465f1-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="465f1-149">Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="465f1-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="465f1-150">`UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="465f1-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="465f1-151">`UseIISIntegration`Consente di configurare il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="465f1-151">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="465f1-152">Per utilizzare IIS con ASP.NET Core, `UseKestrel` e `UseIISIntegration` deve essere specificato.</span><span class="sxs-lookup"><span data-stu-id="465f1-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="465f1-153">`UseIISIntegration`attiva solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="465f1-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="465f1-154">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="465f1-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="465f1-155">Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="465f1-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="465f1-156">Quando si configura un host, [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi possono essere forniti.</span><span class="sxs-lookup"><span data-stu-id="465f1-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="465f1-157">Se un `Startup` classe è specificata, deve definire un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="465f1-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="465f1-158">Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="465f1-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="465f1-159">Più chiamate a `ConfigureServices` aggiungere uno a altro.</span><span class="sxs-lookup"><span data-stu-id="465f1-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="465f1-160">Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="465f1-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="465f1-161">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="465f1-161">Host configuration values</span></span>

<span data-ttu-id="465f1-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa su approcci seguenti per impostare l'host di valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="465f1-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="465f1-163">Configurazione di generatore di host, che include variabili di ambiente con il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="465f1-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="465f1-164">Ad esempio `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="465f1-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="465f1-165">Metodi espliciti, ad esempio `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="465f1-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="465f1-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="465f1-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="465f1-167">Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa, indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="465f1-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="465f1-168">L'host usa indipendentemente dall'opzione imposta un valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="465f1-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="465f1-169">Per ulteriori informazioni, vedere [configurazione verrà ignorato](#overriding-configuration) nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="465f1-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="465f1-170">Acquisire gli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="465f1-170">Capture Startup Errors</span></span>

<span data-ttu-id="465f1-171">Questa impostazione Controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="465f1-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="465f1-172">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="465f1-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="465f1-173">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="465f1-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="465f1-174">**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="465f1-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="465f1-175">**Impostare utilizzando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="465f1-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="465f1-176">**Variabile di ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="465f1-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="465f1-177">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="465f1-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="465f1-178">Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="465f1-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="465f1-181">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="465f1-181">Content Root</span></span>

<span data-ttu-id="465f1-182">Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="465f1-183">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="465f1-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="465f1-184">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-184">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-185">**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.</span><span class="sxs-lookup"><span data-stu-id="465f1-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="465f1-186">**Impostare utilizzando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="465f1-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="465f1-187">**Variabile di ambiente**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="465f1-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="465f1-188">La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="465f1-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="465f1-189">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="465f1-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="465f1-192">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="465f1-192">Detailed Errors</span></span>

<span data-ttu-id="465f1-193">Determina se gli errori dettagliati devono essere acquisiti gli errori.</span><span class="sxs-lookup"><span data-stu-id="465f1-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="465f1-194">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="465f1-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="465f1-195">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="465f1-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="465f1-196">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="465f1-196">**Default**: false</span></span>  
<span data-ttu-id="465f1-197">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="465f1-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="465f1-198">**Variabile di ambiente**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="465f1-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="465f1-199">Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="465f1-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="465f1-202">Ambiente</span><span class="sxs-lookup"><span data-stu-id="465f1-202">Environment</span></span>

<span data-ttu-id="465f1-203">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="465f1-203">Sets the app's environment.</span></span>

<span data-ttu-id="465f1-204">**Chiave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="465f1-204">**Key**: environment</span></span>  
<span data-ttu-id="465f1-205">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-205">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-206">**Predefinito**: produzione</span><span class="sxs-lookup"><span data-stu-id="465f1-206">**Default**: Production</span></span>  
<span data-ttu-id="465f1-207">**Impostare utilizzando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="465f1-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="465f1-208">**Variabile di ambiente**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="465f1-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="465f1-209">Qualsiasi valore, è possibile impostare l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="465f1-209">The environment can be set to any value.</span></span> <span data-ttu-id="465f1-210">I valori definiti dal framework includono `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="465f1-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="465f1-211">I valori non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="465f1-211">Values aren't case sensitive.</span></span> <span data-ttu-id="465f1-212">Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="465f1-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="465f1-213">Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="465f1-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="465f1-214">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="465f1-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="465f1-217">Hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="465f1-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="465f1-218">Imposta gli assembly di avvio hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="465f1-219">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="465f1-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="465f1-220">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-220">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-221">**Predefinito**: una stringa vuota</span><span class="sxs-lookup"><span data-stu-id="465f1-221">**Default**: Empty string</span></span>  
<span data-ttu-id="465f1-222">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="465f1-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="465f1-223">**Variabile di ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="465f1-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="465f1-224">Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting.</span><span class="sxs-lookup"><span data-stu-id="465f1-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="465f1-225">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="465f1-226">Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="465f1-227">Se vengono specificati gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="465f1-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-230">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="465f1-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="465f1-231">Preferiscono gli URL di Hosting</span><span class="sxs-lookup"><span data-stu-id="465f1-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="465f1-232">Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="465f1-233">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="465f1-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="465f1-234">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="465f1-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="465f1-235">**Predefinito**: true</span><span class="sxs-lookup"><span data-stu-id="465f1-235">**Default**: true</span></span>  
<span data-ttu-id="465f1-236">**Impostare utilizzando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="465f1-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="465f1-237">**Variabile di ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="465f1-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="465f1-238">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-241">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="465f1-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="465f1-242">Impedire l'avvio di Hosting</span><span class="sxs-lookup"><span data-stu-id="465f1-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="465f1-243">Impedisce il caricamento automatico di hosting agli assembly di avvio, inclusi gli assembly di avvio configurati dall'assembly dell'applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="465f1-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="465f1-244">Vedere [aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:hosting/ihostingstartup) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="465f1-244">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="465f1-245">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="465f1-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="465f1-246">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="465f1-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="465f1-247">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="465f1-247">**Default**: false</span></span>  
<span data-ttu-id="465f1-248">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="465f1-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="465f1-249">**Variabile di ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="465f1-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="465f1-250">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-253">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="465f1-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="465f1-254">URL del server</span><span class="sxs-lookup"><span data-stu-id="465f1-254">Server URLs</span></span>

<span data-ttu-id="465f1-255">Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.</span><span class="sxs-lookup"><span data-stu-id="465f1-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="465f1-256">**Chiave**: URL</span><span class="sxs-lookup"><span data-stu-id="465f1-256">**Key**: urls</span></span>  
<span data-ttu-id="465f1-257">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-257">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-258">**Predefinito**: indirizzo http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="465f1-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="465f1-259">**Impostare utilizzando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="465f1-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="465f1-260">**Variabile di ambiente**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="465f1-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="465f1-261">Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="465f1-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="465f1-262">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="465f1-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="465f1-263">Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="465f1-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="465f1-264">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="465f1-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="465f1-265">I formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="465f1-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="465f1-267">Kestrel ha la propria configurazione di endpoint API.</span><span class="sxs-lookup"><span data-stu-id="465f1-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="465f1-268">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="465f1-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="465f1-270">Timeout di chiusura</span><span class="sxs-lookup"><span data-stu-id="465f1-270">Shutdown Timeout</span></span>

<span data-ttu-id="465f1-271">Specifica la quantità di tempo di attesa per l'host web per l'arresto.</span><span class="sxs-lookup"><span data-stu-id="465f1-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="465f1-272">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="465f1-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="465f1-273">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="465f1-273">**Type**: *int*</span></span>  
<span data-ttu-id="465f1-274">**Predefinito**: 5</span><span class="sxs-lookup"><span data-stu-id="465f1-274">**Default**: 5</span></span>  
<span data-ttu-id="465f1-275">**Impostare utilizzando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="465f1-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="465f1-276">**Variabile di ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="465f1-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="465f1-277">Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="465f1-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="465f1-278">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="465f1-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-281">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="465f1-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="465f1-282">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="465f1-282">Startup Assembly</span></span>

<span data-ttu-id="465f1-283">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="465f1-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="465f1-284">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="465f1-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="465f1-285">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-285">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-286">**Predefinito**: assembly dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="465f1-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="465f1-287">**Impostare utilizzando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="465f1-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="465f1-288">**Variabile di ambiente**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="465f1-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="465f1-289">L'assembly in base al nome (`string`) o un tipo (`TStartup`) è possibile fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="465f1-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="465f1-290">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="465f1-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="465f1-293">Radice Web</span><span class="sxs-lookup"><span data-stu-id="465f1-293">Web Root</span></span>

<span data-ttu-id="465f1-294">Imposta il percorso relativo alle risorse statiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="465f1-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="465f1-295">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="465f1-295">**Key**: webroot</span></span>  
<span data-ttu-id="465f1-296">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="465f1-296">**Type**: *string*</span></span>  
<span data-ttu-id="465f1-297">**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista.</span><span class="sxs-lookup"><span data-stu-id="465f1-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="465f1-298">Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="465f1-299">**Impostare utilizzando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="465f1-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="465f1-300">**Variabile di ambiente**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="465f1-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="465f1-303">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="465f1-303">Overriding configuration</span></span>

<span data-ttu-id="465f1-304">Utilizzare [configurazione](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="465f1-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="465f1-305">Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="465f1-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="465f1-306">Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="465f1-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="465f1-307">La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="465f1-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="465f1-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="465f1-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="465f1-310">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="465f1-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="465f1-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="465f1-313">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="465f1-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="465f1-314">Il [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="465f1-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="465f1-315">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="465f1-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="465f1-316">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="465f1-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="465f1-317">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="465f1-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="465f1-318">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="465f1-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="465f1-319">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="465f1-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="465f1-320">Per specificare l'host di eseguire in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="465f1-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="465f1-321">L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="465f1-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="465f1-322">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="465f1-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="465f1-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="465f1-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="465f1-324">**Run**</span><span class="sxs-lookup"><span data-stu-id="465f1-324">**Run**</span></span>

<span data-ttu-id="465f1-325">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="465f1-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="465f1-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="465f1-326">**Start**</span></span>

<span data-ttu-id="465f1-327">Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="465f1-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="465f1-328">Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="465f1-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="465f1-329">L'app può inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="465f1-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="465f1-330">Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="465f1-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="465f1-331">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="465f1-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="465f1-332">Iniziare con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="465f1-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-333">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="465f1-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="465f1-334">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="465f1-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="465f1-335">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="465f1-336">**Start (url di stringa, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="465f1-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="465f1-337">Iniziare con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="465f1-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-338">Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="465f1-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="465f1-339">**Start (azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="465f1-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="465f1-340">Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="465f1-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-341">Utilizzare le seguenti richieste del browser con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="465f1-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="465f1-342">Richiesta</span><span class="sxs-lookup"><span data-stu-id="465f1-342">Request</span></span>                                    | <span data-ttu-id="465f1-343">Risposta</span><span class="sxs-lookup"><span data-stu-id="465f1-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="465f1-344">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="465f1-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="465f1-345">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="465f1-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="465f1-346">Genera un'eccezione con stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="465f1-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="465f1-347">Genera un'eccezione con stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="465f1-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="465f1-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="465f1-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="465f1-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="465f1-349">Hello World!</span></span>                             |

<span data-ttu-id="465f1-350">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="465f1-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="465f1-351">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="465f1-352">**Start (string url, azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="465f1-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="465f1-353">Utilizzare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="465f1-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-354">Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="465f1-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="465f1-355">**StartWith (azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="465f1-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="465f1-356">Fornire un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="465f1-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-357">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="465f1-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="465f1-358">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="465f1-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="465f1-359">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="465f1-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="465f1-360">**StartWith (string url, azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="465f1-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="465f1-361">Fornire un URL e un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="465f1-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="465f1-362">Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="465f1-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="465f1-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="465f1-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="465f1-364">**Run**</span><span class="sxs-lookup"><span data-stu-id="465f1-364">**Run**</span></span>

<span data-ttu-id="465f1-365">Il `Run` metodo avvia l'app web e blocca il thread chiamante finché non viene chiuso l'host:</span><span class="sxs-lookup"><span data-stu-id="465f1-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="465f1-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="465f1-366">**Start**</span></span>

<span data-ttu-id="465f1-367">Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="465f1-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="465f1-368">Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="465f1-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="465f1-369">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="465f1-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="465f1-370">Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app.</span><span class="sxs-lookup"><span data-stu-id="465f1-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="465f1-371">Utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="465f1-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="465f1-372">Oggetto [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) può essere usato per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="465f1-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="465f1-373">In alternativa, inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="465f1-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="465f1-374">Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi.</span><span class="sxs-lookup"><span data-stu-id="465f1-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="465f1-375">Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="465f1-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="465f1-376">Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per la configurazione di pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="465f1-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="465f1-377">`IHostingEnvironment`possono essere inserite nella `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="465f1-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="465f1-378">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="465f1-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="465f1-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività di post-avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="465f1-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="465f1-380">Tre proprietà nell'interfaccia vengono utilizzati per registrare i token di annullamento `Action` metodi che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="465f1-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="465f1-381">È inoltre disponibile un `StopApplication` metodo.</span><span class="sxs-lookup"><span data-stu-id="465f1-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="465f1-382">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="465f1-382">Cancellation Token</span></span>    | <span data-ttu-id="465f1-383">Generato quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="465f1-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="465f1-384">L'host sia stato avviato.</span><span class="sxs-lookup"><span data-stu-id="465f1-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="465f1-385">L'host è in esecuzione un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="465f1-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="465f1-386">Stia ancora elaborando le richieste.</span><span class="sxs-lookup"><span data-stu-id="465f1-386">Requests may still be processing.</span></span> <span data-ttu-id="465f1-387">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="465f1-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="465f1-388">L'host di completamento in corso un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="465f1-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="465f1-389">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="465f1-389">All requests should be processed.</span></span> <span data-ttu-id="465f1-390">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="465f1-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="465f1-391">Metodo</span><span class="sxs-lookup"><span data-stu-id="465f1-391">Method</span></span>            | <span data-ttu-id="465f1-392">Operazione</span><span class="sxs-lookup"><span data-stu-id="465f1-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="465f1-393">Chiusura di richieste dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="465f1-393">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="465f1-394">Risoluzione dei problemi relativi a System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="465f1-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="465f1-395">**Si applica a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="465f1-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="465f1-396">Un host può essere compilato inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="465f1-396">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="465f1-397">Se l'host viene compilato in questo modo, potrebbe verificarsi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="465f1-397">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="465f1-398">Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="465f1-398">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="465f1-399">Se l'app inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente a `WebHostBuilder` con il nome di assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="465f1-399">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="465f1-400">In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="465f1-400">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="465f1-401">**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando l'applicazione non chiama `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="465f1-401">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="465f1-402">Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="465f1-402">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="465f1-403">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="465f1-403">Additional resources</span></span>

* [<span data-ttu-id="465f1-404">Pubblicare in Windows tramite IIS</span><span class="sxs-lookup"><span data-stu-id="465f1-404">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="465f1-405">Pubblicare in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="465f1-405">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="465f1-406">Pubblicare in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="465f1-406">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="465f1-407">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="465f1-407">Host in a Windows Service</span></span>](xref:hosting/windows-service)
