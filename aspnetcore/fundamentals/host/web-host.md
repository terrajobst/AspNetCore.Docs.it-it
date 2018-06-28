---
title: Host Web ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ce95599ec8e940635ca63c3bf9a3c28784a3f371
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687490"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="a461f-103">Host Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a461f-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="a461f-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a461f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a461f-105">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="a461f-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="a461f-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="a461f-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a461f-107">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="a461f-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="a461f-108">Questo argomento tratta l'host Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="a461f-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="a461f-109">Per informazioni sull'host generico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a461f-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="a461f-110">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="a461f-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a461f-112">Creare un host usando un'istanza di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a461f-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="a461f-113">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="a461f-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a461f-114">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="a461f-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="a461f-115">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="a461f-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="a461f-116">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="a461f-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="a461f-117">Configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e configura il server usando i provider di configurazione dell'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="a461f-118">Per le opzioni predefinite Kestrel, vedere la [sezione Opzioni Kestrel in Introduzione all'implementazione di server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="a461f-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="a461f-119">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="a461f-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="a461f-120">Carica [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) facoltativo da:</span><span class="sxs-lookup"><span data-stu-id="a461f-120">Loads optional [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) from:</span></span>
  * <span data-ttu-id="a461f-121">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a461f-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="a461f-122">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a461f-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="a461f-123">[Segreti dell'utente](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="a461f-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a461f-124">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="a461f-124">Environment variables.</span></span>
  * <span data-ttu-id="a461f-125">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="a461f-125">Command-line arguments.</span></span>
* <span data-ttu-id="a461f-126">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="a461f-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="a461f-127">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a461f-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="a461f-128">Se eseguito in IIS, abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="a461f-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="a461f-129">Configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a461f-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="a461f-130">Il modulo crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a461f-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="a461f-131">Configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="a461f-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="a461f-132">Per le opzioni predefinite IIS, vedere la [sezione Opzioni IIS in Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a461f-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="a461f-133">Imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a461f-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="a461f-134">Per ulteriori informazioni, vedere [Convalida dell'ambito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="a461f-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="a461f-135">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="a461f-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a461f-136">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="a461f-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a461f-137">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a461f-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a461f-138">Per altre informazioni sulla configurazione delle app, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a461f-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="a461f-139">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a461f-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="a461f-140">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a461f-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a461f-142">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a461f-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="a461f-143">La creazione di un host viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="a461f-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a461f-144">Nei modelli di progetto `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a461f-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="a461f-145">`WebHostBuilder` richiede un [server che implementa IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="a461f-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a461f-146">I server incorporati sono [Kestrel](xref:fundamentals/servers/kestrel) e [HTTP.sys](xref:fundamentals/servers/httpsys) (prima della versione ASP.NET Core 2.0, HTTP.sys era chiamato [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a461f-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="a461f-147">In questo esempio, il [metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a461f-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="a461f-148">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="a461f-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a461f-149">La radice di contenuto predefinita viene ottenuta per `UseContentRoot` tramite [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="a461f-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="a461f-150">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="a461f-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a461f-151">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a461f-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a461f-152">Per usare IIS come proxy inverso, chiamare [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="a461f-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="a461f-153">A differenza di [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` non configura un *server*.</span><span class="sxs-lookup"><span data-stu-id="a461f-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="a461f-154">`UseIISIntegration` configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="a461f-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="a461f-155">Per usare IIS con ASP.NET Core, è necessario specificare `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="a461f-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="a461f-156">`UseIISIntegration` viene attivato solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a461f-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="a461f-157">Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a461f-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a461f-158">Un'implementazione minima che configura un host (e un'app ASP.NET Core) include la specifica di un server e la configurazione della pipeline delle richieste dell'app:</span><span class="sxs-lookup"><span data-stu-id="a461f-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="a461f-159">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="a461f-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="a461f-160">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a461f-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="a461f-161">Per altre informazioni, vedere [Avvio dell'applicazione in ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a461f-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="a461f-162">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="a461f-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a461f-163">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="a461f-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="a461f-164">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="a461f-164">Host configuration values</span></span>

<span data-ttu-id="a461f-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="a461f-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a461f-166">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="a461f-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="a461f-167">Ad esempio `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a461f-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="a461f-168">Metodi espliciti, ad esempio [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="a461f-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="a461f-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="a461f-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="a461f-170">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="a461f-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="a461f-171">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="a461f-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="a461f-172">Per altre informazioni, vedere [Override della configurazione](#override-configuration) nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="a461f-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="a461f-173">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="a461f-173">Capture Startup Errors</span></span>

<span data-ttu-id="a461f-174">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="a461f-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="a461f-175">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="a461f-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="a461f-176">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="a461f-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a461f-177">**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="a461f-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="a461f-178">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="a461f-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="a461f-179">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="a461f-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="a461f-180">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="a461f-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="a461f-181">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="a461f-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="a461f-184">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="a461f-184">Content Root</span></span>

<span data-ttu-id="a461f-185">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="a461f-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="a461f-186">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="a461f-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="a461f-187">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-187">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-188">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a461f-189">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a461f-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a461f-190">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="a461f-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="a461f-191">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="a461f-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="a461f-192">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="a461f-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="a461f-195">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="a461f-195">Detailed Errors</span></span>

<span data-ttu-id="a461f-196">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="a461f-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="a461f-197">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="a461f-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="a461f-198">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="a461f-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a461f-199">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="a461f-199">**Default**: false</span></span>  
<span data-ttu-id="a461f-200">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a461f-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a461f-201">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="a461f-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="a461f-202">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="a461f-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="a461f-205">Ambiente</span><span class="sxs-lookup"><span data-stu-id="a461f-205">Environment</span></span>

<span data-ttu-id="a461f-206">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-206">Sets the app's environment.</span></span>

<span data-ttu-id="a461f-207">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="a461f-207">**Key**: environment</span></span>  
<span data-ttu-id="a461f-208">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-208">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-209">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="a461f-209">**Default**: Production</span></span>  
<span data-ttu-id="a461f-210">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a461f-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a461f-211">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a461f-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="a461f-212">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="a461f-212">The environment can be set to any value.</span></span> <span data-ttu-id="a461f-213">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="a461f-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a461f-214">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="a461f-214">Values aren't case sensitive.</span></span> <span data-ttu-id="a461f-215">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a461f-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a461f-216">Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a461f-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="a461f-217">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a461f-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="a461f-220">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="a461f-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="a461f-221">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="a461f-222">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="a461f-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="a461f-223">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-223">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-224">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="a461f-224">**Default**: Empty string</span></span>  
<span data-ttu-id="a461f-225">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a461f-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a461f-226">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="a461f-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="a461f-227">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="a461f-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="a461f-228">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a461f-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a461f-229">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="a461f-230">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="a461f-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-233">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a461f-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="a461f-234">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="a461f-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="a461f-235">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="a461f-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="a461f-236">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="a461f-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="a461f-237">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="a461f-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a461f-238">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="a461f-238">**Default**: true</span></span>  
<span data-ttu-id="a461f-239">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="a461f-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="a461f-240">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="a461f-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="a461f-241">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a461f-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-244">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a461f-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="a461f-245">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="a461f-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="a461f-246">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="a461f-247">Per altre informazioni, vedere [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) (Migliorare un'app da un assembly esterno con IHostingStartup).</span><span class="sxs-lookup"><span data-stu-id="a461f-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="a461f-248">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="a461f-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="a461f-249">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="a461f-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a461f-250">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="a461f-250">**Default**: false</span></span>  
<span data-ttu-id="a461f-251">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a461f-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a461f-252">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="a461f-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="a461f-253">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a461f-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-256">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a461f-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="a461f-257">URL del server</span><span class="sxs-lookup"><span data-stu-id="a461f-257">Server URLs</span></span>

<span data-ttu-id="a461f-258">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="a461f-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="a461f-259">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="a461f-259">**Key**: urls</span></span>  
<span data-ttu-id="a461f-260">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-260">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-261">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="a461f-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="a461f-262">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="a461f-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="a461f-263">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="a461f-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="a461f-264">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="a461f-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="a461f-265">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="a461f-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="a461f-266">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="a461f-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="a461f-267">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="a461f-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="a461f-268">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="a461f-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="a461f-270">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="a461f-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="a461f-271">Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="a461f-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="a461f-273">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="a461f-273">Shutdown Timeout</span></span>

<span data-ttu-id="a461f-274">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="a461f-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="a461f-275">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="a461f-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="a461f-276">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="a461f-276">**Type**: *int*</span></span>  
<span data-ttu-id="a461f-277">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="a461f-277">**Default**: 5</span></span>  
<span data-ttu-id="a461f-278">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="a461f-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="a461f-279">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="a461f-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="a461f-280">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) accetta [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="a461f-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="a461f-281">Questa funzionalità è stata introdotta in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a461f-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a461f-282">Durante il periodo di timeout, l'hosting:</span><span class="sxs-lookup"><span data-stu-id="a461f-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="a461f-283">Attiva [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="a461f-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="a461f-284">Tenta di arrestare i servizi ospitati, registrando eventuali errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="a461f-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="a461f-285">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="a461f-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="a461f-286">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="a461f-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="a461f-287">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="a461f-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-290">Questa funzionalità non è disponibile in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a461f-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="a461f-291">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="a461f-291">Startup Assembly</span></span>

<span data-ttu-id="a461f-292">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a461f-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="a461f-293">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="a461f-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="a461f-294">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-294">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-295">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="a461f-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="a461f-296">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="a461f-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="a461f-297">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="a461f-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="a461f-298">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="a461f-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="a461f-299">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a461f-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="a461f-302">Radice Web</span><span class="sxs-lookup"><span data-stu-id="a461f-302">Web Root</span></span>

<span data-ttu-id="a461f-303">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="a461f-304">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="a461f-304">**Key**: webroot</span></span>  
<span data-ttu-id="a461f-305">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="a461f-305">**Type**: *string*</span></span>  
<span data-ttu-id="a461f-306">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="a461f-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="a461f-307">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="a461f-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="a461f-308">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="a461f-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="a461f-309">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="a461f-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="a461f-312">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="a461f-312">Override configuration</span></span>

<span data-ttu-id="a461f-313">Usare [Configuration](xref:fundamentals/configuration/index) per configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="a461f-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="a461f-314">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente in un file *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="a461f-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="a461f-315">Qualsiasi configurazione caricata dal file *hosting.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="a461f-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="a461f-316">La configurazione compilata (in `config`) viene usata per configurare l'host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="a461f-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a461f-318">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="a461f-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="a461f-319">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="a461f-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-321">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="a461f-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="a461f-322">Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="a461f-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="a461f-323">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="a461f-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a461f-324">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="a461f-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="a461f-325">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="a461f-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="a461f-326">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="a461f-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a461f-327">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="a461f-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a461f-328">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a461f-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="a461f-329">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="a461f-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="a461f-330">L'argomento della riga di comando esegue l'override del valore `urls`dal file *hosting.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="a461f-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="a461f-331">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="a461f-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a461f-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a461f-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a461f-333">**Run**</span><span class="sxs-lookup"><span data-stu-id="a461f-333">**Run**</span></span>

<span data-ttu-id="a461f-334">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="a461f-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a461f-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="a461f-335">**Start**</span></span>

<span data-ttu-id="a461f-336">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="a461f-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a461f-337">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="a461f-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="a461f-338">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="a461f-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="a461f-339">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="a461f-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="a461f-340">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a461f-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="a461f-341">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a461f-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a461f-342">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a461f-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a461f-343">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="a461f-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a461f-344">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="a461f-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a461f-345">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a461f-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="a461f-346">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a461f-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a461f-347">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a461f-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="a461f-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a461f-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a461f-349">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="a461f-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="a461f-350">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="a461f-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="a461f-351">Richiesta</span><span class="sxs-lookup"><span data-stu-id="a461f-351">Request</span></span>                                    | <span data-ttu-id="a461f-352">Risposta</span><span class="sxs-lookup"><span data-stu-id="a461f-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="a461f-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="a461f-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="a461f-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="a461f-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="a461f-355">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="a461f-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="a461f-356">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="a461f-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="a461f-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="a461f-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="a461f-358">Hello World!</span><span class="sxs-lookup"><span data-stu-id="a461f-358">Hello World!</span></span>                             |

<span data-ttu-id="a461f-359">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="a461f-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a461f-360">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="a461f-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a461f-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a461f-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a461f-362">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a461f-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a461f-363">Produce lo stesso risultato di **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a461f-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="a461f-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="a461f-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a461f-365">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a461f-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a461f-366">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a461f-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a461f-367">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="a461f-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a461f-368">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="a461f-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a461f-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="a461f-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a461f-370">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a461f-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a461f-371">Produce lo stesso risultato di **StartWith(Action&lt;IApplicationBuilder&gt; app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a461f-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a461f-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a461f-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a461f-373">**Run**</span><span class="sxs-lookup"><span data-stu-id="a461f-373">**Run**</span></span>

<span data-ttu-id="a461f-374">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="a461f-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a461f-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="a461f-375">**Start**</span></span>

<span data-ttu-id="a461f-376">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="a461f-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a461f-377">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="a461f-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a461f-378">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="a461f-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="a461f-379">L'[interfaccia IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="a461f-380">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="a461f-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="a461f-381">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="a461f-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="a461f-382">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a461f-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="a461f-383">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="a461f-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="a461f-384">Per informazioni dettagliate, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a461f-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="a461f-385">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="a461f-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="a461f-386">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="a461f-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a461f-387">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="a461f-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="a461f-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="a461f-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="a461f-389">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="a461f-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="a461f-390">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="a461f-390">Cancellation Token</span></span>    | <span data-ttu-id="a461f-391">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="a461f-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="a461f-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="a461f-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="a461f-393">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="a461f-393">The host has fully started.</span></span> |
| [<span data-ttu-id="a461f-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="a461f-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="a461f-395">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="a461f-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a461f-396">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="a461f-396">All requests should be processed.</span></span> <span data-ttu-id="a461f-397">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="a461f-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="a461f-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="a461f-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="a461f-399">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="a461f-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a461f-400">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a461f-400">Requests may still be processing.</span></span> <span data-ttu-id="a461f-401">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="a461f-401">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="a461f-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="a461f-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="a461f-403">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="a461f-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="a461f-404">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="a461f-404">Scope validation</span></span>

<span data-ttu-id="a461f-405">In ASP.NET Core 2.0 o versioni successive [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) su `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a461f-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="a461f-406">Quando `ValidateScopes` è impostato su `true`, il provider di servizi predefinito esegue dei controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="a461f-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="a461f-407">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="a461f-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="a461f-408">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="a461f-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="a461f-409">Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="a461f-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="a461f-410">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="a461f-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="a461f-411">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="a461f-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="a461f-412">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="a461f-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="a461f-413">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="a461f-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="a461f-414">Per convalidare sempre gli ambiti, inclusi quelli dell'ambiente di produzione, configurare [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) nel generatore di host:</span><span class="sxs-lookup"><span data-stu-id="a461f-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="a461f-415">Troubleshooting System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="a461f-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="a461f-416">**Si applica solo ad ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="a461f-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="a461f-417">È possibile creare un host inserendo `IStartup` direttamente nel contenitore di inserimento delle dipendenze anziché chiamando `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="a461f-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="a461f-418">Se l'host viene creato in questo modo, è possibile che si verifichi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="a461f-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="a461f-419">Ciò si verifica perché è necessario [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (assembly corrente) per cercare `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="a461f-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="a461f-420">Se l'app inserisce manualmente `IStartup` nel contenitore di inserimento delle dipendenze, aggiungere la chiamata seguente a `WebHostBuilder` con il nome dell'assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="a461f-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="a461f-421">In alternativa, aggiungere `Configure` fittizio a `WebHostBuilder`, che imposta `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="a461f-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="a461f-422">**NOTA**: quest'operazione è necessaria solo con la versione ASP.NET Core 2.0 e solo quando l'app non esegue la chiamata a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a461f-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="a461f-423">Per altre informazioni, vedere [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)) e [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs) (Esempio di StartupInjection).</span><span class="sxs-lookup"><span data-stu-id="a461f-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a461f-424">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a461f-424">Additional resources</span></span>

* [<span data-ttu-id="a461f-425">Host in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="a461f-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="a461f-426">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="a461f-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="a461f-427">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="a461f-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="a461f-428">Host in un servizio di Windows</span><span class="sxs-lookup"><span data-stu-id="a461f-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
