---
title: Hosting in ASP.NET Core
author: guardrex
description: "Informazioni sull'host web in ASP.NET Core, che è responsabile per la gestione di app di avvio e durata."
keywords: Web di ASP.NET Core, host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="adec9-104">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adec9-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="adec9-105">Da [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="adec9-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="adec9-106">Le applicazioni ASP.NET Core configurare e avviare un *host*, che è responsabile della gestione di avvio e la durata delle app.</span><span class="sxs-lookup"><span data-stu-id="adec9-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="adec9-107">Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="adec9-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="adec9-108">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="adec9-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-109">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="adec9-110">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="adec9-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="adec9-111">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="adec9-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="adec9-112">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="adec9-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="adec9-113">Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:</span><span class="sxs-lookup"><span data-stu-id="adec9-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="adec9-114">`CreateDefaultBuilder`esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="adec9-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="adec9-115">Configura [Kestrel](servers/kestrel.md) come server web.</span><span class="sxs-lookup"><span data-stu-id="adec9-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span>
* <span data-ttu-id="adec9-116">Imposta la radice del contenuto [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="adec9-116">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="adec9-117">Configurazione facoltativa di carichi da:</span><span class="sxs-lookup"><span data-stu-id="adec9-117">Loads optional configuration from:</span></span>
  * <span data-ttu-id="adec9-118">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="adec9-118">*appsettings.json*.</span></span>
  * <span data-ttu-id="adec9-119">*appSettings. {Ambiente} JSON*.</span><span class="sxs-lookup"><span data-stu-id="adec9-119">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="adec9-120">[Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="adec9-120">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="adec9-121">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="adec9-121">Environment variables.</span></span>
  * <span data-ttu-id="adec9-122">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="adec9-122">Command-line arguments.</span></span>
* <span data-ttu-id="adec9-123">Configura [registrazione](xref:fundamentals/logging) per l'output di console ed eseguire il debug con [filtri log](xref:fundamentals/logging#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. {Ambiente} JSON* file.</span><span class="sxs-lookup"><span data-stu-id="adec9-123">Configures [logging](xref:fundamentals/logging) for console and debug output with [log filtering](xref:fundamentals/logging#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="adec9-124">Quando si esegue dietro IIS, consente l'integrazione di IIS configurando il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="adec9-124">When running behind IIS, enables IIS integration by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="adec9-125">Il modulo Crea un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="adec9-125">The module creates a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="adec9-126">Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="adec9-126">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span>

<span data-ttu-id="adec9-127">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="adec9-127">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="adec9-128">La radice di contenuto predefinito è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="adec9-128">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="adec9-129">Ciò comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="adec9-129">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="adec9-130">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="adec9-130">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="adec9-131">Vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration) per ulteriori informazioni sulla configurazione delle app.</span><span class="sxs-lookup"><span data-stu-id="adec9-131">See [Configuration in ASP.NET Core](xref:fundamentals/configuration) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="adec9-132">Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="adec9-132">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="adec9-133">Vedere la scheda di 1. x di ASP.NET Core per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="adec9-133">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-134">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-135">Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="adec9-135">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="adec9-136">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo.</span><span class="sxs-lookup"><span data-stu-id="adec9-136">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="adec9-137">Nei modelli di progetto, `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="adec9-137">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="adec9-138">Nell'esempio *Program.cs* viene illustrato come utilizzare `WebHostBuilder` per compilare l'host:</span><span class="sxs-lookup"><span data-stu-id="adec9-138">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="adec9-139">`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="adec9-139">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="adec9-140">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="adec9-140">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="adec9-141">In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="adec9-141">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="adec9-142">Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="adec9-142">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="adec9-143">La radice di contenuto predefinito fornito a `UseContentRoot` è [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="adec9-143">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="adec9-144">Ciò comporta l'utilizzo di cartella radice del progetto web come radice del contenuto quando l'applicazione viene avviata dalla cartella radice (ad esempio, la chiamata [dotnet eseguire](/dotnet/core/tools/dotnet-run) dalla cartella del progetto).</span><span class="sxs-lookup"><span data-stu-id="adec9-144">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="adec9-145">Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="adec9-145">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="adec9-146">Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="adec9-146">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="adec9-147">`UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="adec9-147">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="adec9-148">`UseIISIntegration`Consente di configurare il percorso di base e la porta server deve rimanere in attesa quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="adec9-148">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="adec9-149">Per utilizzare IIS con ASP.NET Core, è necessario specificare sia `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="adec9-149">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="adec9-150">`UseIISIntegration`attiva solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="adec9-150">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="adec9-151">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="adec9-151">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="adec9-152">Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="adec9-152">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="adec9-153">Quando si configura un host, è possibile fornire [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi.</span><span class="sxs-lookup"><span data-stu-id="adec9-153">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="adec9-154">Se si specifica un `Startup` (classe), è necessario definire un `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="adec9-154">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="adec9-155">Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="adec9-155">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="adec9-156">Più chiamate a `ConfigureServices` aggiungere uno a altro.</span><span class="sxs-lookup"><span data-stu-id="adec9-156">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="adec9-157">Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="adec9-157">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="adec9-158">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="adec9-158">Host configuration values</span></span>

<span data-ttu-id="adec9-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) fornisce metodi per l'impostazione che può anche essere impostata direttamente con la maggior parte dei valori di configurazione disponibili per l'host, [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="adec9-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="adec9-160">Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa (tra virgolette) indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="adec9-160">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="adec9-161">Acquisire gli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="adec9-161">Capture Startup Errors</span></span>

<span data-ttu-id="adec9-162">Questa impostazione Controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="adec9-162">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="adec9-163">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="adec9-163">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="adec9-164">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="adec9-164">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="adec9-165">**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="adec9-165">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="adec9-166">**Impostare utilizzando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="adec9-166">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="adec9-167">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="adec9-167">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="adec9-168">Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="adec9-168">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-169">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-170">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="adec9-171">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="adec9-171">Content Root</span></span>

<span data-ttu-id="adec9-172">Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="adec9-172">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="adec9-173">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="adec9-173">**Key**: contentRoot</span></span>  
<span data-ttu-id="adec9-174">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-174">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-175">**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.</span><span class="sxs-lookup"><span data-stu-id="adec9-175">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="adec9-176">**Impostare utilizzando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="adec9-176">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="adec9-177">La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="adec9-177">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="adec9-178">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="adec9-178">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-179">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-180">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="adec9-181">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="adec9-181">Detailed Errors</span></span>

<span data-ttu-id="adec9-182">Determina se gli errori dettagliati devono essere acquisiti gli errori.</span><span class="sxs-lookup"><span data-stu-id="adec9-182">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="adec9-183">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="adec9-183">**Key**: detailedErrors</span></span>  
<span data-ttu-id="adec9-184">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="adec9-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="adec9-185">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="adec9-185">**Default**: false</span></span>  
<span data-ttu-id="adec9-186">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="adec9-186">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="adec9-187">Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="adec9-187">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-188">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-189">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-189">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="adec9-190">Ambiente</span><span class="sxs-lookup"><span data-stu-id="adec9-190">Environment</span></span>

<span data-ttu-id="adec9-191">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="adec9-191">Sets the app's environment.</span></span>

<span data-ttu-id="adec9-192">**Chiave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="adec9-192">**Key**: environment</span></span>  
<span data-ttu-id="adec9-193">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-193">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-194">**Predefinito**: produzione</span><span class="sxs-lookup"><span data-stu-id="adec9-194">**Default**: Production</span></span>  
<span data-ttu-id="adec9-195">**Impostare utilizzando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="adec9-195">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="adec9-196">È possibile impostare il *ambiente* su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="adec9-196">You can set the *Environment* to any value.</span></span> <span data-ttu-id="adec9-197">I valori definiti dal framework includono `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="adec9-197">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="adec9-198">I valori non sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="adec9-198">Values aren't case sensitive.</span></span> <span data-ttu-id="adec9-199">Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="adec9-199">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="adec9-200">Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="adec9-200">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="adec9-201">Per ulteriori informazioni, vedere [utilizzo di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="adec9-201">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-202">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-203">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="adec9-204">Hosting di assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="adec9-204">Hosting Startup Assemblies</span></span>

<span data-ttu-id="adec9-205">Imposta gli assembly di avvio hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-205">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="adec9-206">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="adec9-206">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="adec9-207">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-207">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-208">**Predefinito**: una stringa vuota</span><span class="sxs-lookup"><span data-stu-id="adec9-208">**Default**: Empty string</span></span>  
<span data-ttu-id="adec9-209">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="adec9-209">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="adec9-210">Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting.</span><span class="sxs-lookup"><span data-stu-id="adec9-210">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="adec9-211">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="adec9-211">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="adec9-212">Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-212">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="adec9-213">Quando si specificano gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="adec9-213">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-214">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-215">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-216">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="adec9-216">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="adec9-217">Preferiscono gli URL di Hosting</span><span class="sxs-lookup"><span data-stu-id="adec9-217">Prefer Hosting URLs</span></span>

<span data-ttu-id="adec9-218">Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-218">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="adec9-219">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="adec9-219">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="adec9-220">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="adec9-220">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="adec9-221">**Predefinito**: true</span><span class="sxs-lookup"><span data-stu-id="adec9-221">**Default**: true</span></span>  
<span data-ttu-id="adec9-222">**Impostare utilizzando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="adec9-222">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="adec9-223">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="adec9-223">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-224">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-224">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-225">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-226">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="adec9-226">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="adec9-227">Impedire l'avvio di Hosting</span><span class="sxs-lookup"><span data-stu-id="adec9-227">Prevent Hosting Startup</span></span>

<span data-ttu-id="adec9-228">Impedisce il caricamento automatico di assembly di avvio, tra cui assembly dell'applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="adec9-228">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="adec9-229">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="adec9-229">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="adec9-230">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="adec9-230">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="adec9-231">**Predefinito**: false</span><span class="sxs-lookup"><span data-stu-id="adec9-231">**Default**: false</span></span>  
<span data-ttu-id="adec9-232">**Impostare utilizzando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="adec9-232">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="adec9-233">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="adec9-233">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-234">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-234">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-235">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-235">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-236">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="adec9-236">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="adec9-237">URL del server</span><span class="sxs-lookup"><span data-stu-id="adec9-237">Server URLs</span></span>

<span data-ttu-id="adec9-238">Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.</span><span class="sxs-lookup"><span data-stu-id="adec9-238">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="adec9-239">**Chiave**: URL</span><span class="sxs-lookup"><span data-stu-id="adec9-239">**Key**: urls</span></span>  
<span data-ttu-id="adec9-240">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-240">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-241">**Predefinito**: indirizzo http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="adec9-241">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="adec9-242">**Impostare utilizzando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="adec9-242">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="adec9-243">Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="adec9-243">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="adec9-244">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="adec9-244">For example, `http://localhost:123`.</span></span> <span data-ttu-id="adec9-245">Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="adec9-245">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="adec9-246">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="adec9-246">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="adec9-247">I formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="adec9-247">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-248">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="adec9-249">Kestrel ha la propria configurazione di endpoint API.</span><span class="sxs-lookup"><span data-stu-id="adec9-249">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="adec9-250">Per ulteriori informazioni, vedere [implementazione Kestrel del server web in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="adec9-250">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-251">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="adec9-252">Timeout di chiusura</span><span class="sxs-lookup"><span data-stu-id="adec9-252">Shutdown Timeout</span></span>

<span data-ttu-id="adec9-253">Specifica la quantità di tempo di attesa per l'host web per l'arresto.</span><span class="sxs-lookup"><span data-stu-id="adec9-253">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="adec9-254">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="adec9-254">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="adec9-255">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="adec9-255">**Type**: *int*</span></span>  
<span data-ttu-id="adec9-256">**Predefinito**: 5</span><span class="sxs-lookup"><span data-stu-id="adec9-256">**Default**: 5</span></span>  
<span data-ttu-id="adec9-257">**Impostare utilizzando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="adec9-257">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="adec9-258">Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="adec9-258">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="adec9-259">Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="adec9-259">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-260">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-260">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-261">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-261">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-262">Questa funzionalità è disponibile in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="adec9-262">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="adec9-263">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="adec9-263">Startup Assembly</span></span>

<span data-ttu-id="adec9-264">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="adec9-264">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="adec9-265">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="adec9-265">**Key**: startupAssembly</span></span>  
<span data-ttu-id="adec9-266">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-266">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-267">**Predefinito**: assembly dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="adec9-267">**Default**: The app's assembly</span></span>  
<span data-ttu-id="adec9-268">**Impostare utilizzando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="adec9-268">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="adec9-269">È possibile fare riferimento all'assembly in base al nome (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="adec9-269">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="adec9-270">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="adec9-270">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-271">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-271">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-272">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="adec9-273">Radice Web</span><span class="sxs-lookup"><span data-stu-id="adec9-273">Web Root</span></span>

<span data-ttu-id="adec9-274">Imposta il percorso relativo alle risorse statiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="adec9-274">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="adec9-275">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="adec9-275">**Key**: webroot</span></span>  
<span data-ttu-id="adec9-276">**Tipo**: *stringa*</span><span class="sxs-lookup"><span data-stu-id="adec9-276">**Type**: *string*</span></span>  
<span data-ttu-id="adec9-277">**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista.</span><span class="sxs-lookup"><span data-stu-id="adec9-277">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="adec9-278">Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-278">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="adec9-279">**Impostare utilizzando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="adec9-279">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-280">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-280">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-281">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-281">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="adec9-282">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="adec9-282">Overriding configuration</span></span>

<span data-ttu-id="adec9-283">Utilizzare [configurazione](configuration.md) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="adec9-283">Use [Configuration](configuration.md) to configure the host.</span></span> <span data-ttu-id="adec9-284">Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="adec9-284">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="adec9-285">Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="adec9-285">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="adec9-286">La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="adec9-286">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-287">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-287">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="adec9-288">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="adec9-288">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="adec9-289">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="adec9-289">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-290">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-290">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="adec9-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="adec9-292">Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:</span><span class="sxs-lookup"><span data-stu-id="adec9-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="adec9-293">Il `UseConfiguration` non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="adec9-293">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="adec9-294">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="adec9-294">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="adec9-295">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="adec9-295">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="adec9-296">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="adec9-296">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="adec9-297">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="adec9-297">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="adec9-298">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="adec9-298">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="adec9-299">Per specificare l'host di eseguire in un URL specifico, è possibile passare il valore desiderato da un prompt dei comandi quando si esegue `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="adec9-299">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="adec9-300">L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="adec9-300">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="adec9-301">Ordine di priorità</span><span class="sxs-lookup"><span data-stu-id="adec9-301">Ordering importance</span></span>

<span data-ttu-id="adec9-302">Alcune delle `WebHostBuilder` impostazioni vengono lette prima dalle variabili di ambiente, se impostata.</span><span class="sxs-lookup"><span data-stu-id="adec9-302">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="adec9-303">Queste variabili di ambiente utilizzano il formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="adec9-303">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="adec9-304">Per impostare gli URL che il server utilizza per impostazione predefinita, si imposta `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="adec9-304">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="adec9-305">È possibile sostituire qualsiasi di questi valori di variabili di ambiente specificando una configurazione (utilizzando `UseConfiguration`) o impostando in modo esplicito il valore (con `UseSetting` o uno dei metodi di estensione esplicita, ad esempio `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="adec9-305">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="adec9-306">L'host usa indipendentemente dall'opzione imposta il valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="adec9-306">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="adec9-307">Se si desidera impostare l'URL predefinito a un valore a livello di codice ma consentono di eseguire l'override con la configurazione, è possibile utilizzare la configurazione della riga di comando dopo l'impostazione dell'URL.</span><span class="sxs-lookup"><span data-stu-id="adec9-307">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="adec9-308">Vedere [configurazione verrà ignorato](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="adec9-308">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="adec9-309">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="adec9-309">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adec9-310">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="adec9-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="adec9-311">**Run**</span><span class="sxs-lookup"><span data-stu-id="adec9-311">**Run**</span></span>

<span data-ttu-id="adec9-312">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="adec9-312">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="adec9-313">**Start**</span><span class="sxs-lookup"><span data-stu-id="adec9-313">**Start**</span></span>

<span data-ttu-id="adec9-314">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="adec9-314">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="adec9-315">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="adec9-315">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="adec9-316">È possibile inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="adec9-316">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="adec9-317">Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="adec9-317">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="adec9-318">**Start (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="adec9-318">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="adec9-319">Iniziare con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="adec9-319">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="adec9-320">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="adec9-320">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="adec9-321">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="adec9-321">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="adec9-322">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-322">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="adec9-323">**Start (url di stringa, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="adec9-323">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="adec9-324">Iniziare con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="adec9-324">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="adec9-325">Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="adec9-325">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="adec9-326">**Start (azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="adec9-326">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="adec9-327">Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="adec9-327">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="adec9-328">Utilizzare le seguenti richieste del browser con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="adec9-328">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="adec9-329">Richiesta</span><span class="sxs-lookup"><span data-stu-id="adec9-329">Request</span></span>                                    | <span data-ttu-id="adec9-330">Risposta</span><span class="sxs-lookup"><span data-stu-id="adec9-330">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="adec9-331">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="adec9-331">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="adec9-332">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="adec9-332">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="adec9-333">Genera un'eccezione con stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="adec9-333">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="adec9-334">Genera un'eccezione con stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="adec9-334">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="adec9-335">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="adec9-335">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="adec9-336">Hello World!</span><span class="sxs-lookup"><span data-stu-id="adec9-336">Hello World!</span></span>                             |

<span data-ttu-id="adec9-337">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="adec9-337">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="adec9-338">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-338">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="adec9-339">**Start (string url, azione<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="adec9-339">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="adec9-340">Utilizzare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="adec9-340">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="adec9-341">Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="adec9-341">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="adec9-342">**StartWith (azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="adec9-342">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="adec9-343">Fornire un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="adec9-343">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="adec9-344">Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="adec9-344">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="adec9-345">`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="adec9-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="adec9-346">L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.</span><span class="sxs-lookup"><span data-stu-id="adec9-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="adec9-347">**StartWith (string url, azione<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="adec9-347">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="adec9-348">Fornire un URL e un delegato per configurare un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="adec9-348">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="adec9-349">Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="adec9-349">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adec9-350">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="adec9-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="adec9-351">**Run**</span><span class="sxs-lookup"><span data-stu-id="adec9-351">**Run**</span></span>

<span data-ttu-id="adec9-352">Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:</span><span class="sxs-lookup"><span data-stu-id="adec9-352">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="adec9-353">**Start**</span><span class="sxs-lookup"><span data-stu-id="adec9-353">**Start**</span></span>

<span data-ttu-id="adec9-354">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="adec9-354">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="adec9-355">Se si passa un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="adec9-355">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="adec9-356">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="adec9-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="adec9-357">Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app.</span><span class="sxs-lookup"><span data-stu-id="adec9-357">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="adec9-358">È possibile utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="adec9-358">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="adec9-359">È possibile utilizzare un [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="adec9-359">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="adec9-360">In alternativa, è possibile inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="adec9-360">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="adec9-361">Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi.</span><span class="sxs-lookup"><span data-stu-id="adec9-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="adec9-362">Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="adec9-362">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="adec9-363">Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per impostare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="adec9-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="adec9-364">È possibile inserire `IHostingEnvironment` nel `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="adec9-364">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="adec9-365">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="adec9-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="adec9-366">Il [IApplicationLifetime interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente di eseguire attività di post-avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="adec9-366">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="adec9-367">Le tre proprietà nell'interfaccia sono token di annullamento che è possibile registrare con `Action` metodi per definire gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="adec9-367">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="adec9-368">È inoltre disponibile un `StopApplication` metodo.</span><span class="sxs-lookup"><span data-stu-id="adec9-368">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="adec9-369">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="adec9-369">Cancellation Token</span></span>    | <span data-ttu-id="adec9-370">Generato quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="adec9-370">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="adec9-371">L'host sia stato avviato.</span><span class="sxs-lookup"><span data-stu-id="adec9-371">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="adec9-372">L'host è in esecuzione un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="adec9-372">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="adec9-373">Stia ancora elaborando le richieste.</span><span class="sxs-lookup"><span data-stu-id="adec9-373">Requests may still be processing.</span></span> <span data-ttu-id="adec9-374">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="adec9-374">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="adec9-375">L'host di completamento in corso un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="adec9-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="adec9-376">Tutte le richieste devono essere completamente elaborate.</span><span class="sxs-lookup"><span data-stu-id="adec9-376">All requests should be completely processed.</span></span> <span data-ttu-id="adec9-377">Blocchi di arresto fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="adec9-377">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="adec9-378">Metodo</span><span class="sxs-lookup"><span data-stu-id="adec9-378">Method</span></span>            | <span data-ttu-id="adec9-379">Azione</span><span class="sxs-lookup"><span data-stu-id="adec9-379">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="adec9-380">Chiusura di richieste dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="adec9-380">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="adec9-381">Risoluzione dei problemi relativi a System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="adec9-381">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="adec9-382">**Si applica a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="adec9-382">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="adec9-383">Se si compila l'host inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`, potrebbe verificarsi l'errore seguente: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="adec9-383">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="adec9-384">Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="adec9-384">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="adec9-385">Se inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente al `WebHostBuilder` con il nome di assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="adec9-385">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="adec9-386">In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="adec9-386">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="adec9-387">**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando non si chiama `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="adec9-387">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="adec9-388">Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="adec9-388">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adec9-389">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="adec9-389">Additional resources</span></span>

* [<span data-ttu-id="adec9-390">Pubblicare in Windows tramite IIS</span><span class="sxs-lookup"><span data-stu-id="adec9-390">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="adec9-391">Pubblicare in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="adec9-391">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="adec9-392">Pubblicare in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="adec9-392">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="adec9-393">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="adec9-393">Host in a Windows Service</span></span>](xref:hosting/windows-service)
