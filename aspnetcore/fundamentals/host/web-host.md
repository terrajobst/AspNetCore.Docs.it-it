---
title: Host Web ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 48f3b664d901bdfb27cdf9e798fa60c0587d1def
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610285"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="e69c8-103">Host Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e69c8-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="e69c8-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e69c8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e69c8-105">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="e69c8-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e69c8-107">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="e69c8-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="e69c8-108">L'host può anche configurare la registrazione, l'inserimento delle dipendenze e la configurazione.</span><span class="sxs-lookup"><span data-stu-id="e69c8-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e69c8-109">Per la versione 1.1 di questo argomento, scaricare [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) (Host Web ASP.NET Core - Versione 1.1, PDF).</span><span class="sxs-lookup"><span data-stu-id="e69c8-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="e69c8-110">Questo articolo descrive l'host Web ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), che serve per l'hosting di app web.</span><span class="sxs-lookup"><span data-stu-id="e69c8-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="e69c8-111">Per informazioni sull'host generico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="e69c8-112">Questo articolo descrive l'host Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="e69c8-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="e69c8-113">In ASP.NET Core 3.0, l'host generico sostituisce l'host Web.</span><span class="sxs-lookup"><span data-stu-id="e69c8-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="e69c8-114">Per altre informazioni, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="e69c8-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="e69c8-115">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="e69c8-115">Set up a host</span></span>

<span data-ttu-id="e69c8-116">Creare un host usando un'istanza di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e69c8-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="e69c8-117">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="e69c8-118">Il nome del metodo del generatore `CreateWebHostBuilder` è un nome speciale che identifica il metodo del generatore per i componenti esterni, ad esempio [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="e69c8-118">The builder method name, `CreateWebHostBuilder`, is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="e69c8-119">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-119">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="e69c8-120">Un'app tipica chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="e69c8-120">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="e69c8-121">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="e69c8-121">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="e69c8-122">Configura il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web usando i provider di configurazione dell'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-122">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="e69c8-123">Per le opzioni predefinite del server Kestrel, vedere <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-123">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="e69c8-124">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="e69c8-124">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="e69c8-125">Carica la [configurazione dell'host](#host-configuration-values) da:</span><span class="sxs-lookup"><span data-stu-id="e69c8-125">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="e69c8-126">Le variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="e69c8-126">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="e69c8-127">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e69c8-127">Command-line arguments.</span></span>
* <span data-ttu-id="e69c8-128">Carica la configurazione dell'app nell'ordine seguente da:</span><span class="sxs-lookup"><span data-stu-id="e69c8-128">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="e69c8-129">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-129">*appsettings.json*.</span></span>
  * <span data-ttu-id="e69c8-130">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-130">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="e69c8-131">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="e69c8-131">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="e69c8-132">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="e69c8-132">Environment variables.</span></span>
  * <span data-ttu-id="e69c8-133">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e69c8-133">Command-line arguments.</span></span>
* <span data-ttu-id="e69c8-134">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="e69c8-134">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="e69c8-135">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-135">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="e69c8-136">Se eseguito in IIS con il [Modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index), che configura l'indirizzo di base e la porta dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-136">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="e69c8-137">L'integrazione di IIS configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="e69c8-137">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="e69c8-138">Per le opzioni predefinite di IIS, vedere <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-138">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="e69c8-139">Imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e69c8-139">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="e69c8-140">Per ulteriori informazioni, vedere [Convalida dell'ambito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="e69c8-140">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="e69c8-141">La configurazione definita da `CreateDefaultBuilder` può essere sottoposta a override e aumentata tramite [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e altri metodi e metodi di estensione di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e69c8-141">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="e69c8-142">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="e69c8-142">A few examples follow:</span></span>

* <span data-ttu-id="e69c8-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) viene usato per specificare altri elementi `IConfiguration` per l'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="e69c8-144">La chiamata a `ConfigureAppConfiguration` seguente consente di aggiungere un delegato per includere la configurazione dell'app nel file *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-144">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="e69c8-145">`ConfigureAppConfiguration` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="e69c8-145">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="e69c8-146">Si noti che questa configurazione non è valida per l'host (ad esempio, gli URL del server o l'ambiente).</span><span class="sxs-lookup"><span data-stu-id="e69c8-146">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="e69c8-147">Vedere la sezione [Valori di configurazione dell'host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="e69c8-147">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="e69c8-148">La chiamata a `ConfigureLogging` seguente consente di aggiungere un delegato per configurare il livello di registrazione minimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="e69c8-148">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="e69c8-149">Questa impostazione sostituisce le impostazioni in *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configurate da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-149">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e69c8-150">`ConfigureLogging` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="e69c8-150">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="e69c8-151">La chiamata a `ConfigureKestrel` seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-151">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="e69c8-152">La chiamata a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-152">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="e69c8-153">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="e69c8-153">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="e69c8-154">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e69c8-154">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="e69c8-155">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://visualstudio.microsoft.com) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="e69c8-155">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="e69c8-156">Per altre informazioni sulla configurazione dell'app, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-156">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="e69c8-157">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="e69c8-157">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="e69c8-158">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e69c8-158">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="e69c8-159">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="e69c8-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="e69c8-160">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="e69c8-161">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="e69c8-162">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="e69c8-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="e69c8-163">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="e69c8-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="e69c8-164">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="e69c8-164">Host configuration values</span></span>

<span data-ttu-id="e69c8-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="e69c8-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="e69c8-166">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="e69c8-167">Ad esempio `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="e69c8-168">Le estensioni come [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vedere la sezione [Override della configurazione](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="e69c8-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="e69c8-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="e69c8-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="e69c8-170">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="e69c8-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="e69c8-171">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="e69c8-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="e69c8-172">Per altre informazioni, vedere [Override della configurazione](#override-configuration) nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="e69c8-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="e69c8-173">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="e69c8-173">Application Key (Name)</span></span>

<span data-ttu-id="e69c8-174">La proprietà [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) viene impostata automaticamente quando si chiama [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="e69c8-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="e69c8-175">Il valore viene impostato sul nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="e69c8-176">Per impostare il valore in modo esplicito, usare [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="e69c8-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="e69c8-177">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="e69c8-177">**Key**: applicationName</span></span>  
<span data-ttu-id="e69c8-178">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-178">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-179">**Impostazione predefinita**: nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="e69c8-180">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e69c8-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e69c8-181">**Variabile di ambiente**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="e69c8-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="e69c8-182">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="e69c8-182">Capture Startup Errors</span></span>

<span data-ttu-id="e69c8-183">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="e69c8-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="e69c8-184">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="e69c8-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="e69c8-185">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e69c8-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e69c8-186">**Impostazione predefinita**: il valore predefinito è `false`, a meno che l'app non venga eseguita con Kestrel in IIS; in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="e69c8-187">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="e69c8-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="e69c8-188">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="e69c8-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="e69c8-189">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="e69c8-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e69c8-190">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="e69c8-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="e69c8-191">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="e69c8-191">Content Root</span></span>

<span data-ttu-id="e69c8-192">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="e69c8-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="e69c8-193">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="e69c8-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="e69c8-194">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-194">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-195">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="e69c8-196">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="e69c8-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="e69c8-197">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="e69c8-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="e69c8-198">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="e69c8-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="e69c8-199">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="e69c8-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="e69c8-200">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="e69c8-200">Detailed Errors</span></span>

<span data-ttu-id="e69c8-201">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="e69c8-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="e69c8-202">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="e69c8-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="e69c8-203">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e69c8-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e69c8-204">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="e69c8-204">**Default**: false</span></span>  
<span data-ttu-id="e69c8-205">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e69c8-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e69c8-206">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="e69c8-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="e69c8-207">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="e69c8-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="e69c8-208">Ambiente</span><span class="sxs-lookup"><span data-stu-id="e69c8-208">Environment</span></span>

<span data-ttu-id="e69c8-209">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-209">Sets the app's environment.</span></span>

<span data-ttu-id="e69c8-210">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="e69c8-210">**Key**: environment</span></span>  
<span data-ttu-id="e69c8-211">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-211">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-212">**Impostazione predefinita**: Produzione</span><span class="sxs-lookup"><span data-stu-id="e69c8-212">**Default**: Production</span></span>  
<span data-ttu-id="e69c8-213">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="e69c8-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="e69c8-214">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="e69c8-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="e69c8-215">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="e69c8-215">The environment can be set to any value.</span></span> <span data-ttu-id="e69c8-216">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e69c8-217">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e69c8-217">Values aren't case sensitive.</span></span> <span data-ttu-id="e69c8-218">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="e69c8-219">Quando si usa [Visual Studio](https://visualstudio.microsoft.com), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="e69c8-220">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="e69c8-221">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="e69c8-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="e69c8-222">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="e69c8-223">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="e69c8-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="e69c8-224">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-224">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-225">**Impostazione predefinita**: Stringa vuota</span><span class="sxs-lookup"><span data-stu-id="e69c8-225">**Default**: Empty string</span></span>  
<span data-ttu-id="e69c8-226">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e69c8-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e69c8-227">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e69c8-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="e69c8-228">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e69c8-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="e69c8-229">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="e69c8-230">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="e69c8-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="e69c8-231">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="e69c8-231">HTTPS Port</span></span>

<span data-ttu-id="e69c8-232">Impostare la porta di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e69c8-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="e69c8-233">Usata per [imporre HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="e69c8-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="e69c8-234">**Chiave**: https_port **Tipo**: *string*
**Impostazione predefinita**: non è impostato nessun valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="e69c8-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="e69c8-235">**Impostare usando**: `UseSetting`
**Variabile di ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="e69c8-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="e69c8-236">Assembly di avvio dell'hosting da escludere</span><span class="sxs-lookup"><span data-stu-id="e69c8-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="e69c8-237">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da escludere all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e69c8-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="e69c8-238">**Chiave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="e69c8-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="e69c8-239">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-239">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-240">**Impostazione predefinita**: Stringa vuota</span><span class="sxs-lookup"><span data-stu-id="e69c8-240">**Default**: Empty string</span></span>  
<span data-ttu-id="e69c8-241">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e69c8-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e69c8-242">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e69c8-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="e69c8-243">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="e69c8-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="e69c8-244">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="e69c8-245">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="e69c8-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="e69c8-246">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e69c8-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e69c8-247">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="e69c8-247">**Default**: true</span></span>  
<span data-ttu-id="e69c8-248">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="e69c8-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="e69c8-249">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="e69c8-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="e69c8-250">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="e69c8-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="e69c8-251">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="e69c8-252">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="e69c8-253">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e69c8-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="e69c8-254">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="e69c8-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e69c8-255">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="e69c8-255">**Default**: false</span></span>  
<span data-ttu-id="e69c8-256">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e69c8-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e69c8-257">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="e69c8-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="e69c8-258">URL del server</span><span class="sxs-lookup"><span data-stu-id="e69c8-258">Server URLs</span></span>

<span data-ttu-id="e69c8-259">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="e69c8-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="e69c8-260">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="e69c8-260">**Key**: urls</span></span>  
<span data-ttu-id="e69c8-261">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-261">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-262">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="e69c8-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="e69c8-263">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="e69c8-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="e69c8-264">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="e69c8-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="e69c8-265">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="e69c8-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="e69c8-266">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e69c8-267">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="e69c8-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="e69c8-268">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="e69c8-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e69c8-269">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="e69c8-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="e69c8-270">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="e69c8-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="e69c8-271">Per ulteriori informazioni, vedere <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="e69c8-272">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="e69c8-272">Shutdown Timeout</span></span>

<span data-ttu-id="e69c8-273">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="e69c8-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="e69c8-274">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="e69c8-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="e69c8-275">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="e69c8-275">**Type**: *int*</span></span>  
<span data-ttu-id="e69c8-276">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="e69c8-276">**Default**: 5</span></span>  
<span data-ttu-id="e69c8-277">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="e69c8-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="e69c8-278">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="e69c8-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="e69c8-279">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) accetta [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="e69c8-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="e69c8-280">Durante il periodo di timeout, l'hosting:</span><span class="sxs-lookup"><span data-stu-id="e69c8-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="e69c8-281">Attiva [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="e69c8-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="e69c8-282">Tenta di arrestare i servizi ospitati, registrando eventuali errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="e69c8-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="e69c8-283">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="e69c8-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="e69c8-284">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="e69c8-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="e69c8-285">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="e69c8-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="e69c8-286">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="e69c8-286">Startup Assembly</span></span>

<span data-ttu-id="e69c8-287">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="e69c8-288">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="e69c8-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="e69c8-289">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-289">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-290">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="e69c8-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="e69c8-291">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="e69c8-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="e69c8-292">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="e69c8-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="e69c8-293">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="e69c8-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="e69c8-294">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="e69c8-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="e69c8-295">Radice Web</span><span class="sxs-lookup"><span data-stu-id="e69c8-295">Web Root</span></span>

<span data-ttu-id="e69c8-296">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="e69c8-297">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="e69c8-297">**Key**: webroot</span></span>  
<span data-ttu-id="e69c8-298">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="e69c8-298">**Type**: *string*</span></span>  
<span data-ttu-id="e69c8-299">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="e69c8-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="e69c8-300">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="e69c8-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="e69c8-301">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="e69c8-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="e69c8-302">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="e69c8-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="e69c8-303">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="e69c8-303">Override configuration</span></span>

<span data-ttu-id="e69c8-304">Usare la [Configurazione](xref:fundamentals/configuration/index) per configurare l'host Web.</span><span class="sxs-lookup"><span data-stu-id="e69c8-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="e69c8-305">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente nel file *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e69c8-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="e69c8-306">Qualsiasi configurazione caricata dal file *hostsettings.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e69c8-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="e69c8-307">La configurazione compilata (in `config`) viene usata per configurare l'host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="e69c8-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="e69c8-308">La configurazione di `IWebHostBuilder` viene aggiunta alla configurazione dell'app, ma non il contrario: &mdash;`ConfigureAppConfiguration` non influenza la configurazione di `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="e69c8-309">Esecuzione dell'override della configurazione specificata da `UseUrls` con prima la configurazione *hostsettings.json* e quindi con la configurazione dell'argomento della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="e69c8-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="e69c8-310">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e69c8-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="e69c8-311">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="e69c8-312">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="e69c8-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="e69c8-313">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="e69c8-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="e69c8-314">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="e69c8-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="e69c8-315">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="e69c8-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="e69c8-316">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="e69c8-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="e69c8-317">`UseConfiguration` copia solo le chiavi dall'elemento `IConfiguration` specificato nella configurazione del generatore di host.</span><span class="sxs-lookup"><span data-stu-id="e69c8-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="e69c8-318">Pertanto, l'impostazione di `reloadOnChange: true` per i file JSON, INI e XML non ha alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="e69c8-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="e69c8-319">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="e69c8-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="e69c8-320">L'argomento della riga di comando esegue l'override del valore `urls` dal file *hostsettings.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="e69c8-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="e69c8-321">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="e69c8-321">Manage the host</span></span>

<span data-ttu-id="e69c8-322">**Run**</span><span class="sxs-lookup"><span data-stu-id="e69c8-322">**Run**</span></span>

<span data-ttu-id="e69c8-323">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="e69c8-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e69c8-324">**Start**</span><span class="sxs-lookup"><span data-stu-id="e69c8-324">**Start**</span></span>

<span data-ttu-id="e69c8-325">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e69c8-326">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="e69c8-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="e69c8-327">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="e69c8-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="e69c8-328">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="e69c8-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="e69c8-329">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="e69c8-330">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e69c8-331">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="e69c8-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e69c8-332">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e69c8-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e69c8-333">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e69c8-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e69c8-334">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="e69c8-335">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e69c8-336">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="e69c8-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="e69c8-338">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="e69c8-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="e69c8-339">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="e69c8-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="e69c8-340">Richiesta</span><span class="sxs-lookup"><span data-stu-id="e69c8-340">Request</span></span>                                    | <span data-ttu-id="e69c8-341">Risposta</span><span class="sxs-lookup"><span data-stu-id="e69c8-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="e69c8-342">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="e69c8-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="e69c8-343">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="e69c8-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="e69c8-344">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="e69c8-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="e69c8-345">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="e69c8-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="e69c8-346">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="e69c8-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="e69c8-347">Hello World!</span><span class="sxs-lookup"><span data-stu-id="e69c8-347">Hello World!</span></span>                             |

<span data-ttu-id="e69c8-348">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e69c8-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e69c8-349">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e69c8-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e69c8-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="e69c8-351">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="e69c8-352">Produce lo stesso risultato di **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="e69c8-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="e69c8-354">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e69c8-355">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="e69c8-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e69c8-356">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e69c8-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e69c8-357">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="e69c8-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e69c8-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="e69c8-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="e69c8-359">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e69c8-360">Produce lo stesso risultato di **StartWith(Action&lt;IApplicationBuilder&gt; app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="e69c8-361">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="e69c8-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="e69c8-362">L'[interfaccia IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="e69c8-363">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="e69c8-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="e69c8-364">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#environment-based-startup-class-and-methods) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="e69c8-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="e69c8-365">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e69c8-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="e69c8-366">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="e69c8-367">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e69c8-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="e69c8-368">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="e69c8-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="e69c8-369">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/write) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="e69c8-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="e69c8-370">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e69c8-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="e69c8-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="e69c8-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="e69c8-372">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="e69c8-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="e69c8-373">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="e69c8-373">Cancellation Token</span></span>    | <span data-ttu-id="e69c8-374">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="e69c8-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="e69c8-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="e69c8-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="e69c8-376">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="e69c8-376">The host has fully started.</span></span> |
| [<span data-ttu-id="e69c8-377">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="e69c8-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="e69c8-378">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="e69c8-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="e69c8-379">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="e69c8-379">All requests should be processed.</span></span> <span data-ttu-id="e69c8-380">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="e69c8-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="e69c8-381">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="e69c8-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="e69c8-382">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="e69c8-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="e69c8-383">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="e69c8-383">Requests may still be processing.</span></span> <span data-ttu-id="e69c8-384">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="e69c8-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="e69c8-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="e69c8-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="e69c8-386">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="e69c8-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="e69c8-387">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="e69c8-387">Scope validation</span></span>

<span data-ttu-id="e69c8-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) su `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e69c8-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="e69c8-389">Quando `ValidateScopes` è impostato su `true`, il provider di servizi predefinito esegue dei controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="e69c8-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="e69c8-390">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="e69c8-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="e69c8-391">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="e69c8-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="e69c8-392">Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="e69c8-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="e69c8-393">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="e69c8-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="e69c8-394">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="e69c8-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="e69c8-395">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="e69c8-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="e69c8-396">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="e69c8-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="e69c8-397">Per convalidare sempre gli ambiti, inclusi quelli dell'ambiente di produzione, configurare [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) nel generatore di host:</span><span class="sxs-lookup"><span data-stu-id="e69c8-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="e69c8-398">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e69c8-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
