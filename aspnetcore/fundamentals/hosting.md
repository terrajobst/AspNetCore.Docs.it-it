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
ms.openlocfilehash: 3dab68da1b2b24351d40266429bf25f26ba467bf
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094801"
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="c0dd6-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0dd6-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="c0dd6-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0dd6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c0dd6-105">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="c0dd6-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c0dd6-107">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="c0dd6-108">Impostazione di un host</span><span class="sxs-lookup"><span data-stu-id="c0dd6-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="c0dd6-110">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="c0dd6-111">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="c0dd6-112">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="c0dd6-113">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="c0dd6-114">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="c0dd6-115">Configura [Kestrel](servers/kestrel.md) come server Web.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="c0dd6-116">Per le opzioni predefinite Kestrel, vedere la [sezione Opzioni Kestrel in Introduzione all'implementazione di server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="c0dd6-117">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="c0dd6-118">Carica le configurazioni facoltative da:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="c0dd6-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="c0dd6-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="c0dd6-121">[Segreti dell'utente](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="c0dd6-122">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-122">Environment variables.</span></span>
  * <span data-ttu-id="c0dd6-123">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-123">Command-line arguments.</span></span>
* <span data-ttu-id="c0dd6-124">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="c0dd6-125">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="c0dd6-126">Se eseguito in IIS, abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="c0dd6-127">Configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="c0dd6-128">Il modulo crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="c0dd6-129">Configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="c0dd6-130">Per le opzioni predefinite IIS, vedere la [sezione Opzioni IIS in Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="c0dd6-131">Imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="c0dd6-132">Per ulteriori informazioni, vedere [Convalida dell'ambito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="c0dd6-133">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c0dd6-134">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c0dd6-135">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c0dd6-136">Per altre informazioni sulla configurazione delle app, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="c0dd6-137">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="c0dd6-138">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="c0dd6-140">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="c0dd6-141">La creazione di un host viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="c0dd6-142">Nei modelli di progetto `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="c0dd6-143">`WebHostBuilder` richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="c0dd6-144">I server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima della versione ASP.NET Core 2.0, HTTP.sys era chiamato [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="c0dd6-145">In questo esempio, il [metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="c0dd6-146">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c0dd6-147">La radice di contenuto predefinita viene ottenuta per `UseContentRoot` tramite [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="c0dd6-148">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c0dd6-149">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c0dd6-150">Per usare IIS come proxy inverso, chiamare [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="c0dd6-151">A differenza di [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` non configura un *server*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="c0dd6-152">`UseIISIntegration` configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="c0dd6-153">Per usare IIS con ASP.NET Core, è necessario specificare `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="c0dd6-154">`UseIISIntegration` viene attivato solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="c0dd6-155">Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="c0dd6-156">Un'implementazione minima che configura un host (e un'app ASP.NET Core) include la specifica di un server e la configurazione della pipeline delle richieste dell'app:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="c0dd6-157">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="c0dd6-158">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="c0dd6-159">Per altre informazioni, vedere [Avvio dell'applicazione in ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="c0dd6-160">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="c0dd6-161">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c0dd6-162">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="c0dd6-162">Host configuration values</span></span>

<span data-ttu-id="c0dd6-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="c0dd6-164">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="c0dd6-165">Ad esempio `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="c0dd6-166">Metodi espliciti, ad esempio `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="c0dd6-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="c0dd6-168">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="c0dd6-169">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="c0dd6-170">Per altre informazioni, vedere [Override della configurazione](#overriding-configuration) nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="c0dd6-171">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="c0dd6-171">Capture Startup Errors</span></span>

<span data-ttu-id="c0dd6-172">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="c0dd6-173">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="c0dd6-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="c0dd6-174">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c0dd6-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0dd6-175">**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="c0dd6-176">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="c0dd6-177">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="c0dd6-178">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="c0dd6-179">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="c0dd6-182">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="c0dd6-182">Content Root</span></span>

<span data-ttu-id="c0dd6-183">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="c0dd6-184">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="c0dd6-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="c0dd6-185">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-185">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-186">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="c0dd6-187">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="c0dd6-188">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="c0dd6-189">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="c0dd6-190">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="c0dd6-193">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="c0dd6-193">Detailed Errors</span></span>

<span data-ttu-id="c0dd6-194">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="c0dd6-195">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="c0dd6-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="c0dd6-196">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c0dd6-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0dd6-197">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="c0dd6-197">**Default**: false</span></span>  
<span data-ttu-id="c0dd6-198">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0dd6-199">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="c0dd6-200">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="c0dd6-203">Ambiente</span><span class="sxs-lookup"><span data-stu-id="c0dd6-203">Environment</span></span>

<span data-ttu-id="c0dd6-204">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-204">Sets the app's environment.</span></span>

<span data-ttu-id="c0dd6-205">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="c0dd6-205">**Key**: environment</span></span>  
<span data-ttu-id="c0dd6-206">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-206">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-207">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="c0dd6-207">**Default**: Production</span></span>  
<span data-ttu-id="c0dd6-208">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="c0dd6-209">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="c0dd6-210">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-210">The environment can be set to any value.</span></span> <span data-ttu-id="c0dd6-211">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="c0dd6-212">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-212">Values aren't case sensitive.</span></span> <span data-ttu-id="c0dd6-213">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="c0dd6-214">Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="c0dd6-215">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-215">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="c0dd6-218">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="c0dd6-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="c0dd6-219">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="c0dd6-220">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="c0dd6-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="c0dd6-221">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-221">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-222">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="c0dd6-222">**Default**: Empty string</span></span>  
<span data-ttu-id="c0dd6-223">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0dd6-224">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="c0dd6-225">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="c0dd6-226">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="c0dd6-227">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="c0dd6-228">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-231">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="c0dd6-232">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="c0dd6-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="c0dd6-233">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="c0dd6-234">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="c0dd6-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="c0dd6-235">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c0dd6-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0dd6-236">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="c0dd6-236">**Default**: true</span></span>  
<span data-ttu-id="c0dd6-237">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="c0dd6-238">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="c0dd6-239">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-242">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="c0dd6-243">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="c0dd6-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="c0dd6-244">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="c0dd6-245">Per altre informazioni, vedere [Migliorare un'app da un assembly esterno](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-245">See [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="c0dd6-246">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="c0dd6-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="c0dd6-247">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c0dd6-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0dd6-248">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="c0dd6-248">**Default**: false</span></span>  
<span data-ttu-id="c0dd6-249">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0dd6-250">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="c0dd6-251">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-254">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="c0dd6-255">URL del server</span><span class="sxs-lookup"><span data-stu-id="c0dd6-255">Server URLs</span></span>

<span data-ttu-id="c0dd6-256">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="c0dd6-257">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="c0dd6-257">**Key**: urls</span></span>  
<span data-ttu-id="c0dd6-258">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-258">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-259">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="c0dd6-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="c0dd6-260">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="c0dd6-261">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="c0dd6-262">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="c0dd6-263">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="c0dd6-264">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="c0dd6-265">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="c0dd6-266">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="c0dd6-268">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="c0dd6-269">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="c0dd6-271">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="c0dd6-271">Shutdown Timeout</span></span>

<span data-ttu-id="c0dd6-272">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="c0dd6-273">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="c0dd6-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="c0dd6-274">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-274">**Type**: *int*</span></span>  
<span data-ttu-id="c0dd6-275">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="c0dd6-275">**Default**: 5</span></span>  
<span data-ttu-id="c0dd6-276">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="c0dd6-277">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="c0dd6-278">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) accetta [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="c0dd6-279">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="c0dd6-280">Durante il periodo di timeout, l'hosting:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="c0dd6-281">Attiva [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="c0dd6-282">Tenta di arrestare i servizi ospitati, registrando eventuali errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="c0dd6-283">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="c0dd6-284">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="c0dd6-285">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-288">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="c0dd6-289">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="c0dd6-289">Startup Assembly</span></span>

<span data-ttu-id="c0dd6-290">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="c0dd6-291">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="c0dd6-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="c0dd6-292">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-292">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-293">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="c0dd6-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="c0dd6-294">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="c0dd6-295">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="c0dd6-296">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="c0dd6-297">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="c0dd6-300">Radice Web</span><span class="sxs-lookup"><span data-stu-id="c0dd6-300">Web Root</span></span>

<span data-ttu-id="c0dd6-301">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="c0dd6-302">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="c0dd6-302">**Key**: webroot</span></span>  
<span data-ttu-id="c0dd6-303">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="c0dd6-303">**Type**: *string*</span></span>  
<span data-ttu-id="c0dd6-304">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="c0dd6-305">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="c0dd6-306">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="c0dd6-307">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="c0dd6-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="c0dd6-310">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="c0dd6-310">Overriding configuration</span></span>

<span data-ttu-id="c0dd6-311">Usare [Configuration](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="c0dd6-312">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente in un file *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="c0dd6-313">Qualsiasi configurazione caricata dal file *hosting.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="c0dd6-314">La configurazione compilata (in `config`) viene usata per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0dd6-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="c0dd6-317">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="c0dd6-320">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="c0dd6-321">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="c0dd6-322">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="c0dd6-323">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="c0dd6-324">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="c0dd6-325">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="c0dd6-326">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="c0dd6-327">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="c0dd6-328">L'argomento della riga di comando esegue l'override del valore `urls`dal file *hosting.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="c0dd6-329">Avvio dell'host</span><span class="sxs-lookup"><span data-stu-id="c0dd6-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0dd6-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0dd6-331">**Run**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-331">**Run**</span></span>

<span data-ttu-id="c0dd6-332">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c0dd6-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-333">**Start**</span></span>

<span data-ttu-id="c0dd6-334">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c0dd6-335">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="c0dd6-336">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="c0dd6-337">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="c0dd6-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="c0dd6-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="c0dd6-339">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c0dd6-340">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c0dd6-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c0dd6-341">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0dd6-342">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0dd6-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="c0dd6-344">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c0dd6-345">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="c0dd6-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c0dd6-347">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="c0dd6-348">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="c0dd6-349">Richiesta</span><span class="sxs-lookup"><span data-stu-id="c0dd6-349">Request</span></span>                                    | <span data-ttu-id="c0dd6-350">Risposta</span><span class="sxs-lookup"><span data-stu-id="c0dd6-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="c0dd6-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="c0dd6-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="c0dd6-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="c0dd6-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="c0dd6-353">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="c0dd6-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="c0dd6-354">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="c0dd6-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="c0dd6-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="c0dd6-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="c0dd6-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="c0dd6-356">Hello World!</span></span>                             |

<span data-ttu-id="c0dd6-357">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0dd6-358">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0dd6-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c0dd6-360">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="c0dd6-361">Produce lo stesso risultato di **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-361">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="c0dd6-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c0dd6-363">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c0dd6-364">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c0dd6-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c0dd6-365">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0dd6-366">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0dd6-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c0dd6-368">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c0dd6-369">Produce lo stesso risultato di **StartWith(Action&lt;IApplicationBuilder&gt; app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-369">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0dd6-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0dd6-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0dd6-371">**Run**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-371">**Run**</span></span>

<span data-ttu-id="c0dd6-372">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c0dd6-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-373">**Start**</span></span>

<span data-ttu-id="c0dd6-374">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c0dd6-375">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="c0dd6-376">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="c0dd6-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="c0dd6-377">L'[interfaccia IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="c0dd6-378">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="c0dd6-379">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="c0dd6-380">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="c0dd6-381">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="c0dd6-382">Per informazioni dettagliate, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-382">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="c0dd6-383">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="c0dd6-384">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="c0dd6-385">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="c0dd6-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="c0dd6-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="c0dd6-387">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="c0dd6-388">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="c0dd6-388">Cancellation Token</span></span>    | <span data-ttu-id="c0dd6-389">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="c0dd6-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="c0dd6-390">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="c0dd6-391">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="c0dd6-392">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-392">Requests may still be processing.</span></span> <span data-ttu-id="c0dd6-393">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="c0dd6-394">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="c0dd6-395">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-395">All requests should be processed.</span></span> <span data-ttu-id="c0dd6-396">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="c0dd6-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="c0dd6-398">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="c0dd6-399">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="c0dd6-399">Scope validation</span></span>

<span data-ttu-id="c0dd6-400">In ASP.NET Core 2.0 o versioni successive [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) su `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="c0dd6-401">Quando `ValidateScopes` è impostato su `true`, il provider di servizi predefinito esegue dei controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c0dd6-402">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c0dd6-403">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c0dd6-404">Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="c0dd6-405">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c0dd6-406">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c0dd6-407">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c0dd6-408">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c0dd6-409">Per convalidare sempre gli ambiti, inclusi quelli dell'ambiente di produzione, configurare [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) nel generatore di host:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="c0dd6-410">Troubleshooting System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="c0dd6-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="c0dd6-411">**Si applica solo ad ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="c0dd6-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="c0dd6-412">È possibile creare un host inserendo `IStartup` direttamente nel contenitore di inserimento delle dipendenze anziché chiamando `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="c0dd6-413">Se l'host viene creato in questo modo, è possibile che si verifichi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="c0dd6-414">Ciò si verifica perché è necessario [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (assembly corrente) per cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="c0dd6-415">Se l'app inserisce manualmente `IStartup` nel contenitore di inserimento delle dipendenze, aggiungere la chiamata seguente a `WebHostBuilder` con il nome dell'assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="c0dd6-416">In alternativa, aggiungere `Configure` fittizio a `WebHostBuilder`, che imposta `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="c0dd6-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="c0dd6-417">**NOTA**: quest'operazione è necessaria solo con la versione ASP.NET Core 2.0 e solo quando l'app non esegue la chiamata a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0dd6-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="c0dd6-418">Per altre informazioni, vedere [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)) e [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs) (Esempio di StartupInjection).</span><span class="sxs-lookup"><span data-stu-id="c0dd6-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0dd6-419">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c0dd6-419">Additional resources</span></span>

* [<span data-ttu-id="c0dd6-420">Host in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="c0dd6-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="c0dd6-421">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="c0dd6-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="c0dd6-422">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="c0dd6-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="c0dd6-423">Host in un servizio di Windows</span><span class="sxs-lookup"><span data-stu-id="c0dd6-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
