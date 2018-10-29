---
title: Host Web ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: e19f12f69dfdd5653aea9c6be2b05f24009b875e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477449"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="51ae7-103">Host Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51ae7-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="51ae7-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51ae7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="51ae7-105">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="51ae7-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="51ae7-107">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="51ae7-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="51ae7-108">Questo argomento tratta l'host Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="51ae7-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="51ae7-109">Per informazioni sull'host generico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="51ae7-110">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="51ae7-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="51ae7-111">Creare un host usando un'istanza di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="51ae7-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="51ae7-112">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="51ae7-113">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="51ae7-114">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="51ae7-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="51ae7-115">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="51ae7-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="51ae7-116">Configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e configura il server usando i provider di configurazione dell'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="51ae7-117">Per le opzioni predefinite di Kestrel, vedere <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="51ae7-118">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="51ae7-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="51ae7-119">Carica la [configurazione dell'host](#host-configuration-values) da:</span><span class="sxs-lookup"><span data-stu-id="51ae7-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="51ae7-120">Le variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="51ae7-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="51ae7-121">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="51ae7-121">Command-line arguments.</span></span>
* <span data-ttu-id="51ae7-122">Carica la configurazione dell'app nell'ordine seguente da:</span><span class="sxs-lookup"><span data-stu-id="51ae7-122">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="51ae7-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="51ae7-124">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="51ae7-125">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="51ae7-125">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="51ae7-126">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="51ae7-126">Environment variables.</span></span>
  * <span data-ttu-id="51ae7-127">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="51ae7-127">Command-line arguments.</span></span>
* <span data-ttu-id="51ae7-128">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="51ae7-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="51ae7-129">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="51ae7-130">Se eseguito in IIS, abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="51ae7-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="51ae7-131">Configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="51ae7-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="51ae7-132">Il modulo crea un proxy inverso tra IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="51ae7-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="51ae7-133">Configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="51ae7-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="51ae7-134">Per le opzioni predefinite di IIS, vedere <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="51ae7-135">Imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="51ae7-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="51ae7-136">Per ulteriori informazioni, vedere [Convalida dell'ambito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="51ae7-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="51ae7-137">La configurazione definita da `CreateDefaultBuilder` può essere sottoposta a override e aumentata tramite [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e altri metodi e metodi di estensione di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="51ae7-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="51ae7-138">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="51ae7-138">A few examples follow:</span></span>

* <span data-ttu-id="51ae7-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) viene usato per specificare altri elementi `IConfiguration` per l'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="51ae7-140">La chiamata a `ConfigureAppConfiguration` seguente consente di aggiungere un delegato per includere la configurazione dell'app nel file *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="51ae7-141">`ConfigureAppConfiguration` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="51ae7-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="51ae7-142">Si noti che questa configurazione non è valida per l'host (ad esempio, gli URL del server o l'ambiente).</span><span class="sxs-lookup"><span data-stu-id="51ae7-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="51ae7-143">Vedere la sezione [Valori di configurazione dell'host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="51ae7-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="51ae7-144">La chiamata a `ConfigureLogging` seguente consente di aggiungere un delegato per configurare il livello di registrazione minimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="51ae7-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="51ae7-145">Questa impostazione sostituisce le impostazioni in *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configurate da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="51ae7-146">`ConfigureLogging` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="51ae7-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="51ae7-147">La chiamata a `ConfigureKestrel` seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="51ae7-148">La chiamata a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="51ae7-149">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="51ae7-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="51ae7-150">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="51ae7-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="51ae7-151">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="51ae7-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="51ae7-152">Per altre informazioni sulla configurazione dell'app, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="51ae7-153">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="51ae7-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="51ae7-154">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="51ae7-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="51ae7-155">Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="51ae7-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="51ae7-156">La creazione di un host viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="51ae7-157">Nei modelli di progetto `Main` si trova in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="51ae7-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="51ae7-158">`WebHostBuilder` richiede un [server che implementa IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="51ae7-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="51ae7-159">I server incorporati sono [Kestrel](xref:fundamentals/servers/kestrel) e [HTTP.sys](xref:fundamentals/servers/httpsys) (prima della versione ASP.NET Core 2.0, HTTP.sys era chiamato [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="51ae7-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="51ae7-160">In questo esempio, il [metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="51ae7-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="51ae7-161">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="51ae7-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="51ae7-162">La radice di contenuto predefinita viene ottenuta per `UseContentRoot` tramite [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="51ae7-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="51ae7-163">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="51ae7-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="51ae7-164">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="51ae7-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="51ae7-165">Per usare IIS come proxy inverso, chiamare [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="51ae7-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="51ae7-166">A differenza di [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` non configura un *server*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="51ae7-167">`UseIISIntegration` configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="51ae7-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="51ae7-168">Per usare IIS con ASP.NET Core, è necessario specificare `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="51ae7-169">`UseIISIntegration` viene attivato solo quando si esegue IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="51ae7-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="51ae7-170">Per altre informazioni, vedere <xref:fundamentals/servers/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="51ae7-171">Un'implementazione minima che configura un host (e un'app ASP.NET Core) include la specifica di un server e la configurazione della pipeline delle richieste dell'app:</span><span class="sxs-lookup"><span data-stu-id="51ae7-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="51ae7-172">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="51ae7-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="51ae7-173">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="51ae7-174">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="51ae7-175">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="51ae7-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="51ae7-176">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="51ae7-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="51ae7-177">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="51ae7-177">Host configuration values</span></span>

<span data-ttu-id="51ae7-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="51ae7-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="51ae7-179">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="51ae7-180">Ad esempio `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="51ae7-181">Le estensioni come [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vedere la sezione [Override della configurazione](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="51ae7-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="51ae7-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="51ae7-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="51ae7-183">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="51ae7-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="51ae7-184">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="51ae7-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="51ae7-185">Per altre informazioni, vedere [Override della configurazione](#override-configuration) nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="51ae7-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="51ae7-186">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="51ae7-186">Application Key (Name)</span></span>

<span data-ttu-id="51ae7-187">La proprietà [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) viene impostata automaticamente quando si chiama [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="51ae7-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="51ae7-188">Il valore viene impostato sul nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="51ae7-189">Per impostare il valore in modo esplicito, usare [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="51ae7-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="51ae7-190">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="51ae7-190">**Key**: applicationName</span></span>  
<span data-ttu-id="51ae7-191">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-191">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-192">**Impostazione predefinita**: il nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="51ae7-193">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="51ae7-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="51ae7-194">**Variabile di ambiente**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="51ae7-194">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="51ae7-195">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="51ae7-195">Capture Startup Errors</span></span>

<span data-ttu-id="51ae7-196">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="51ae7-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="51ae7-197">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="51ae7-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="51ae7-198">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="51ae7-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="51ae7-199">**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="51ae7-200">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="51ae7-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="51ae7-201">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="51ae7-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="51ae7-202">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="51ae7-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="51ae7-203">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="51ae7-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="51ae7-204">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="51ae7-204">Content Root</span></span>

<span data-ttu-id="51ae7-205">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="51ae7-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="51ae7-206">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="51ae7-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="51ae7-207">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-207">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-208">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="51ae7-209">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="51ae7-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="51ae7-210">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="51ae7-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="51ae7-211">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="51ae7-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="51ae7-212">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="51ae7-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="51ae7-213">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="51ae7-213">Detailed Errors</span></span>

<span data-ttu-id="51ae7-214">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="51ae7-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="51ae7-215">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="51ae7-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="51ae7-216">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="51ae7-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="51ae7-217">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="51ae7-217">**Default**: false</span></span>  
<span data-ttu-id="51ae7-218">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="51ae7-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="51ae7-219">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="51ae7-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="51ae7-220">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="51ae7-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="51ae7-221">Ambiente</span><span class="sxs-lookup"><span data-stu-id="51ae7-221">Environment</span></span>

<span data-ttu-id="51ae7-222">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-222">Sets the app's environment.</span></span>

<span data-ttu-id="51ae7-223">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="51ae7-223">**Key**: environment</span></span>  
<span data-ttu-id="51ae7-224">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-224">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-225">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="51ae7-225">**Default**: Production</span></span>  
<span data-ttu-id="51ae7-226">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="51ae7-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="51ae7-227">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="51ae7-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="51ae7-228">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="51ae7-228">The environment can be set to any value.</span></span> <span data-ttu-id="51ae7-229">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="51ae7-230">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="51ae7-230">Values aren't case sensitive.</span></span> <span data-ttu-id="51ae7-231">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="51ae7-232">Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="51ae7-233">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="51ae7-234">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="51ae7-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="51ae7-235">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="51ae7-236">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="51ae7-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="51ae7-237">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-237">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-238">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="51ae7-238">**Default**: Empty string</span></span>  
<span data-ttu-id="51ae7-239">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="51ae7-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="51ae7-240">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="51ae7-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="51ae7-241">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="51ae7-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="51ae7-242">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="51ae7-243">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="51ae7-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="51ae7-244">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="51ae7-244">HTTPS Port</span></span>

<span data-ttu-id="51ae7-245">Impostare la porta di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="51ae7-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="51ae7-246">Usata per [imporre HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="51ae7-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="51ae7-247">**Chiave**: https_port **Tipo**: *string*
**Valore predefinito**: non è impostato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="51ae7-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="51ae7-248">**Impostazione con**: `UseSetting`
**Variabile di ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="51ae7-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="51ae7-249">Assembly di avvio dell'hosting da escludere</span><span class="sxs-lookup"><span data-stu-id="51ae7-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="51ae7-250">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da escludere all'avvio.</span><span class="sxs-lookup"><span data-stu-id="51ae7-250">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="51ae7-251">**Chiave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="51ae7-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="51ae7-252">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-252">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-253">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="51ae7-253">**Default**: Empty string</span></span>  
<span data-ttu-id="51ae7-254">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="51ae7-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="51ae7-255">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="51ae7-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="51ae7-256">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="51ae7-256">Prefer Hosting URLs</span></span>

<span data-ttu-id="51ae7-257">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-257">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="51ae7-258">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="51ae7-258">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="51ae7-259">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="51ae7-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="51ae7-260">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="51ae7-260">**Default**: true</span></span>  
<span data-ttu-id="51ae7-261">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="51ae7-261">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="51ae7-262">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="51ae7-262">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="51ae7-263">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="51ae7-263">Prevent Hosting Startup</span></span>

<span data-ttu-id="51ae7-264">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-264">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="51ae7-265">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-265">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="51ae7-266">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="51ae7-266">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="51ae7-267">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="51ae7-267">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="51ae7-268">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="51ae7-268">**Default**: false</span></span>  
<span data-ttu-id="51ae7-269">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="51ae7-269">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="51ae7-270">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="51ae7-270">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="51ae7-271">URL del server</span><span class="sxs-lookup"><span data-stu-id="51ae7-271">Server URLs</span></span>

<span data-ttu-id="51ae7-272">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="51ae7-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="51ae7-273">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="51ae7-273">**Key**: urls</span></span>  
<span data-ttu-id="51ae7-274">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-274">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-275">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="51ae7-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="51ae7-276">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="51ae7-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="51ae7-277">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="51ae7-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="51ae7-278">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="51ae7-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="51ae7-279">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="51ae7-280">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="51ae7-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="51ae7-281">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="51ae7-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="51ae7-282">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="51ae7-282">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="51ae7-283">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="51ae7-283">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="51ae7-284">Per ulteriori informazioni, vedere <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-284">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="51ae7-285">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="51ae7-285">Shutdown Timeout</span></span>

<span data-ttu-id="51ae7-286">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="51ae7-286">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="51ae7-287">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="51ae7-287">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="51ae7-288">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="51ae7-288">**Type**: *int*</span></span>  
<span data-ttu-id="51ae7-289">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="51ae7-289">**Default**: 5</span></span>  
<span data-ttu-id="51ae7-290">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="51ae7-290">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="51ae7-291">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="51ae7-291">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="51ae7-292">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) accetta [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="51ae7-292">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="51ae7-293">Durante il periodo di timeout, l'hosting:</span><span class="sxs-lookup"><span data-stu-id="51ae7-293">During the timeout period, hosting:</span></span>

* <span data-ttu-id="51ae7-294">Attiva [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="51ae7-294">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="51ae7-295">Tenta di arrestare i servizi ospitati, registrando eventuali errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="51ae7-295">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="51ae7-296">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="51ae7-296">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="51ae7-297">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="51ae7-297">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="51ae7-298">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="51ae7-298">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="51ae7-299">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="51ae7-299">Startup Assembly</span></span>

<span data-ttu-id="51ae7-300">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-300">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="51ae7-301">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="51ae7-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="51ae7-302">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-302">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-303">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="51ae7-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="51ae7-304">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="51ae7-304">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="51ae7-305">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="51ae7-305">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="51ae7-306">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="51ae7-306">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="51ae7-307">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="51ae7-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="51ae7-308">Radice Web</span><span class="sxs-lookup"><span data-stu-id="51ae7-308">Web Root</span></span>

<span data-ttu-id="51ae7-309">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-309">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="51ae7-310">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="51ae7-310">**Key**: webroot</span></span>  
<span data-ttu-id="51ae7-311">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="51ae7-311">**Type**: *string*</span></span>  
<span data-ttu-id="51ae7-312">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="51ae7-312">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="51ae7-313">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="51ae7-313">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="51ae7-314">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="51ae7-314">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="51ae7-315">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="51ae7-315">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="51ae7-316">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="51ae7-316">Override configuration</span></span>

<span data-ttu-id="51ae7-317">Usare la [Configurazione](xref:fundamentals/configuration/index) per configurare l'host Web.</span><span class="sxs-lookup"><span data-stu-id="51ae7-317">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="51ae7-318">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente nel file *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="51ae7-318">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="51ae7-319">Qualsiasi configurazione caricata dal file *hostsettings.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="51ae7-319">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="51ae7-320">La configurazione compilata (in `config`) viene usata per configurare l'host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="51ae7-320">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="51ae7-321">La configurazione di `IWebHostBuilder` viene aggiunta alla configurazione dell'app, ma non il contrario: &mdash;`ConfigureAppConfiguration` non influenza la configurazione di `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-321">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="51ae7-322">Esecuzione dell'override della configurazione specificata da `UseUrls` con prima la configurazione *hostsettings.json* e quindi con la configurazione dell'argomento della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="51ae7-322">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="51ae7-323">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="51ae7-323">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="51ae7-324">Esecuzione dell'override della configurazione specificata da `UseUrls` con prima la configurazione *hostsettings.json* e quindi con la configurazione dell'argomento della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="51ae7-324">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="51ae7-325">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="51ae7-325">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="51ae7-326">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-326">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="51ae7-327">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="51ae7-327">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="51ae7-328">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="51ae7-328">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="51ae7-329">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="51ae7-329">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="51ae7-330">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="51ae7-330">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="51ae7-331">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="51ae7-331">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="51ae7-332">`UseConfiguration` copia solo le chiavi dall'elemento `IConfiguration` specificato nella configurazione del generatore di host.</span><span class="sxs-lookup"><span data-stu-id="51ae7-332">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="51ae7-333">Pertanto, l'impostazione di `reloadOnChange: true` per i file JSON, INI e XML non ha alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="51ae7-333">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="51ae7-334">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="51ae7-334">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="51ae7-335">L'argomento della riga di comando esegue l'override del valore `urls` dal file *hostsettings.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="51ae7-335">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="51ae7-336">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="51ae7-336">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="51ae7-337">**Run**</span><span class="sxs-lookup"><span data-stu-id="51ae7-337">**Run**</span></span>

<span data-ttu-id="51ae7-338">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="51ae7-338">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="51ae7-339">**Start**</span><span class="sxs-lookup"><span data-stu-id="51ae7-339">**Start**</span></span>

<span data-ttu-id="51ae7-340">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-340">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="51ae7-341">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="51ae7-341">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="51ae7-342">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="51ae7-342">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="51ae7-343">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="51ae7-343">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="51ae7-344">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-344">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="51ae7-345">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-345">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="51ae7-346">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="51ae7-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="51ae7-347">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="51ae7-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="51ae7-348">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="51ae7-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="51ae7-349">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-349">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="51ae7-350">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-350">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="51ae7-351">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-351">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="51ae7-352">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-352">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="51ae7-353">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="51ae7-353">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="51ae7-354">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="51ae7-354">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="51ae7-355">Richiesta</span><span class="sxs-lookup"><span data-stu-id="51ae7-355">Request</span></span>                                    | <span data-ttu-id="51ae7-356">Risposta</span><span class="sxs-lookup"><span data-stu-id="51ae7-356">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="51ae7-357">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="51ae7-357">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="51ae7-358">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="51ae7-358">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="51ae7-359">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="51ae7-359">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="51ae7-360">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="51ae7-360">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="51ae7-361">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="51ae7-361">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="51ae7-362">Hello World!</span><span class="sxs-lookup"><span data-stu-id="51ae7-362">Hello World!</span></span>                             |

<span data-ttu-id="51ae7-363">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="51ae7-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="51ae7-364">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="51ae7-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="51ae7-365">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-365">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="51ae7-366">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-366">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="51ae7-367">Produce lo stesso risultato di **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-367">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="51ae7-368">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-368">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="51ae7-369">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-369">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="51ae7-370">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="51ae7-370">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="51ae7-371">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="51ae7-371">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="51ae7-372">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="51ae7-372">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="51ae7-373">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="51ae7-373">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="51ae7-374">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-374">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="51ae7-375">Produce lo stesso risultato di **StartWith(Action&lt;IApplicationBuilder&gt; app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-375">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="51ae7-376">**Run**</span><span class="sxs-lookup"><span data-stu-id="51ae7-376">**Run**</span></span>

<span data-ttu-id="51ae7-377">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="51ae7-377">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="51ae7-378">**Start**</span><span class="sxs-lookup"><span data-stu-id="51ae7-378">**Start**</span></span>

<span data-ttu-id="51ae7-379">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-379">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="51ae7-380">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="51ae7-380">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="51ae7-381">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="51ae7-381">IHostingEnvironment interface</span></span>

<span data-ttu-id="51ae7-382">L'[interfaccia IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-382">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="51ae7-383">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="51ae7-383">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="51ae7-384">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#environment-based-startup-class-and-methods) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="51ae7-384">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="51ae7-385">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-385">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="51ae7-386">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-386">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="51ae7-387">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="51ae7-387">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="51ae7-388">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="51ae7-388">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="51ae7-389">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/index#write-middleware) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="51ae7-389">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="51ae7-390">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="51ae7-390">IApplicationLifetime interface</span></span>

<span data-ttu-id="51ae7-391">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="51ae7-391">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="51ae7-392">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="51ae7-392">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="51ae7-393">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="51ae7-393">Cancellation Token</span></span>    | <span data-ttu-id="51ae7-394">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="51ae7-394">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="51ae7-395">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="51ae7-395">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="51ae7-396">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="51ae7-396">The host has fully started.</span></span> |
| [<span data-ttu-id="51ae7-397">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="51ae7-397">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="51ae7-398">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="51ae7-398">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="51ae7-399">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="51ae7-399">All requests should be processed.</span></span> <span data-ttu-id="51ae7-400">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="51ae7-400">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="51ae7-401">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="51ae7-401">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="51ae7-402">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="51ae7-402">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="51ae7-403">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="51ae7-403">Requests may still be processing.</span></span> <span data-ttu-id="51ae7-404">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="51ae7-404">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="51ae7-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="51ae7-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="51ae7-406">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="51ae7-406">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="51ae7-407">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="51ae7-407">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="51ae7-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) su `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="51ae7-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="51ae7-409">Quando `ValidateScopes` è impostato su `true`, il provider di servizi predefinito esegue dei controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="51ae7-409">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="51ae7-410">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="51ae7-410">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="51ae7-411">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="51ae7-411">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="51ae7-412">Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="51ae7-412">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="51ae7-413">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="51ae7-413">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="51ae7-414">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="51ae7-414">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="51ae7-415">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="51ae7-415">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="51ae7-416">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="51ae7-416">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="51ae7-417">Per convalidare sempre gli ambiti, inclusi quelli dell'ambiente di produzione, configurare [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) nel generatore di host:</span><span class="sxs-lookup"><span data-stu-id="51ae7-417">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="51ae7-418">Troubleshooting System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="51ae7-418">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="51ae7-419">**Quanto segue si applica solo alle app ASP.NET Core 2.0 quando l'app non chiama `UseStartup` o `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="51ae7-419">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="51ae7-420">È possibile creare un host inserendo `IStartup` direttamente nel contenitore di inserimento delle dipendenze anziché chiamando `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="51ae7-420">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="51ae7-421">Se l'host viene creato in questo modo, è possibile che si verifichi l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="51ae7-421">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="51ae7-422">Ciò si verifica perché, per cercare `HostingStartupAttributes`, è necessario il nome dell'app, vale a dire il nome dell'assembly corrente.</span><span class="sxs-lookup"><span data-stu-id="51ae7-422">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="51ae7-423">Se l'app inserisce manualmente `IStartup` nel contenitore di inserimento delle dipendenze, aggiungere la chiamata seguente a `WebHostBuilder` con il nome dell'assembly specificato:</span><span class="sxs-lookup"><span data-stu-id="51ae7-423">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="51ae7-424">In alternativa, aggiungere un valore `Configure` fittizio a `WebHostBuilder`, che imposta automaticamente il nome dell'app:</span><span class="sxs-lookup"><span data-stu-id="51ae7-424">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="51ae7-425">Per altre informazioni, vedere [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)) e [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs) (Esempio di StartupInjection).</span><span class="sxs-lookup"><span data-stu-id="51ae7-425">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="51ae7-426">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="51ae7-426">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
