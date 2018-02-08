---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="e8d6f-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d6f-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="e8d6f-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8d6f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e8d6f-105">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="e8d6f-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e8d6f-107">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="e8d6f-108">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="e8d6f-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e8d6f-110">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="e8d6f-111">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="e8d6f-112">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="e8d6f-113">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="e8d6f-114">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="e8d6f-115">Configura [Kestrel](servers/kestrel.md) come server Web.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="e8d6f-116">Per le opzioni predefinite Kestrel, vedere la [sezione Opzioni Kestrel in Introduzione all'implementazione di server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="e8d6f-117">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="e8d6f-118">Carica le configurazioni facoltative da:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="e8d6f-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="e8d6f-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="e8d6f-121">[Segreti dell'utente](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="e8d6f-122">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-122">Environment variables.</span></span>
  * <span data-ttu-id="e8d6f-123">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-123">Command-line arguments.</span></span>
* <span data-ttu-id="e8d6f-124">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="e8d6f-125">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="e8d6f-126">Se eseguito in IIS, abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="e8d6f-127">Configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e8d6f-128">Il modulo crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="e8d6f-129">Configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="e8d6f-130">Per le opzioni predefinite IIS, vedere la [sezione Opzioni IIS in Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="e8d6f-131">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="e8d6f-132">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="e8d6f-133">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="e8d6f-134">Per altre informazioni sulla configurazione delle app, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6f-135">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="e8d6f-136">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-138">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="e8d6f-139">La creazione di un host viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="e8d6f-140">Nei modelli di progetto `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="e8d6f-141">`WebHostBuilder` richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="e8d6f-142">I server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima della versione ASP.NET Core 2.0, HTTP.sys era chiamato [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="e8d6f-143">In questo esempio, il [metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="e8d6f-144">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="e8d6f-145">La radice di contenuto predefinita viene ottenuta per `UseContentRoot` tramite [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="e8d6f-146">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="e8d6f-147">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="e8d6f-148">Per usare IIS come proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="e8d6f-149">A differenza di [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` non configura un *server*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="e8d6f-150">`UseIISIntegration` configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="e8d6f-151">Per usare IIS con ASP.NET Core, è necessario specificare `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="e8d6f-152">`UseIISIntegration` viene attivato solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="e8d6f-153">Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="e8d6f-154">Un'implementazione minima che configura un host (e un'app ASP.NET Core) include la specifica di un server e la configurazione della pipeline delle richieste dell'app:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="e8d6f-155">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="e8d6f-156">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="e8d6f-157">Per altre informazioni, vedere [Avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="e8d6f-158">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="e8d6f-159">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="e8d6f-160">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="e8d6f-160">Host configuration values</span></span>

<span data-ttu-id="e8d6f-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="e8d6f-162">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="e8d6f-163">Ad esempio `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="e8d6f-164">Metodi espliciti, ad esempio `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="e8d6f-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="e8d6f-166">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="e8d6f-167">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="e8d6f-168">Per altre informazioni, vedere [Override della configurazione](#overriding-configuration) nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="e8d6f-169">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="e8d6f-169">Capture Startup Errors</span></span>

<span data-ttu-id="e8d6f-170">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="e8d6f-171">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="e8d6f-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="e8d6f-172">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e8d6f-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e8d6f-173">**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="e8d6f-174">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="e8d6f-175">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="e8d6f-176">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e8d6f-177">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="e8d6f-180">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="e8d6f-180">Content Root</span></span>

<span data-ttu-id="e8d6f-181">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="e8d6f-182">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="e8d6f-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="e8d6f-183">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-183">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-184">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="e8d6f-185">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="e8d6f-186">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="e8d6f-187">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="e8d6f-188">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="e8d6f-191">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="e8d6f-191">Detailed Errors</span></span>

<span data-ttu-id="e8d6f-192">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="e8d6f-193">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="e8d6f-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="e8d6f-194">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e8d6f-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e8d6f-195">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="e8d6f-195">**Default**: false</span></span>  
<span data-ttu-id="e8d6f-196">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e8d6f-197">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="e8d6f-198">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="e8d6f-201">Ambiente</span><span class="sxs-lookup"><span data-stu-id="e8d6f-201">Environment</span></span>

<span data-ttu-id="e8d6f-202">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-202">Sets the app's environment.</span></span>

<span data-ttu-id="e8d6f-203">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="e8d6f-203">**Key**: environment</span></span>  
<span data-ttu-id="e8d6f-204">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-204">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-205">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="e8d6f-205">**Default**: Production</span></span>  
<span data-ttu-id="e8d6f-206">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="e8d6f-207">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="e8d6f-208">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-208">The environment can be set to any value.</span></span> <span data-ttu-id="e8d6f-209">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e8d6f-210">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-210">Values aren't case sensitive.</span></span> <span data-ttu-id="e8d6f-211">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="e8d6f-212">Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="e8d6f-213">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="e8d6f-216">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="e8d6f-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="e8d6f-217">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="e8d6f-218">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="e8d6f-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="e8d6f-219">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-219">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-220">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="e8d6f-220">**Default**: Empty string</span></span>  
<span data-ttu-id="e8d6f-221">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e8d6f-222">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="e8d6f-223">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="e8d6f-224">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="e8d6f-225">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="e8d6f-226">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-229">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="e8d6f-230">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="e8d6f-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="e8d6f-231">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="e8d6f-232">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="e8d6f-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="e8d6f-233">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e8d6f-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e8d6f-234">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="e8d6f-234">**Default**: true</span></span>  
<span data-ttu-id="e8d6f-235">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="e8d6f-236">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="e8d6f-237">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-240">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="e8d6f-241">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="e8d6f-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="e8d6f-242">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="e8d6f-243">Per altre informazioni, vedere [Aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:host-and-deploy/ihostingstartup).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="e8d6f-244">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e8d6f-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="e8d6f-245">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e8d6f-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e8d6f-246">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="e8d6f-246">**Default**: false</span></span>  
<span data-ttu-id="e8d6f-247">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e8d6f-248">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="e8d6f-249">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-252">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="e8d6f-253">URL del server</span><span class="sxs-lookup"><span data-stu-id="e8d6f-253">Server URLs</span></span>

<span data-ttu-id="e8d6f-254">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="e8d6f-255">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="e8d6f-255">**Key**: urls</span></span>  
<span data-ttu-id="e8d6f-256">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-256">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-257">**Impostazione predefinita**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="e8d6f-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="e8d6f-258">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="e8d6f-259">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="e8d6f-260">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="e8d6f-261">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e8d6f-262">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="e8d6f-263">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e8d6f-264">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="e8d6f-266">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="e8d6f-267">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="e8d6f-269">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="e8d6f-269">Shutdown Timeout</span></span>

<span data-ttu-id="e8d6f-270">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="e8d6f-271">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="e8d6f-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="e8d6f-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-272">**Type**: *int*</span></span>  
<span data-ttu-id="e8d6f-273">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="e8d6f-273">**Default**: 5</span></span>  
<span data-ttu-id="e8d6f-274">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="e8d6f-275">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="e8d6f-276">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione `UseShutdownTimeout` accetta `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="e8d6f-277">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-280">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="e8d6f-281">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="e8d6f-281">Startup Assembly</span></span>

<span data-ttu-id="e8d6f-282">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="e8d6f-283">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="e8d6f-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="e8d6f-284">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-284">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-285">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="e8d6f-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="e8d6f-286">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="e8d6f-287">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="e8d6f-288">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="e8d6f-289">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="e8d6f-292">Radice Web</span><span class="sxs-lookup"><span data-stu-id="e8d6f-292">Web Root</span></span>

<span data-ttu-id="e8d6f-293">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="e8d6f-294">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="e8d6f-294">**Key**: webroot</span></span>  
<span data-ttu-id="e8d6f-295">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e8d6f-295">**Type**: *string*</span></span>  
<span data-ttu-id="e8d6f-296">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="e8d6f-297">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="e8d6f-298">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="e8d6f-299">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="e8d6f-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="e8d6f-302">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="e8d6f-302">Overriding configuration</span></span>

<span data-ttu-id="e8d6f-303">Usare [Configuration](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="e8d6f-304">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente in un file *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="e8d6f-305">Qualsiasi configurazione caricata dal file *hosting.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="e8d6f-306">La configurazione compilata (in `config`) viene usata per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e8d6f-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="e8d6f-309">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="e8d6f-312">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="e8d6f-313">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="e8d6f-314">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="e8d6f-315">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="e8d6f-316">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="e8d6f-317">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="e8d6f-318">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="e8d6f-319">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="e8d6f-320">L'argomento della riga di comando esegue l'override del valore `urls`dal file *hosting.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="e8d6f-321">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="e8d6f-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8d6f-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e8d6f-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-323">**Run**</span></span>

<span data-ttu-id="e8d6f-324">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e8d6f-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-325">**Start**</span></span>

<span data-ttu-id="e8d6f-326">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e8d6f-327">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="e8d6f-328">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="e8d6f-329">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="e8d6f-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="e8d6f-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="e8d6f-331">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e8d6f-332">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="e8d6f-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e8d6f-333">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e8d6f-334">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e8d6f-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="e8d6f-336">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e8d6f-337">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="e8d6f-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="e8d6f-339">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="e8d6f-340">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="e8d6f-341">Richiesta</span><span class="sxs-lookup"><span data-stu-id="e8d6f-341">Request</span></span>                                    | <span data-ttu-id="e8d6f-342">Risposta</span><span class="sxs-lookup"><span data-stu-id="e8d6f-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="e8d6f-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="e8d6f-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="e8d6f-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="e8d6f-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="e8d6f-345">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="e8d6f-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="e8d6f-346">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="e8d6f-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="e8d6f-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="e8d6f-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="e8d6f-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="e8d6f-348">Hello World!</span></span>                             |

<span data-ttu-id="e8d6f-349">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e8d6f-350">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e8d6f-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="e8d6f-352">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="e8d6f-353">Produce lo stesso risultato di **Start(Action<IRouteBuilder> routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="e8d6f-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="e8d6f-355">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e8d6f-356">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="e8d6f-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e8d6f-357">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e8d6f-358">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e8d6f-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="e8d6f-360">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e8d6f-361">Produce lo stesso risultato di **StartWith(Action<IApplicationBuilder> app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8d6f-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8d6f-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8d6f-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-363">**Run**</span></span>

<span data-ttu-id="e8d6f-364">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e8d6f-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-365">**Start**</span></span>

<span data-ttu-id="e8d6f-366">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e8d6f-367">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="e8d6f-368">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="e8d6f-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="e8d6f-369">L'[interfaccia IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="e8d6f-370">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="e8d6f-371">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="e8d6f-372">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="e8d6f-373">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="e8d6f-374">Per informazioni dettagliate, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="e8d6f-375">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="e8d6f-376">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="e8d6f-377">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e8d6f-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="e8d6f-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="e8d6f-379">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="e8d6f-380">È presente anche un metodo `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="e8d6f-381">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="e8d6f-381">Cancellation Token</span></span>    | <span data-ttu-id="e8d6f-382">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="e8d6f-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="e8d6f-383">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="e8d6f-384">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="e8d6f-385">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-385">Requests may still be processing.</span></span> <span data-ttu-id="e8d6f-386">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="e8d6f-387">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="e8d6f-388">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-388">All requests should be processed.</span></span> <span data-ttu-id="e8d6f-389">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="e8d6f-390">Metodo</span><span class="sxs-lookup"><span data-stu-id="e8d6f-390">Method</span></span>            | <span data-ttu-id="e8d6f-391">Operazione</span><span class="sxs-lookup"><span data-stu-id="e8d6f-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="e8d6f-392">Richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="e8d6f-393">Troubleshooting System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="e8d6f-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="e8d6f-394">**Si applica solo ad ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="e8d6f-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="e8d6f-395">È possibile creare un host inserendo `IStartup` direttamente nel contenitore di inserimento delle dipendenze anziché chiamando `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="e8d6f-396">Se l'host viene creato in questo modo, è possibile che si verifichi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="e8d6f-397">Ciò si verifica perché è necessario [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (assembly corrente) per cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="e8d6f-398">Se l'app inserisce manualmente `IStartup` nel contenitore di inserimento delle dipendenze, aggiungere la chiamata seguente a `WebHostBuilder` con il nome dell'assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="e8d6f-399">In alternativa, aggiungere `Configure` fittizio a `WebHostBuilder`, che imposta `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="e8d6f-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="e8d6f-400">**NOTA**: quest'operazione è necessaria solo con la versione ASP.NET Core 2.0 e solo quando l'app non esegue la chiamata a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e8d6f-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="e8d6f-401">Per altre informazioni, vedere [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)) e [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs) (Esempio di StartupInjection).</span><span class="sxs-lookup"><span data-stu-id="e8d6f-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8d6f-402">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e8d6f-402">Additional resources</span></span>

* [<span data-ttu-id="e8d6f-403">Host in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="e8d6f-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e8d6f-404">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="e8d6f-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="e8d6f-405">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="e8d6f-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="e8d6f-406">Host in un servizio di Windows</span><span class="sxs-lookup"><span data-stu-id="e8d6f-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
