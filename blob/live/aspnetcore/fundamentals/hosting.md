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
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="62ca7-104">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62ca7-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="62ca7-105">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62ca7-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="62ca7-106">Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="62ca7-107">Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="62ca7-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="62ca7-108">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="62ca7-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62ca7-110">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="62ca7-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="62ca7-111">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="62ca7-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="62ca7-112">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="62ca7-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="62ca7-113">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="62ca7-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="62ca7-114">`CreateDefaultBuilder`esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="62ca7-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="62ca7-115">Configura [Kestrel](servers/kestrel.md) come server web.</span><span class="sxs-lookup"><span data-stu-id="62ca7-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="62ca7-116">Per le opzioni predefinite Kestrel, vedere [il Kestrel Opzioni sezione di implementazione del server web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="62ca7-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="62ca7-117">Imposta la radice del contenuto [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="62ca7-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="62ca7-118">Configurazione facoltativa di carichi da:</span><span class="sxs-lookup"><span data-stu-id="62ca7-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="62ca7-119">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="62ca7-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="62ca7-120">*appSettings. {Ambiente} JSON*.</span><span class="sxs-lookup"><span data-stu-id="62ca7-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="62ca7-121">[Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="62ca7-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="62ca7-122">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="62ca7-122">Environment variables.</span></span>
  * <span data-ttu-id="62ca7-123">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="62ca7-123">Command-line arguments.</span></span>
* <span data-ttu-id="62ca7-124">Configura [registrazione](xref:fundamentals/logging/index) per l'output di console ed eseguire il debug con [filtri log](xref:fundamentals/logging/index#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. {Ambiente} JSON* file.</span><span class="sxs-lookup"><span data-stu-id="62ca7-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="62ca7-125">Quando si esegue dietro IIS, consente [integrazione IIS](xref:publishing/iis) configurando il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="62ca7-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="62ca7-126">Il modulo Crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="62ca7-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="62ca7-127">Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="62ca7-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="62ca7-128">Per le opzioni predefinite IIS, vedere [IIS Opzioni sezione dell'Host ASP.NET Core in Windows con IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="62ca7-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="62ca7-129">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="62ca7-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="62ca7-130">La radice di contenuto predefinito è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="62ca7-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="62ca7-131">Ciò comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="62ca7-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="62ca7-132">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="62ca7-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="62ca7-133">Vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration/index) per ulteriori informazioni sulla configurazione delle app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="62ca7-134">Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="62ca7-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="62ca7-135">Vedere la scheda di 1. x di ASP.NET Core per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="62ca7-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-137">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="62ca7-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="62ca7-138">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="62ca7-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="62ca7-139">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="62ca7-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="62ca7-140">Nell'esempio *Program.cs* viene illustrato come utilizzare `WebHostBuilder` per compilare l'host:</span><span class="sxs-lookup"><span data-stu-id="62ca7-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="62ca7-141">`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="62ca7-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="62ca7-142">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="62ca7-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="62ca7-143">In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="62ca7-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="62ca7-144">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="62ca7-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="62ca7-145">La radice di contenuto predefinito fornito a `UseContentRoot` è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="62ca7-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="62ca7-146">Ciò comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="62ca7-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="62ca7-147">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="62ca7-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="62ca7-148">Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="62ca7-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="62ca7-149">`UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="62ca7-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="62ca7-150">`UseIISIntegration`Consente di configurare il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="62ca7-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="62ca7-151">Per utilizzare IIS con ASP.NET Core, è necessario specificare sia `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="62ca7-152">`UseIISIntegration`attiva solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="62ca7-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="62ca7-153">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="62ca7-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="62ca7-154">Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="62ca7-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="62ca7-155">Quando si configura un host, è possibile fornire [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi.</span><span class="sxs-lookup"><span data-stu-id="62ca7-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="62ca7-156">Se si specifica un `Startup` (classe), è necessario definire un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="62ca7-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="62ca7-157">Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="62ca7-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="62ca7-158">Più chiamate a `ConfigureServices` aggiungere uno a altro.</span><span class="sxs-lookup"><span data-stu-id="62ca7-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="62ca7-159">Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="62ca7-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="62ca7-160">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="62ca7-160">Host configuration values</span></span>

<span data-ttu-id="62ca7-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) fornisce metodi per l'impostazione che può anche essere impostata direttamente con la maggior parte dei valori di configurazione disponibili per l'host, [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="62ca7-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="62ca7-162">Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa (tra virgolette) indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="62ca7-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="62ca7-163">Acquisire gli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="62ca7-163">Capture Startup Errors</span></span>

<span data-ttu-id="62ca7-164">Questa impostazione Controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="62ca7-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="62ca7-165">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="62ca7-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="62ca7-166">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="62ca7-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62ca7-167">**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="62ca7-168">**Impostare utilizzando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="62ca7-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="62ca7-169">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="62ca7-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="62ca7-170">Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="62ca7-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="62ca7-173">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="62ca7-173">Content Root</span></span>

<span data-ttu-id="62ca7-174">Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62ca7-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="62ca7-175">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="62ca7-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="62ca7-176">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-176">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-177">**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="62ca7-178">**Impostare utilizzando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="62ca7-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="62ca7-179">La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="62ca7-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="62ca7-180">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="62ca7-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="62ca7-183">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="62ca7-183">Detailed Errors</span></span>

<span data-ttu-id="62ca7-184">Determina se gli errori dettagliati devono essere acquisiti gli errori.</span><span class="sxs-lookup"><span data-stu-id="62ca7-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="62ca7-185">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="62ca7-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="62ca7-186">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="62ca7-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62ca7-187">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="62ca7-187">**Default**: false</span></span>  
<span data-ttu-id="62ca7-188">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62ca7-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="62ca7-189">Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="62ca7-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="62ca7-192">Ambiente</span><span class="sxs-lookup"><span data-stu-id="62ca7-192">Environment</span></span>

<span data-ttu-id="62ca7-193">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-193">Sets the app's environment.</span></span>

<span data-ttu-id="62ca7-194">**Chiave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="62ca7-194">**Key**: environment</span></span>  
<span data-ttu-id="62ca7-195">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-195">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-196">**Predefinito**: produzione</span><span class="sxs-lookup"><span data-stu-id="62ca7-196">**Default**: Production</span></span>  
<span data-ttu-id="62ca7-197">**Impostare utilizzando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="62ca7-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="62ca7-198">È possibile impostare il *ambiente* su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="62ca7-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="62ca7-199">I valori definiti dal framework includono `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="62ca7-200">I valori non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="62ca7-200">Values aren't case sensitive.</span></span> <span data-ttu-id="62ca7-201">Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="62ca7-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="62ca7-202">Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="62ca7-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="62ca7-203">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="62ca7-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="62ca7-206">Hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="62ca7-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="62ca7-207">Imposta gli assembly di avvio hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="62ca7-208">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="62ca7-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="62ca7-209">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-209">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-210">**Predefinito**: una stringa vuota</span><span class="sxs-lookup"><span data-stu-id="62ca7-210">**Default**: Empty string</span></span>  
<span data-ttu-id="62ca7-211">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62ca7-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="62ca7-212">Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting.</span><span class="sxs-lookup"><span data-stu-id="62ca7-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="62ca7-213">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="62ca7-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="62ca7-214">Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="62ca7-215">Quando si specificano gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="62ca7-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-218">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="62ca7-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="62ca7-219">Preferiscono gli URL di Hosting</span><span class="sxs-lookup"><span data-stu-id="62ca7-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="62ca7-220">Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="62ca7-221">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="62ca7-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="62ca7-222">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="62ca7-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62ca7-223">**Predefinito**: true</span><span class="sxs-lookup"><span data-stu-id="62ca7-223">**Default**: true</span></span>  
<span data-ttu-id="62ca7-224">**Impostare utilizzando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="62ca7-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="62ca7-225">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="62ca7-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-228">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="62ca7-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="62ca7-229">Impedire l'avvio di Hosting</span><span class="sxs-lookup"><span data-stu-id="62ca7-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="62ca7-230">Impedisce il caricamento automatico di hosting agli assembly di avvio, inclusi gli assembly di avvio configurati dall'assembly dell'applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="62ca7-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="62ca7-231">Vedere [aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:hosting/ihostingstartup) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="62ca7-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="62ca7-232">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="62ca7-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="62ca7-233">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="62ca7-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62ca7-234">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="62ca7-234">**Default**: false</span></span>  
<span data-ttu-id="62ca7-235">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62ca7-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="62ca7-236">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="62ca7-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-237">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-238">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-239">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="62ca7-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="62ca7-240">URL del server</span><span class="sxs-lookup"><span data-stu-id="62ca7-240">Server URLs</span></span>

<span data-ttu-id="62ca7-241">Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.</span><span class="sxs-lookup"><span data-stu-id="62ca7-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="62ca7-242">**Chiave**: URL</span><span class="sxs-lookup"><span data-stu-id="62ca7-242">**Key**: urls</span></span>  
<span data-ttu-id="62ca7-243">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-243">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-244">**Predefinito**: indirizzo http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="62ca7-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="62ca7-245">**Impostare utilizzando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="62ca7-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="62ca7-246">Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="62ca7-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="62ca7-247">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="62ca7-248">Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="62ca7-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="62ca7-249">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="62ca7-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="62ca7-250">I formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="62ca7-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="62ca7-252">Kestrel ha la propria configurazione di endpoint API.</span><span class="sxs-lookup"><span data-stu-id="62ca7-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="62ca7-253">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="62ca7-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="62ca7-255">Timeout di chiusura</span><span class="sxs-lookup"><span data-stu-id="62ca7-255">Shutdown Timeout</span></span>

<span data-ttu-id="62ca7-256">Specifica la quantità di tempo di attesa per l'host web per l'arresto.</span><span class="sxs-lookup"><span data-stu-id="62ca7-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="62ca7-257">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="62ca7-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="62ca7-258">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="62ca7-258">**Type**: *int*</span></span>  
<span data-ttu-id="62ca7-259">**Predefinito**: 5</span><span class="sxs-lookup"><span data-stu-id="62ca7-259">**Default**: 5</span></span>  
<span data-ttu-id="62ca7-260">**Impostare utilizzando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="62ca7-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="62ca7-261">Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="62ca7-262">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="62ca7-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-263">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-264">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-265">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="62ca7-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="62ca7-266">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="62ca7-266">Startup Assembly</span></span>

<span data-ttu-id="62ca7-267">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="62ca7-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="62ca7-268">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="62ca7-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="62ca7-269">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-269">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-270">**Predefinito**: assembly dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="62ca7-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="62ca7-271">**Impostare utilizzando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="62ca7-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="62ca7-272">È possibile fare riferimento all'assembly in base al nome (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="62ca7-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="62ca7-273">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="62ca7-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-274">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-275">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="62ca7-276">Radice Web</span><span class="sxs-lookup"><span data-stu-id="62ca7-276">Web Root</span></span>

<span data-ttu-id="62ca7-277">Imposta il percorso relativo alle risorse statiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="62ca7-278">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="62ca7-278">**Key**: webroot</span></span>  
<span data-ttu-id="62ca7-279">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="62ca7-279">**Type**: *string*</span></span>  
<span data-ttu-id="62ca7-280">**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista.</span><span class="sxs-lookup"><span data-stu-id="62ca7-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="62ca7-281">Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="62ca7-282">**Impostare utilizzando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="62ca7-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-284">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="62ca7-285">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="62ca7-285">Overriding configuration</span></span>

<span data-ttu-id="62ca7-286">Utilizzare [configurazione](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="62ca7-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="62ca7-287">Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="62ca7-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="62ca7-288">Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="62ca7-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="62ca7-289">La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62ca7-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="62ca7-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="62ca7-292">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="62ca7-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-293">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-294">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="62ca7-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="62ca7-295">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="62ca7-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="62ca7-296">Il `UseConfiguration` non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="62ca7-297">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="62ca7-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="62ca7-298">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="62ca7-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="62ca7-299">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="62ca7-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="62ca7-300">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="62ca7-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="62ca7-301">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="62ca7-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="62ca7-302">Per specificare l'host di eseguire in un URL specifico, è possibile passare il valore desiderato da un prompt dei comandi quando si esegue `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="62ca7-303">L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="62ca7-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="62ca7-304">Ordine di priorità</span><span class="sxs-lookup"><span data-stu-id="62ca7-304">Ordering importance</span></span>

<span data-ttu-id="62ca7-305">Alcune delle `WebHostBuilder` impostazioni vengono lette prima dalle variabili di ambiente, se impostata.</span><span class="sxs-lookup"><span data-stu-id="62ca7-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="62ca7-306">Queste variabili di ambiente utilizzano il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="62ca7-307">Per impostare gli URL che il server utilizza per impostazione predefinita, si imposta `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="62ca7-308">È possibile sostituire qualsiasi di questi valori di variabili di ambiente specificando una configurazione (utilizzando `UseConfiguration`) o impostando in modo esplicito il valore (con `UseSetting` o uno dei metodi di estensione esplicita, ad esempio `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="62ca7-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="62ca7-309">L'host usa indipendentemente dall'opzione imposta il valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="62ca7-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="62ca7-310">Se si desidera impostare l'URL predefinito a un valore a livello di codice ma consentono di eseguire l'override con la configurazione, è possibile utilizzare la configurazione della riga di comando dopo l'impostazione dell'URL.</span><span class="sxs-lookup"><span data-stu-id="62ca7-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="62ca7-311">Vedere [configurazione verrà ignorato](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="62ca7-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="62ca7-312">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="62ca7-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62ca7-313">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62ca7-314">**Run**</span><span class="sxs-lookup"><span data-stu-id="62ca7-314">**Run**</span></span>

<span data-ttu-id="62ca7-315">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="62ca7-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="62ca7-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="62ca7-316">**Start**</span></span>

<span data-ttu-id="62ca7-317">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="62ca7-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="62ca7-318">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="62ca7-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="62ca7-319">È possibile inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="62ca7-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="62ca7-320">Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="62ca7-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="62ca7-321">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="62ca7-322">Iniziare con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="62ca7-323">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="62ca7-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="62ca7-324">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="62ca7-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62ca7-325">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62ca7-326">**Start (url di stringa, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="62ca7-327">Iniziare con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="62ca7-328">Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="62ca7-329">**Start (azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="62ca7-330">Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="62ca7-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="62ca7-331">Utilizzare le seguenti richieste del browser con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="62ca7-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="62ca7-332">Richiesta</span><span class="sxs-lookup"><span data-stu-id="62ca7-332">Request</span></span>                                    | <span data-ttu-id="62ca7-333">Risposta</span><span class="sxs-lookup"><span data-stu-id="62ca7-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="62ca7-334">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="62ca7-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="62ca7-335">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="62ca7-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="62ca7-336">Genera un'eccezione con stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="62ca7-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="62ca7-337">Genera un'eccezione con stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="62ca7-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="62ca7-338">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="62ca7-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="62ca7-339">Hello World!</span><span class="sxs-lookup"><span data-stu-id="62ca7-339">Hello World!</span></span>                             |

<span data-ttu-id="62ca7-340">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="62ca7-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62ca7-341">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62ca7-342">**Start (string url, azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="62ca7-343">Utilizzare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="62ca7-344">Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="62ca7-345">**StartWith (azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="62ca7-346">Fornire un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="62ca7-347">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="62ca7-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="62ca7-348">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="62ca7-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62ca7-349">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="62ca7-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62ca7-350">**StartWith (string url, azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="62ca7-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="62ca7-351">Fornire un URL e un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="62ca7-352">Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62ca7-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62ca7-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62ca7-354">**Run**</span><span class="sxs-lookup"><span data-stu-id="62ca7-354">**Run**</span></span>

<span data-ttu-id="62ca7-355">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="62ca7-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="62ca7-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="62ca7-356">**Start**</span></span>

<span data-ttu-id="62ca7-357">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="62ca7-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="62ca7-358">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="62ca7-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="62ca7-359">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="62ca7-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="62ca7-360">Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app.</span><span class="sxs-lookup"><span data-stu-id="62ca7-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="62ca7-361">È possibile utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="62ca7-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="62ca7-362">È possibile utilizzare un [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="62ca7-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="62ca7-363">In alternativa, è possibile inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="62ca7-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="62ca7-364">Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi.</span><span class="sxs-lookup"><span data-stu-id="62ca7-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="62ca7-365">Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="62ca7-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="62ca7-366">Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per impostare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="62ca7-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="62ca7-367">È possibile inserire `IHostingEnvironment` nel `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="62ca7-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="62ca7-368">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="62ca7-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="62ca7-369">Il [IApplicationLifetime interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente di eseguire attività di post-avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="62ca7-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="62ca7-370">Le tre proprietà nell'interfaccia sono token di annullamento che è possibile registrare con `Action` metodi per definire gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="62ca7-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="62ca7-371">È inoltre disponibile un `StopApplication` metodo.</span><span class="sxs-lookup"><span data-stu-id="62ca7-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="62ca7-372">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="62ca7-372">Cancellation Token</span></span>    | <span data-ttu-id="62ca7-373">Generato quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="62ca7-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="62ca7-374">L'host sia stato avviato.</span><span class="sxs-lookup"><span data-stu-id="62ca7-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="62ca7-375">L'host è in esecuzione un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="62ca7-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="62ca7-376">Stia ancora elaborando le richieste.</span><span class="sxs-lookup"><span data-stu-id="62ca7-376">Requests may still be processing.</span></span> <span data-ttu-id="62ca7-377">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="62ca7-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="62ca7-378">L'host di completamento in corso un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="62ca7-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="62ca7-379">Tutte le richieste devono essere completamente elaborate.</span><span class="sxs-lookup"><span data-stu-id="62ca7-379">All requests should be completely processed.</span></span> <span data-ttu-id="62ca7-380">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="62ca7-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="62ca7-381">Metodo</span><span class="sxs-lookup"><span data-stu-id="62ca7-381">Method</span></span>            | <span data-ttu-id="62ca7-382">Azione</span><span class="sxs-lookup"><span data-stu-id="62ca7-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="62ca7-383">Chiusura di richieste dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="62ca7-383">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="62ca7-384">Risoluzione dei problemi relativi a System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="62ca7-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="62ca7-385">**Si applica a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="62ca7-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="62ca7-386">Se si compila l'host inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`, potrebbe verificarsi l'errore seguente: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="62ca7-387">Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="62ca7-388">Se inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente al `WebHostBuilder` con il nome di assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="62ca7-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="62ca7-389">In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="62ca7-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="62ca7-390">**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando non si chiama `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="62ca7-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="62ca7-391">Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="62ca7-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62ca7-392">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="62ca7-392">Additional resources</span></span>

* [<span data-ttu-id="62ca7-393">Pubblicare in Windows tramite IIS</span><span class="sxs-lookup"><span data-stu-id="62ca7-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="62ca7-394">Pubblicare in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="62ca7-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="62ca7-395">Pubblicare in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="62ca7-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="62ca7-396">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="62ca7-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
