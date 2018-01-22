---
title: Hosting in ASP.NET Core
author: guardrex
description: "Informazioni sull'host web in ASP.NET Core, che è responsabile per la gestione di app di avvio e durata."
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="caac5-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="caac5-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="caac5-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="caac5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="caac5-105">Le applicazioni ASP.NET Core configurare e avviare un *host*.</span><span class="sxs-lookup"><span data-stu-id="caac5-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="caac5-106">L'host è responsabile della gestione di avvio e la durata delle app.</span><span class="sxs-lookup"><span data-stu-id="caac5-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="caac5-107">Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="caac5-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="caac5-108">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="caac5-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="caac5-110">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="caac5-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="caac5-111">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="caac5-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="caac5-112">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="caac5-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="caac5-113">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="caac5-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="caac5-114">`CreateDefaultBuilder`esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="caac5-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="caac5-115">Configura [Kestrel](servers/kestrel.md) come server web.</span><span class="sxs-lookup"><span data-stu-id="caac5-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="caac5-116">Per le opzioni predefinite Kestrel, vedere [il Kestrel Opzioni sezione di implementazione del server web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="caac5-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="caac5-117">Imposta la radice del contenuto per il percorso restituito da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="caac5-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="caac5-118">Configurazione facoltativa di carichi da:</span><span class="sxs-lookup"><span data-stu-id="caac5-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="caac5-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="caac5-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="caac5-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="caac5-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="caac5-121">[Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="caac5-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="caac5-122">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="caac5-122">Environment variables.</span></span>
  * <span data-ttu-id="caac5-123">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="caac5-123">Command-line arguments.</span></span>
* <span data-ttu-id="caac5-124">Configura [registrazione](xref:fundamentals/logging/index) per l'output di console ed eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="caac5-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="caac5-125">La registrazione include [filtri log](xref:fundamentals/logging/index#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. { Ambiente} JSON* file.</span><span class="sxs-lookup"><span data-stu-id="caac5-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="caac5-126">Quando si esegue dietro IIS, abilita [integrazione IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="caac5-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="caac5-127">Consente di configurare il percorso di base e la porta di ascolto il server quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="caac5-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="caac5-128">Il modulo Crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="caac5-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="caac5-129">Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="caac5-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="caac5-130">Per le opzioni predefinite IIS, vedere [IIS Opzioni sezione dell'Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="caac5-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="caac5-131">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="caac5-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="caac5-132">Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="caac5-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="caac5-133">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="caac5-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="caac5-134">Per ulteriori informazioni sulla configurazione delle app, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="caac5-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="caac5-135">Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="caac5-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="caac5-136">Per ulteriori informazioni, vedere la scheda di 1. x di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-138">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="caac5-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="caac5-139">Creazione di un host è in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="caac5-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="caac5-140">Nei modelli di progetto, `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="caac5-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="caac5-141">`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="caac5-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="caac5-142">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="caac5-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="caac5-143">In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="caac5-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="caac5-144">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="caac5-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="caac5-145">La radice di contenuto predefinito viene ottenuta per `UseContentRoot` da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="caac5-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="caac5-146">Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="caac5-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="caac5-147">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="caac5-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="caac5-148">Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="caac5-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="caac5-149">`UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="caac5-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="caac5-150">`UseIISIntegration`Consente di configurare il percorso di base e la porta di ascolto il server quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="caac5-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="caac5-151">Per utilizzare IIS con ASP.NET Core, `UseKestrel` e `UseIISIntegration` deve essere specificato.</span><span class="sxs-lookup"><span data-stu-id="caac5-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="caac5-152">`UseIISIntegration`attiva solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="caac5-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="caac5-153">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="caac5-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="caac5-154">Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="caac5-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="caac5-155">Quando si configura un host, [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi possono essere forniti.</span><span class="sxs-lookup"><span data-stu-id="caac5-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="caac5-156">Se un `Startup` classe è specificata, deve definire un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="caac5-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="caac5-157">Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="caac5-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="caac5-158">Più chiamate a `ConfigureServices` aggiungere uno a altro.</span><span class="sxs-lookup"><span data-stu-id="caac5-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="caac5-159">Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="caac5-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="caac5-160">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="caac5-160">Host configuration values</span></span>

<span data-ttu-id="caac5-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa su approcci seguenti per impostare l'host di valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="caac5-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="caac5-162">Configurazione di generatore di host, che include variabili di ambiente con il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="caac5-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="caac5-163">Ad esempio `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="caac5-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="caac5-164">Metodi espliciti, ad esempio `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="caac5-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="caac5-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="caac5-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="caac5-166">Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa, indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="caac5-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="caac5-167">L'host usa indipendentemente dall'opzione imposta un valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="caac5-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="caac5-168">Per ulteriori informazioni, vedere [configurazione verrà ignorato](#overriding-configuration) nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="caac5-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="caac5-169">Acquisire gli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="caac5-169">Capture Startup Errors</span></span>

<span data-ttu-id="caac5-170">Questa impostazione Controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="caac5-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="caac5-171">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="caac5-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="caac5-172">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="caac5-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="caac5-173">**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="caac5-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="caac5-174">**Impostare utilizzando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="caac5-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="caac5-175">**Variabile di ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="caac5-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="caac5-176">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="caac5-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="caac5-177">Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="caac5-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="caac5-180">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="caac5-180">Content Root</span></span>

<span data-ttu-id="caac5-181">Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="caac5-182">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="caac5-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="caac5-183">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-183">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-184">**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.</span><span class="sxs-lookup"><span data-stu-id="caac5-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="caac5-185">**Impostare utilizzando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="caac5-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="caac5-186">**Variabile di ambiente**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="caac5-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="caac5-187">La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="caac5-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="caac5-188">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="caac5-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="caac5-191">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="caac5-191">Detailed Errors</span></span>

<span data-ttu-id="caac5-192">Determina se gli errori dettagliati devono essere acquisiti gli errori.</span><span class="sxs-lookup"><span data-stu-id="caac5-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="caac5-193">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="caac5-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="caac5-194">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="caac5-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="caac5-195">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="caac5-195">**Default**: false</span></span>  
<span data-ttu-id="caac5-196">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="caac5-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="caac5-197">**Variabile di ambiente**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="caac5-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="caac5-198">Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="caac5-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="caac5-201">Ambiente</span><span class="sxs-lookup"><span data-stu-id="caac5-201">Environment</span></span>

<span data-ttu-id="caac5-202">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="caac5-202">Sets the app's environment.</span></span>

<span data-ttu-id="caac5-203">**Chiave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="caac5-203">**Key**: environment</span></span>  
<span data-ttu-id="caac5-204">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-204">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-205">**Predefinito**: produzione</span><span class="sxs-lookup"><span data-stu-id="caac5-205">**Default**: Production</span></span>  
<span data-ttu-id="caac5-206">**Impostare utilizzando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="caac5-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="caac5-207">**Variabile di ambiente**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="caac5-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="caac5-208">Qualsiasi valore, è possibile impostare l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="caac5-208">The environment can be set to any value.</span></span> <span data-ttu-id="caac5-209">I valori definiti dal framework includono `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="caac5-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="caac5-210">I valori non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="caac5-210">Values aren't case sensitive.</span></span> <span data-ttu-id="caac5-211">Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="caac5-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="caac5-212">Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="caac5-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="caac5-213">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="caac5-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="caac5-216">Hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="caac5-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="caac5-217">Imposta gli assembly di avvio hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="caac5-218">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="caac5-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="caac5-219">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-219">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-220">**Predefinito**: una stringa vuota</span><span class="sxs-lookup"><span data-stu-id="caac5-220">**Default**: Empty string</span></span>  
<span data-ttu-id="caac5-221">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="caac5-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="caac5-222">**Variabile di ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="caac5-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="caac5-223">Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting.</span><span class="sxs-lookup"><span data-stu-id="caac5-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="caac5-224">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="caac5-225">Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="caac5-226">Se vengono specificati gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="caac5-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-229">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="caac5-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="caac5-230">Preferiscono gli URL di Hosting</span><span class="sxs-lookup"><span data-stu-id="caac5-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="caac5-231">Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="caac5-232">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="caac5-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="caac5-233">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="caac5-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="caac5-234">**Predefinito**: true</span><span class="sxs-lookup"><span data-stu-id="caac5-234">**Default**: true</span></span>  
<span data-ttu-id="caac5-235">**Impostare utilizzando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="caac5-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="caac5-236">**Variabile di ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="caac5-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="caac5-237">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-240">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="caac5-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="caac5-241">Impedire l'avvio di Hosting</span><span class="sxs-lookup"><span data-stu-id="caac5-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="caac5-242">Impedisce il caricamento automatico di hosting agli assembly di avvio, inclusi gli assembly di avvio configurati dall'assembly dell'applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="caac5-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="caac5-243">Vedere [aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:host-and-deploy/ihostingstartup) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="caac5-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="caac5-244">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="caac5-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="caac5-245">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="caac5-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="caac5-246">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="caac5-246">**Default**: false</span></span>  
<span data-ttu-id="caac5-247">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="caac5-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="caac5-248">**Variabile di ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="caac5-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="caac5-249">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-252">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="caac5-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="caac5-253">URL del server</span><span class="sxs-lookup"><span data-stu-id="caac5-253">Server URLs</span></span>

<span data-ttu-id="caac5-254">Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.</span><span class="sxs-lookup"><span data-stu-id="caac5-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="caac5-255">**Chiave**: URL</span><span class="sxs-lookup"><span data-stu-id="caac5-255">**Key**: urls</span></span>  
<span data-ttu-id="caac5-256">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-256">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-257">**Predefinito**: indirizzo http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="caac5-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="caac5-258">**Impostare utilizzando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="caac5-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="caac5-259">**Variabile di ambiente**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="caac5-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="caac5-260">Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="caac5-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="caac5-261">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="caac5-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="caac5-262">Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="caac5-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="caac5-263">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="caac5-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="caac5-264">I formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="caac5-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="caac5-266">Kestrel ha la propria configurazione di endpoint API.</span><span class="sxs-lookup"><span data-stu-id="caac5-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="caac5-267">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="caac5-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="caac5-269">Timeout di chiusura</span><span class="sxs-lookup"><span data-stu-id="caac5-269">Shutdown Timeout</span></span>

<span data-ttu-id="caac5-270">Specifica la quantità di tempo di attesa per l'host web per l'arresto.</span><span class="sxs-lookup"><span data-stu-id="caac5-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="caac5-271">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="caac5-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="caac5-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="caac5-272">**Type**: *int*</span></span>  
<span data-ttu-id="caac5-273">**Predefinito**: 5</span><span class="sxs-lookup"><span data-stu-id="caac5-273">**Default**: 5</span></span>  
<span data-ttu-id="caac5-274">**Impostare utilizzando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="caac5-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="caac5-275">**Variabile di ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="caac5-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="caac5-276">Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="caac5-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="caac5-277">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="caac5-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-280">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="caac5-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="caac5-281">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="caac5-281">Startup Assembly</span></span>

<span data-ttu-id="caac5-282">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="caac5-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="caac5-283">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="caac5-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="caac5-284">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-284">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-285">**Predefinito**: assembly dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="caac5-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="caac5-286">**Impostare utilizzando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="caac5-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="caac5-287">**Variabile di ambiente**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="caac5-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="caac5-288">L'assembly in base al nome (`string`) o un tipo (`TStartup`) è possibile fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="caac5-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="caac5-289">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="caac5-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="caac5-292">Radice Web</span><span class="sxs-lookup"><span data-stu-id="caac5-292">Web Root</span></span>

<span data-ttu-id="caac5-293">Imposta il percorso relativo alle risorse statiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="caac5-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="caac5-294">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="caac5-294">**Key**: webroot</span></span>  
<span data-ttu-id="caac5-295">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="caac5-295">**Type**: *string*</span></span>  
<span data-ttu-id="caac5-296">**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista.</span><span class="sxs-lookup"><span data-stu-id="caac5-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="caac5-297">Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="caac5-298">**Impostare utilizzando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="caac5-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="caac5-299">**Variabile di ambiente**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="caac5-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="caac5-302">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="caac5-302">Overriding configuration</span></span>

<span data-ttu-id="caac5-303">Utilizzare [configurazione](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="caac5-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="caac5-304">Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="caac5-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="caac5-305">Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="caac5-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="caac5-306">La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="caac5-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="caac5-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="caac5-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="caac5-309">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="caac5-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="caac5-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="caac5-312">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="caac5-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="caac5-313">Il [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="caac5-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="caac5-314">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="caac5-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="caac5-315">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="caac5-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="caac5-316">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="caac5-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="caac5-317">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="caac5-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="caac5-318">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="caac5-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="caac5-319">Per specificare l'host di eseguire in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="caac5-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="caac5-320">L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="caac5-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="caac5-321">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="caac5-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="caac5-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="caac5-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="caac5-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="caac5-323">**Run**</span></span>

<span data-ttu-id="caac5-324">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="caac5-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="caac5-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="caac5-325">**Start**</span></span>

<span data-ttu-id="caac5-326">Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="caac5-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="caac5-327">Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="caac5-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="caac5-328">L'app può inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="caac5-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="caac5-329">Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="caac5-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="caac5-330">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="caac5-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="caac5-331">Iniziare con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="caac5-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="caac5-332">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="caac5-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="caac5-333">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="caac5-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="caac5-334">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="caac5-335">**Start (url di stringa, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="caac5-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="caac5-336">Iniziare con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="caac5-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="caac5-337">Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="caac5-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="caac5-338">**Start (azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="caac5-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="caac5-339">Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="caac5-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="caac5-340">Utilizzare le seguenti richieste del browser con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="caac5-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="caac5-341">Richiesta</span><span class="sxs-lookup"><span data-stu-id="caac5-341">Request</span></span>                                    | <span data-ttu-id="caac5-342">Risposta</span><span class="sxs-lookup"><span data-stu-id="caac5-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="caac5-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="caac5-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="caac5-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="caac5-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="caac5-345">Genera un'eccezione con stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="caac5-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="caac5-346">Genera un'eccezione con stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="caac5-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="caac5-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="caac5-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="caac5-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="caac5-348">Hello World!</span></span>                             |

<span data-ttu-id="caac5-349">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="caac5-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="caac5-350">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="caac5-351">**Start (string url, azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="caac5-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="caac5-352">Utilizzare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="caac5-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="caac5-353">Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="caac5-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="caac5-354">**StartWith (azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="caac5-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="caac5-355">Fornire un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="caac5-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="caac5-356">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="caac5-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="caac5-357">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="caac5-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="caac5-358">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="caac5-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="caac5-359">**StartWith (string url, azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="caac5-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="caac5-360">Fornire un URL e un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="caac5-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="caac5-361">Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="caac5-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="caac5-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="caac5-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="caac5-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="caac5-363">**Run**</span></span>

<span data-ttu-id="caac5-364">Il `Run` metodo avvia l'app web e blocca il thread chiamante finché non viene chiuso l'host:</span><span class="sxs-lookup"><span data-stu-id="caac5-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="caac5-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="caac5-365">**Start**</span></span>

<span data-ttu-id="caac5-366">Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="caac5-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="caac5-367">Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="caac5-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="caac5-368">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="caac5-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="caac5-369">Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app.</span><span class="sxs-lookup"><span data-stu-id="caac5-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="caac5-370">Utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="caac5-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="caac5-371">Oggetto [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) può essere usato per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="caac5-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="caac5-372">In alternativa, inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="caac5-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="caac5-373">Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi.</span><span class="sxs-lookup"><span data-stu-id="caac5-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="caac5-374">Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="caac5-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="caac5-375">Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per la configurazione di pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="caac5-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="caac5-376">`IHostingEnvironment`possono essere inserite nella `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="caac5-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="caac5-377">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="caac5-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="caac5-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività di post-avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="caac5-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="caac5-379">Tre proprietà nell'interfaccia vengono utilizzati per registrare i token di annullamento `Action` metodi che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="caac5-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="caac5-380">È inoltre disponibile un `StopApplication` metodo.</span><span class="sxs-lookup"><span data-stu-id="caac5-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="caac5-381">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="caac5-381">Cancellation Token</span></span>    | <span data-ttu-id="caac5-382">Generato quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="caac5-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="caac5-383">L'host sia stato avviato.</span><span class="sxs-lookup"><span data-stu-id="caac5-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="caac5-384">L'host è in esecuzione un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="caac5-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="caac5-385">Stia ancora elaborando le richieste.</span><span class="sxs-lookup"><span data-stu-id="caac5-385">Requests may still be processing.</span></span> <span data-ttu-id="caac5-386">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="caac5-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="caac5-387">L'host di completamento in corso un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="caac5-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="caac5-388">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="caac5-388">All requests should be processed.</span></span> <span data-ttu-id="caac5-389">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="caac5-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="caac5-390">Metodo</span><span class="sxs-lookup"><span data-stu-id="caac5-390">Method</span></span>            | <span data-ttu-id="caac5-391">Operazione</span><span class="sxs-lookup"><span data-stu-id="caac5-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="caac5-392">Chiusura di richieste dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="caac5-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="caac5-393">Risoluzione dei problemi relativi a System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="caac5-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="caac5-394">**Si applica a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="caac5-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="caac5-395">Un host può essere compilato inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="caac5-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="caac5-396">Se l'host viene compilato in questo modo, potrebbe verificarsi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="caac5-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="caac5-397">Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="caac5-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="caac5-398">Se l'app inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente a `WebHostBuilder` con il nome di assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="caac5-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="caac5-399">In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="caac5-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="caac5-400">**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando l'applicazione non chiama `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="caac5-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="caac5-401">Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="caac5-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="caac5-402">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="caac5-402">Additional resources</span></span>

* [<span data-ttu-id="caac5-403">Host in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="caac5-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="caac5-404">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="caac5-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="caac5-405">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="caac5-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="caac5-406">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="caac5-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
