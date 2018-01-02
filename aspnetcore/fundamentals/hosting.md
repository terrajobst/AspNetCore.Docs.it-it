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
ms.openlocfilehash: 14e48adf5671a41ad6e135caeb4a87fdf7292aa6
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/20/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="4611e-104">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4611e-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="4611e-105">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4611e-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4611e-106">Le applicazioni ASP.NET Core configurare e avviare un *host*.</span><span class="sxs-lookup"><span data-stu-id="4611e-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="4611e-107">L'host è responsabile della gestione di avvio e la durata delle app.</span><span class="sxs-lookup"><span data-stu-id="4611e-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4611e-108">Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="4611e-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="4611e-109">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="4611e-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4611e-111">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4611e-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4611e-112">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="4611e-112">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="4611e-113">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4611e-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4611e-114">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="4611e-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="4611e-115">`CreateDefaultBuilder`esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="4611e-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="4611e-116">Configura [Kestrel](servers/kestrel.md) come server web.</span><span class="sxs-lookup"><span data-stu-id="4611e-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="4611e-117">Per le opzioni predefinite Kestrel, vedere [il Kestrel Opzioni sezione di implementazione del server web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="4611e-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="4611e-118">Imposta la radice del contenuto [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4611e-118">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="4611e-119">Configurazione facoltativa di carichi da:</span><span class="sxs-lookup"><span data-stu-id="4611e-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="4611e-120">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4611e-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="4611e-121">*appSettings. {Ambiente} JSON*.</span><span class="sxs-lookup"><span data-stu-id="4611e-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="4611e-122">[Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="4611e-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="4611e-123">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4611e-123">Environment variables.</span></span>
  * <span data-ttu-id="4611e-124">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4611e-124">Command-line arguments.</span></span>
* <span data-ttu-id="4611e-125">Configura [registrazione](xref:fundamentals/logging/index) per l'output di console ed eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="4611e-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="4611e-126">La registrazione include [filtri log](xref:fundamentals/logging/index#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. { Ambiente} JSON* file.</span><span class="sxs-lookup"><span data-stu-id="4611e-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="4611e-127">Quando si esegue dietro IIS, consente [integrazione IIS](xref:publishing/iis) configurando il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4611e-127">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="4611e-128">Il modulo Crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4611e-128">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="4611e-129">Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="4611e-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="4611e-130">Per le opzioni predefinite IIS, vedere [IIS Opzioni sezione dell'Host ASP.NET Core in Windows con IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="4611e-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="4611e-131">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="4611e-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4611e-132">La radice di contenuto predefinito è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4611e-132">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="4611e-133">La radice di contenuto predefinito (`Directory.GetCurrentDirectory`) comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="4611e-133">The default content root (`Directory.GetCurrentDirectory`) results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="4611e-134">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4611e-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4611e-135">Per ulteriori informazioni sulla configurazione delle app, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4611e-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="4611e-136">Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="4611e-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="4611e-137">Per ulteriori informazioni, vedere la scheda di 1. x di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-139">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4611e-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4611e-140">Creazione di un host è in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="4611e-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="4611e-141">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4611e-141">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4611e-142">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="4611e-142">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="4611e-143">`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4611e-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="4611e-144">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="4611e-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="4611e-145">In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4611e-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="4611e-146">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="4611e-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4611e-147">La radice di contenuto predefinito fornito a `UseContentRoot` è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="4611e-147">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="4611e-148">Ciò comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="4611e-148">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="4611e-149">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4611e-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4611e-150">Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="4611e-150">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="4611e-151">`UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="4611e-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="4611e-152">`UseIISIntegration`Consente di configurare il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="4611e-152">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="4611e-153">Per utilizzare IIS con ASP.NET Core, è necessario specificare sia `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="4611e-153">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="4611e-154">`UseIISIntegration`attiva solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4611e-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="4611e-155">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4611e-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="4611e-156">Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="4611e-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="4611e-157">Quando si configura un host, è possibile fornire [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi.</span><span class="sxs-lookup"><span data-stu-id="4611e-157">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="4611e-158">Se si specifica un `Startup` (classe), è necessario definire un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="4611e-158">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="4611e-159">Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="4611e-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="4611e-160">Più chiamate a `ConfigureServices` aggiungere uno a altro.</span><span class="sxs-lookup"><span data-stu-id="4611e-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="4611e-161">Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="4611e-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="4611e-162">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="4611e-162">Host configuration values</span></span>

<span data-ttu-id="4611e-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) fornisce approcci seguenti per impostare la maggior parte dei valori di configurazione disponibili per l'host:</span><span class="sxs-lookup"><span data-stu-id="4611e-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides the following approaches for setting most of the available configuration values for the host:</span></span>

* <span data-ttu-id="4611e-164">Variabili di ambiente con il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="4611e-164">Environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="4611e-165">Ad esempio `ASPNETCORE_DETAILEDERRORS`.</span><span class="sxs-lookup"><span data-stu-id="4611e-165">For example, `ASPNETCORE_DETAILEDERRORS`.</span></span>
* <span data-ttu-id="4611e-166">Metodi espliciti, ad esempio `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="4611e-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="4611e-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="4611e-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="4611e-168">Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa, indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="4611e-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="4611e-169">Acquisire gli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="4611e-169">Capture Startup Errors</span></span>

<span data-ttu-id="4611e-170">Questa impostazione Controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="4611e-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="4611e-171">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="4611e-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="4611e-172">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4611e-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4611e-173">**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="4611e-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="4611e-174">**Impostare utilizzando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="4611e-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="4611e-175">**Variabile di ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="4611e-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="4611e-176">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="4611e-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="4611e-177">Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="4611e-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="4611e-180">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="4611e-180">Content Root</span></span>

<span data-ttu-id="4611e-181">Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="4611e-182">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="4611e-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="4611e-183">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-183">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-184">**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.</span><span class="sxs-lookup"><span data-stu-id="4611e-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="4611e-185">**Impostare utilizzando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="4611e-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="4611e-186">**Variabile di ambiente**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="4611e-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="4611e-187">La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="4611e-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="4611e-188">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="4611e-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="4611e-191">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="4611e-191">Detailed Errors</span></span>

<span data-ttu-id="4611e-192">Determina se gli errori dettagliati devono essere acquisiti gli errori.</span><span class="sxs-lookup"><span data-stu-id="4611e-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="4611e-193">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="4611e-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="4611e-194">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4611e-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4611e-195">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="4611e-195">**Default**: false</span></span>  
<span data-ttu-id="4611e-196">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4611e-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4611e-197">**Variabile di ambiente**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="4611e-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="4611e-198">Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4611e-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="4611e-201">Ambiente</span><span class="sxs-lookup"><span data-stu-id="4611e-201">Environment</span></span>

<span data-ttu-id="4611e-202">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="4611e-202">Sets the app's environment.</span></span>

<span data-ttu-id="4611e-203">**Chiave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="4611e-203">**Key**: environment</span></span>  
<span data-ttu-id="4611e-204">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-204">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-205">**Predefinito**: produzione</span><span class="sxs-lookup"><span data-stu-id="4611e-205">**Default**: Production</span></span>  
<span data-ttu-id="4611e-206">**Impostare utilizzando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="4611e-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="4611e-207">**Variabile di ambiente**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="4611e-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="4611e-208">È possibile impostare il *ambiente* su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="4611e-208">You can set the *Environment* to any value.</span></span> <span data-ttu-id="4611e-209">I valori definiti dal framework includono `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="4611e-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="4611e-210">I valori non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4611e-210">Values aren't case sensitive.</span></span> <span data-ttu-id="4611e-211">Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4611e-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="4611e-212">Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="4611e-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="4611e-213">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="4611e-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="4611e-216">Hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="4611e-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="4611e-217">Imposta gli assembly di avvio hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="4611e-218">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="4611e-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="4611e-219">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-219">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-220">**Predefinito**: una stringa vuota</span><span class="sxs-lookup"><span data-stu-id="4611e-220">**Default**: Empty string</span></span>  
<span data-ttu-id="4611e-221">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4611e-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4611e-222">**Variabile di ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="4611e-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="4611e-223">Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting.</span><span class="sxs-lookup"><span data-stu-id="4611e-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="4611e-224">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="4611e-225">Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="4611e-226">Quando si specificano gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="4611e-226">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-229">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="4611e-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="4611e-230">Preferiscono gli URL di Hosting</span><span class="sxs-lookup"><span data-stu-id="4611e-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="4611e-231">Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="4611e-232">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="4611e-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="4611e-233">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4611e-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4611e-234">**Predefinito**: true</span><span class="sxs-lookup"><span data-stu-id="4611e-234">**Default**: true</span></span>  
<span data-ttu-id="4611e-235">**Impostare utilizzando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="4611e-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="4611e-236">**Variabile di ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="4611e-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="4611e-237">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-240">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="4611e-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="4611e-241">Impedire l'avvio di Hosting</span><span class="sxs-lookup"><span data-stu-id="4611e-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="4611e-242">Impedisce il caricamento automatico di hosting agli assembly di avvio, inclusi gli assembly di avvio configurati dall'assembly dell'applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="4611e-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="4611e-243">Vedere [aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:hosting/ihostingstartup) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="4611e-243">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="4611e-244">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="4611e-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="4611e-245">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4611e-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4611e-246">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="4611e-246">**Default**: false</span></span>  
<span data-ttu-id="4611e-247">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4611e-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4611e-248">**Variabile di ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="4611e-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="4611e-249">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-252">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="4611e-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="4611e-253">URL del server</span><span class="sxs-lookup"><span data-stu-id="4611e-253">Server URLs</span></span>

<span data-ttu-id="4611e-254">Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.</span><span class="sxs-lookup"><span data-stu-id="4611e-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="4611e-255">**Chiave**: URL</span><span class="sxs-lookup"><span data-stu-id="4611e-255">**Key**: urls</span></span>  
<span data-ttu-id="4611e-256">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-256">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-257">**Predefinito**: indirizzo http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="4611e-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="4611e-258">**Impostare utilizzando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="4611e-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="4611e-259">**Variabile di ambiente**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="4611e-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="4611e-260">Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="4611e-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="4611e-261">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="4611e-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="4611e-262">Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="4611e-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="4611e-263">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="4611e-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="4611e-264">I formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="4611e-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="4611e-266">Kestrel ha la propria configurazione di endpoint API.</span><span class="sxs-lookup"><span data-stu-id="4611e-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="4611e-267">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="4611e-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="4611e-269">Timeout di chiusura</span><span class="sxs-lookup"><span data-stu-id="4611e-269">Shutdown Timeout</span></span>

<span data-ttu-id="4611e-270">Specifica la quantità di tempo di attesa per l'host web per l'arresto.</span><span class="sxs-lookup"><span data-stu-id="4611e-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="4611e-271">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="4611e-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="4611e-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="4611e-272">**Type**: *int*</span></span>  
<span data-ttu-id="4611e-273">**Predefinito**: 5</span><span class="sxs-lookup"><span data-stu-id="4611e-273">**Default**: 5</span></span>  
<span data-ttu-id="4611e-274">**Impostare utilizzando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="4611e-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="4611e-275">**Variabile di ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="4611e-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="4611e-276">Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="4611e-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="4611e-277">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="4611e-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-280">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="4611e-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="4611e-281">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="4611e-281">Startup Assembly</span></span>

<span data-ttu-id="4611e-282">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="4611e-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="4611e-283">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="4611e-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="4611e-284">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-284">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-285">**Predefinito**: assembly dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4611e-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="4611e-286">**Impostare utilizzando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="4611e-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="4611e-287">**Variabile di ambiente**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="4611e-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="4611e-288">È possibile fare riferimento all'assembly in base al nome (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="4611e-288">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="4611e-289">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="4611e-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="4611e-292">Radice Web</span><span class="sxs-lookup"><span data-stu-id="4611e-292">Web Root</span></span>

<span data-ttu-id="4611e-293">Imposta il percorso relativo alle risorse statiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="4611e-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="4611e-294">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="4611e-294">**Key**: webroot</span></span>  
<span data-ttu-id="4611e-295">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="4611e-295">**Type**: *string*</span></span>  
<span data-ttu-id="4611e-296">**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista.</span><span class="sxs-lookup"><span data-stu-id="4611e-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="4611e-297">Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="4611e-298">**Impostare utilizzando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="4611e-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="4611e-299">**Variabile di ambiente**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="4611e-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="4611e-302">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="4611e-302">Overriding configuration</span></span>

<span data-ttu-id="4611e-303">Utilizzare [configurazione](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="4611e-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="4611e-304">Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="4611e-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="4611e-305">Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4611e-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="4611e-306">La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4611e-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4611e-308">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4611e-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4611e-309">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="4611e-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-311">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4611e-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4611e-312">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="4611e-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="4611e-313">Il `UseConfiguration` non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="4611e-313">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4611e-314">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="4611e-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="4611e-315">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="4611e-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="4611e-316">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="4611e-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="4611e-317">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="4611e-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4611e-318">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4611e-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="4611e-319">Per specificare l'host di eseguire in un URL specifico, è possibile passare il valore desiderato da un prompt dei comandi quando si esegue `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4611e-319">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="4611e-320">L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="4611e-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="4611e-321">Ordine di priorità</span><span class="sxs-lookup"><span data-stu-id="4611e-321">Ordering importance</span></span>

<span data-ttu-id="4611e-322">Alcune delle `WebHostBuilder` impostazioni vengono lette prima dalle variabili di ambiente, se impostata.</span><span class="sxs-lookup"><span data-stu-id="4611e-322">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="4611e-323">Queste variabili di ambiente utilizzano il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="4611e-323">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="4611e-324">Per impostare gli URL che il server utilizza per impostazione predefinita, si imposta `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="4611e-324">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="4611e-325">È possibile sostituire qualsiasi di questi valori di variabili di ambiente specificando una configurazione (utilizzando `UseConfiguration`) o impostando in modo esplicito il valore (con `UseSetting` o uno dei metodi di estensione esplicita, ad esempio `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="4611e-325">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="4611e-326">L'host usa indipendentemente dall'opzione imposta il valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="4611e-326">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="4611e-327">Se si desidera impostare l'URL predefinito a un valore a livello di codice ma consentono di eseguire l'override con la configurazione, è possibile utilizzare la configurazione della riga di comando dopo l'impostazione dell'URL.</span><span class="sxs-lookup"><span data-stu-id="4611e-327">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="4611e-328">Vedere [configurazione verrà ignorato](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="4611e-328">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="4611e-329">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="4611e-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4611e-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4611e-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4611e-331">**Run**</span><span class="sxs-lookup"><span data-stu-id="4611e-331">**Run**</span></span>

<span data-ttu-id="4611e-332">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="4611e-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4611e-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="4611e-333">**Start**</span></span>

<span data-ttu-id="4611e-334">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="4611e-334">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4611e-335">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="4611e-335">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="4611e-336">È possibile inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="4611e-336">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="4611e-337">Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="4611e-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="4611e-338">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="4611e-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="4611e-339">Iniziare con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4611e-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4611e-340">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4611e-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4611e-341">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4611e-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4611e-342">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4611e-343">**Start (url di stringa, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="4611e-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="4611e-344">Iniziare con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4611e-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4611e-345">Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4611e-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="4611e-346">**Start (azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4611e-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4611e-347">Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="4611e-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="4611e-348">Utilizzare le seguenti richieste del browser con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="4611e-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="4611e-349">Richiesta</span><span class="sxs-lookup"><span data-stu-id="4611e-349">Request</span></span>                                    | <span data-ttu-id="4611e-350">Risposta</span><span class="sxs-lookup"><span data-stu-id="4611e-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="4611e-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="4611e-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="4611e-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="4611e-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="4611e-353">Genera un'eccezione con stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="4611e-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="4611e-354">Genera un'eccezione con stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="4611e-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="4611e-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="4611e-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="4611e-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="4611e-356">Hello World!</span></span>                             |

<span data-ttu-id="4611e-357">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4611e-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4611e-358">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4611e-359">**Start (string url, azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4611e-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4611e-360">Utilizzare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4611e-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="4611e-361">Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4611e-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="4611e-362">**StartWith (azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="4611e-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4611e-363">Fornire un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4611e-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4611e-364">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4611e-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4611e-365">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4611e-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4611e-366">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="4611e-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4611e-367">**StartWith (string url, azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="4611e-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4611e-368">Fornire un URL e un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4611e-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4611e-369">Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4611e-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4611e-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4611e-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4611e-371">**Run**</span><span class="sxs-lookup"><span data-stu-id="4611e-371">**Run**</span></span>

<span data-ttu-id="4611e-372">Il `Run` metodo avvia l'app web e blocca il thread chiamante finché non viene chiuso l'host:</span><span class="sxs-lookup"><span data-stu-id="4611e-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4611e-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="4611e-373">**Start**</span></span>

<span data-ttu-id="4611e-374">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="4611e-374">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4611e-375">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="4611e-375">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="4611e-376">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="4611e-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="4611e-377">Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app.</span><span class="sxs-lookup"><span data-stu-id="4611e-377">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="4611e-378">È possibile utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="4611e-378">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="4611e-379">È possibile utilizzare un [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="4611e-379">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="4611e-380">In alternativa, è possibile inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4611e-380">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="4611e-381">Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi.</span><span class="sxs-lookup"><span data-stu-id="4611e-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="4611e-382">Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4611e-382">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="4611e-383">Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per impostare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="4611e-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="4611e-384">È possibile inserire `IHostingEnvironment` nel `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="4611e-384">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="4611e-385">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="4611e-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="4611e-386">Il [IApplicationLifetime interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente di eseguire attività di post-avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="4611e-386">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="4611e-387">Le tre proprietà nell'interfaccia sono token di annullamento che è possibile registrare con `Action` metodi per definire gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="4611e-387">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="4611e-388">È inoltre disponibile un `StopApplication` metodo.</span><span class="sxs-lookup"><span data-stu-id="4611e-388">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="4611e-389">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="4611e-389">Cancellation Token</span></span>    | <span data-ttu-id="4611e-390">Generato quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="4611e-390">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="4611e-391">L'host sia stato avviato.</span><span class="sxs-lookup"><span data-stu-id="4611e-391">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="4611e-392">L'host è in esecuzione un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4611e-392">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="4611e-393">Stia ancora elaborando le richieste.</span><span class="sxs-lookup"><span data-stu-id="4611e-393">Requests may still be processing.</span></span> <span data-ttu-id="4611e-394">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="4611e-394">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="4611e-395">L'host di completamento in corso un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4611e-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="4611e-396">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="4611e-396">All requests should be processed.</span></span> <span data-ttu-id="4611e-397">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="4611e-397">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="4611e-398">Metodo</span><span class="sxs-lookup"><span data-stu-id="4611e-398">Method</span></span>            | <span data-ttu-id="4611e-399">Operazione</span><span class="sxs-lookup"><span data-stu-id="4611e-399">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="4611e-400">Chiusura di richieste dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="4611e-400">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="4611e-401">Risoluzione dei problemi relativi a System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="4611e-401">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="4611e-402">**Si applica a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="4611e-402">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="4611e-403">Se si compila l'host inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`, potrebbe verificarsi l'errore seguente: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="4611e-403">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="4611e-404">Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="4611e-404">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="4611e-405">Se inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente al `WebHostBuilder` con il nome di assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="4611e-405">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="4611e-406">In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="4611e-406">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="4611e-407">**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando non si chiama `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="4611e-407">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="4611e-408">Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="4611e-408">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4611e-409">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4611e-409">Additional resources</span></span>

* [<span data-ttu-id="4611e-410">Pubblicare in Windows tramite IIS</span><span class="sxs-lookup"><span data-stu-id="4611e-410">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="4611e-411">Pubblicare in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="4611e-411">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="4611e-412">Pubblicare in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="4611e-412">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="4611e-413">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="4611e-413">Host in a Windows Service</span></span>](xref:hosting/windows-service)
