---
title: Host Web ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: bc77413127273aba207e68e7fbcb8ad916267e8e
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862278"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="fdce1-103">Host Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdce1-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="fdce1-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fdce1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="fdce1-105">Per la versione 1.1 di questo argomento, scaricare [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) (Host Web ASP.NET Core - Versione 1.1, PDF).</span><span class="sxs-lookup"><span data-stu-id="fdce1-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="fdce1-106">Le app ASP.NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="fdce1-107">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="fdce1-108">L'host configura almeno un server e una pipeline di elaborazione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="fdce1-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="fdce1-109">Questo argomento tratta l'host Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="fdce1-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="fdce1-110">Per informazioni sull'host generico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="fdce1-111">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="fdce1-111">Set up a host</span></span>

<span data-ttu-id="fdce1-112">Creare un host usando un'istanza di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="fdce1-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="fdce1-113">Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="fdce1-114">Nei modelli di progetto `Main` si trova in *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="fdce1-115">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="fdce1-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="fdce1-116">`CreateDefaultBuilder` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="fdce1-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="fdce1-117">Configura il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web usando i provider di configurazione dell'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="fdce1-118">Per le opzioni predefinite del server Kestrel, vedere <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-118">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="fdce1-119">Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="fdce1-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="fdce1-120">Carica la [configurazione dell'host](#host-configuration-values) da:</span><span class="sxs-lookup"><span data-stu-id="fdce1-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="fdce1-121">Le variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="fdce1-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="fdce1-122">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdce1-122">Command-line arguments.</span></span>
* <span data-ttu-id="fdce1-123">Carica la configurazione dell'app nell'ordine seguente da:</span><span class="sxs-lookup"><span data-stu-id="fdce1-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="fdce1-124">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="fdce1-125">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="fdce1-126">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="fdce1-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="fdce1-127">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fdce1-127">Environment variables.</span></span>
  * <span data-ttu-id="fdce1-128">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdce1-128">Command-line arguments.</span></span>
* <span data-ttu-id="fdce1-129">Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug.</span><span class="sxs-lookup"><span data-stu-id="fdce1-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="fdce1-130">La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="fdce1-131">Se eseguito in IIS con il [Modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), `CreateDefaultBuilder` abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index), che configura l'indirizzo di base e la porta dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-131">When running behind IIS with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="fdce1-132">L'integrazione di IIS configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="fdce1-132">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="fdce1-133">Per le opzioni predefinite di IIS, vedere <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-133">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="fdce1-134">Imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="fdce1-134">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="fdce1-135">Per ulteriori informazioni, vedere [Convalida dell'ambito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="fdce1-135">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="fdce1-136">La configurazione definita da `CreateDefaultBuilder` può essere sottoposta a override e aumentata tramite [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e altri metodi e metodi di estensione di [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="fdce1-136">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="fdce1-137">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="fdce1-137">A few examples follow:</span></span>

* <span data-ttu-id="fdce1-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) viene usato per specificare altri elementi `IConfiguration` per l'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="fdce1-139">La chiamata a `ConfigureAppConfiguration` seguente consente di aggiungere un delegato per includere la configurazione dell'app nel file *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-139">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="fdce1-140">`ConfigureAppConfiguration` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="fdce1-140">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="fdce1-141">Si noti che questa configurazione non è valida per l'host (ad esempio, gli URL del server o l'ambiente).</span><span class="sxs-lookup"><span data-stu-id="fdce1-141">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="fdce1-142">Vedere la sezione [Valori di configurazione dell'host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="fdce1-142">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="fdce1-143">La chiamata a `ConfigureLogging` seguente consente di aggiungere un delegato per configurare il livello di registrazione minimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="fdce1-143">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="fdce1-144">Questa impostazione sostituisce le impostazioni in *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configurate da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-144">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="fdce1-145">`ConfigureLogging` può essere chiamato più volte.</span><span class="sxs-lookup"><span data-stu-id="fdce1-145">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="fdce1-146">La chiamata a `ConfigureKestrel` seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-146">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="fdce1-147">La chiamata a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) seguente sostituisce il valore predefinito [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) di 30.000.000 di byte stabilito durante la configurazione del Kestrel da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="fdce1-148">La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="fdce1-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="fdce1-149">Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="fdce1-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="fdce1-150">Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="fdce1-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="fdce1-151">Per altre informazioni sulla configurazione dell'app, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="fdce1-152">In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="fdce1-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="fdce1-153">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fdce1-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="fdce1-154">Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="fdce1-154">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="fdce1-155">Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-155">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="fdce1-156">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-156">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="fdce1-157">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="fdce1-157">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="fdce1-158">Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="fdce1-158">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="fdce1-159">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="fdce1-159">Host configuration values</span></span>

<span data-ttu-id="fdce1-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="fdce1-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="fdce1-161">Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-161">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="fdce1-162">Ad esempio `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-162">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="fdce1-163">Le estensioni come [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vedere la sezione [Override della configurazione](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="fdce1-163">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="fdce1-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata.</span><span class="sxs-lookup"><span data-stu-id="fdce1-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="fdce1-165">Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.</span><span class="sxs-lookup"><span data-stu-id="fdce1-165">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="fdce1-166">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="fdce1-166">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="fdce1-167">Per altre informazioni, vedere [Override della configurazione](#override-configuration) nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="fdce1-167">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="fdce1-168">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="fdce1-168">Application Key (Name)</span></span>

<span data-ttu-id="fdce1-169">La proprietà [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) viene impostata automaticamente quando si chiama [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="fdce1-169">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="fdce1-170">Il valore viene impostato sul nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-170">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="fdce1-171">Per impostare il valore in modo esplicito, usare [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="fdce1-171">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="fdce1-172">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="fdce1-172">**Key**: applicationName</span></span>  
<span data-ttu-id="fdce1-173">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-173">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-174">**Impostazione predefinita**: il nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-174">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="fdce1-175">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fdce1-175">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fdce1-176">**Variabile di ambiente**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="fdce1-176">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="fdce1-177">Acquisizione degli errori di avvio</span><span class="sxs-lookup"><span data-stu-id="fdce1-177">Capture Startup Errors</span></span>

<span data-ttu-id="fdce1-178">Questa impostazione controlla l'acquisizione degli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="fdce1-178">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="fdce1-179">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="fdce1-179">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="fdce1-180">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="fdce1-180">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fdce1-181">**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-181">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="fdce1-182">**Impostare usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="fdce1-182">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="fdce1-183">**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="fdce1-183">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="fdce1-184">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="fdce1-184">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="fdce1-185">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="fdce1-185">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="fdce1-186">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="fdce1-186">Content Root</span></span>

<span data-ttu-id="fdce1-187">Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="fdce1-187">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="fdce1-188">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="fdce1-188">**Key**: contentRoot</span></span>  
<span data-ttu-id="fdce1-189">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-189">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-190">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-190">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="fdce1-191">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="fdce1-191">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="fdce1-192">**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="fdce1-192">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="fdce1-193">La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="fdce1-193">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="fdce1-194">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="fdce1-194">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="fdce1-195">Errori dettagliati</span><span class="sxs-lookup"><span data-stu-id="fdce1-195">Detailed Errors</span></span>

<span data-ttu-id="fdce1-196">Determina l'acquisizione degli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="fdce1-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="fdce1-197">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="fdce1-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="fdce1-198">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="fdce1-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fdce1-199">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="fdce1-199">**Default**: false</span></span>  
<span data-ttu-id="fdce1-200">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fdce1-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fdce1-201">**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="fdce1-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="fdce1-202">Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="fdce1-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="fdce1-203">Ambiente</span><span class="sxs-lookup"><span data-stu-id="fdce1-203">Environment</span></span>

<span data-ttu-id="fdce1-204">Imposta l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-204">Sets the app's environment.</span></span>

<span data-ttu-id="fdce1-205">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="fdce1-205">**Key**: environment</span></span>  
<span data-ttu-id="fdce1-206">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-206">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-207">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="fdce1-207">**Default**: Production</span></span>  
<span data-ttu-id="fdce1-208">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="fdce1-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="fdce1-209">**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="fdce1-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="fdce1-210">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="fdce1-210">The environment can be set to any value.</span></span> <span data-ttu-id="fdce1-211">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="fdce1-212">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fdce1-212">Values aren't case sensitive.</span></span> <span data-ttu-id="fdce1-213">Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="fdce1-214">Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="fdce1-215">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-215">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="fdce1-216">Assembly di avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="fdce1-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="fdce1-217">Imposta gli assembly di avvio dell'hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="fdce1-218">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="fdce1-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="fdce1-219">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-219">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-220">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="fdce1-220">**Default**: Empty string</span></span>  
<span data-ttu-id="fdce1-221">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fdce1-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fdce1-222">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="fdce1-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="fdce1-223">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="fdce1-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="fdce1-224">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-224">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="fdce1-225">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="fdce1-225">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="fdce1-226">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="fdce1-226">HTTPS Port</span></span>

<span data-ttu-id="fdce1-227">Impostare la porta di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fdce1-227">Set the HTTPS redirect port.</span></span> <span data-ttu-id="fdce1-228">Usata per [imporre HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="fdce1-228">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fdce1-229">**Chiave**: https_port **Tipo**: *string*
**Valore predefinito**: non è impostato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="fdce1-229">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="fdce1-230">**Impostazione con**: `UseSetting`
**Variabile di ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="fdce1-230">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="fdce1-231">Assembly di avvio dell'hosting da escludere</span><span class="sxs-lookup"><span data-stu-id="fdce1-231">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="fdce1-232">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da escludere all'avvio.</span><span class="sxs-lookup"><span data-stu-id="fdce1-232">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="fdce1-233">**Chiave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="fdce1-233">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="fdce1-234">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-234">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-235">**Impostazione predefinita**: stringa vuota</span><span class="sxs-lookup"><span data-stu-id="fdce1-235">**Default**: Empty string</span></span>  
<span data-ttu-id="fdce1-236">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fdce1-236">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fdce1-237">**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="fdce1-237">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="fdce1-238">Preferire gli URL di hosting</span><span class="sxs-lookup"><span data-stu-id="fdce1-238">Prefer Hosting URLs</span></span>

<span data-ttu-id="fdce1-239">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-239">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="fdce1-240">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="fdce1-240">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="fdce1-241">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="fdce1-241">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fdce1-242">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="fdce1-242">**Default**: true</span></span>  
<span data-ttu-id="fdce1-243">**Impostare usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="fdce1-243">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="fdce1-244">**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="fdce1-244">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="fdce1-245">Impedire l'avvio dell'hosting</span><span class="sxs-lookup"><span data-stu-id="fdce1-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="fdce1-246">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="fdce1-247">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-247">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="fdce1-248">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="fdce1-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="fdce1-249">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="fdce1-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fdce1-250">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="fdce1-250">**Default**: false</span></span>  
<span data-ttu-id="fdce1-251">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fdce1-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fdce1-252">**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="fdce1-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="fdce1-253">URL del server</span><span class="sxs-lookup"><span data-stu-id="fdce1-253">Server URLs</span></span>

<span data-ttu-id="fdce1-254">Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="fdce1-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="fdce1-255">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="fdce1-255">**Key**: urls</span></span>  
<span data-ttu-id="fdce1-256">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-256">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-257">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="fdce1-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="fdce1-258">**Impostare usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="fdce1-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="fdce1-259">**Variabile di ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fdce1-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="fdce1-260">Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="fdce1-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="fdce1-261">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="fdce1-262">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="fdce1-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="fdce1-263">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="fdce1-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="fdce1-264">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="fdce1-264">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="fdce1-265">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="fdce1-265">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="fdce1-266">Per ulteriori informazioni, vedere <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-266">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="fdce1-267">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="fdce1-267">Shutdown Timeout</span></span>

<span data-ttu-id="fdce1-268">Specifica il tempo di attesa per l'arresto dell'host Web.</span><span class="sxs-lookup"><span data-stu-id="fdce1-268">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="fdce1-269">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="fdce1-269">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="fdce1-270">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="fdce1-270">**Type**: *int*</span></span>  
<span data-ttu-id="fdce1-271">**Impostazione predefinita**: 5</span><span class="sxs-lookup"><span data-stu-id="fdce1-271">**Default**: 5</span></span>  
<span data-ttu-id="fdce1-272">**Impostare usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="fdce1-272">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="fdce1-273">**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="fdce1-273">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="fdce1-274">Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) accetta [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="fdce1-274">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="fdce1-275">Durante il periodo di timeout, l'hosting:</span><span class="sxs-lookup"><span data-stu-id="fdce1-275">During the timeout period, hosting:</span></span>

* <span data-ttu-id="fdce1-276">Attiva [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="fdce1-276">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="fdce1-277">Tenta di arrestare i servizi ospitati, registrando eventuali errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="fdce1-277">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="fdce1-278">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="fdce1-278">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="fdce1-279">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="fdce1-279">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="fdce1-280">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="fdce1-280">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="fdce1-281">Assembly di avvio</span><span class="sxs-lookup"><span data-stu-id="fdce1-281">Startup Assembly</span></span>

<span data-ttu-id="fdce1-282">Determina l'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="fdce1-283">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="fdce1-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="fdce1-284">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-284">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-285">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="fdce1-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="fdce1-286">**Impostare usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="fdce1-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="fdce1-287">**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="fdce1-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="fdce1-288">È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="fdce1-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="fdce1-289">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="fdce1-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="fdce1-290">Radice Web</span><span class="sxs-lookup"><span data-stu-id="fdce1-290">Web Root</span></span>

<span data-ttu-id="fdce1-291">Imposta il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-291">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="fdce1-292">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="fdce1-292">**Key**: webroot</span></span>  
<span data-ttu-id="fdce1-293">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="fdce1-293">**Type**: *string*</span></span>  
<span data-ttu-id="fdce1-294">**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste.</span><span class="sxs-lookup"><span data-stu-id="fdce1-294">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="fdce1-295">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="fdce1-295">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="fdce1-296">**Impostare usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="fdce1-296">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="fdce1-297">**Variabile di ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="fdce1-297">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="fdce1-298">Override della configurazione</span><span class="sxs-lookup"><span data-stu-id="fdce1-298">Override configuration</span></span>

<span data-ttu-id="fdce1-299">Usare la [Configurazione](xref:fundamentals/configuration/index) per configurare l'host Web.</span><span class="sxs-lookup"><span data-stu-id="fdce1-299">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="fdce1-300">Nell'esempio seguente la configurazione dell'host è specificata facoltativamente nel file *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fdce1-300">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="fdce1-301">Qualsiasi configurazione caricata dal file *hostsettings.json* può essere sostituita dagli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdce1-301">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="fdce1-302">La configurazione compilata (in `config`) viene usata per configurare l'host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="fdce1-302">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="fdce1-303">La configurazione di `IWebHostBuilder` viene aggiunta alla configurazione dell'app, ma non il contrario: &mdash;`ConfigureAppConfiguration` non influenza la configurazione di `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-303">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="fdce1-304">Esecuzione dell'override della configurazione specificata da `UseUrls` con prima la configurazione *hostsettings.json* e quindi con la configurazione dell'argomento della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="fdce1-304">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="fdce1-305">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fdce1-305">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="fdce1-306">Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-306">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="fdce1-307">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="fdce1-307">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="fdce1-308">Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="fdce1-308">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="fdce1-309">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="fdce1-309">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="fdce1-310">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="fdce1-310">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="fdce1-311">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="fdce1-311">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="fdce1-312">`UseConfiguration` copia solo le chiavi dall'elemento `IConfiguration` specificato nella configurazione del generatore di host.</span><span class="sxs-lookup"><span data-stu-id="fdce1-312">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="fdce1-313">Pertanto, l'impostazione di `reloadOnChange: true` per i file JSON, INI e XML non ha alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="fdce1-313">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="fdce1-314">Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="fdce1-314">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="fdce1-315">L'argomento della riga di comando esegue l'override del valore `urls` dal file *hostsettings.json*, mentre il server esegue l'ascolto sulla porta 8080:</span><span class="sxs-lookup"><span data-stu-id="fdce1-315">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="fdce1-316">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="fdce1-316">Manage the host</span></span>

<span data-ttu-id="fdce1-317">**Run**</span><span class="sxs-lookup"><span data-stu-id="fdce1-317">**Run**</span></span>

<span data-ttu-id="fdce1-318">Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="fdce1-318">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="fdce1-319">**Start**</span><span class="sxs-lookup"><span data-stu-id="fdce1-319">**Start**</span></span>

<span data-ttu-id="fdce1-320">Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-320">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="fdce1-321">Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:</span><span class="sxs-lookup"><span data-stu-id="fdce1-321">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="fdce1-322">L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico.</span><span class="sxs-lookup"><span data-stu-id="fdce1-322">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="fdce1-323">Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="fdce1-323">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="fdce1-324">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-324">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="fdce1-325">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-325">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fdce1-326">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="fdce1-326">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fdce1-327">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="fdce1-327">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fdce1-328">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="fdce1-328">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fdce1-329">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-329">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="fdce1-330">Start con un URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-330">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fdce1-331">Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-331">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="fdce1-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fdce1-333">Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:</span><span class="sxs-lookup"><span data-stu-id="fdce1-333">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="fdce1-334">Usare le richieste del browser seguenti con l'esempio:</span><span class="sxs-lookup"><span data-stu-id="fdce1-334">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="fdce1-335">Richiesta</span><span class="sxs-lookup"><span data-stu-id="fdce1-335">Request</span></span>                                    | <span data-ttu-id="fdce1-336">Risposta</span><span class="sxs-lookup"><span data-stu-id="fdce1-336">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="fdce1-337">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="fdce1-337">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="fdce1-338">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="fdce1-338">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="fdce1-339">Genera un'eccezione con la stringa "ooops!"</span><span class="sxs-lookup"><span data-stu-id="fdce1-339">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="fdce1-340">Genera un'eccezione con la stringa "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="fdce1-340">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="fdce1-341">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="fdce1-341">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="fdce1-342">Hello World!</span><span class="sxs-lookup"><span data-stu-id="fdce1-342">Hello World!</span></span>                             |

<span data-ttu-id="fdce1-343">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="fdce1-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fdce1-344">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="fdce1-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fdce1-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fdce1-346">Usare un URL e un'istanza di `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-346">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="fdce1-347">Produce lo stesso risultato di **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-347">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="fdce1-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fdce1-349">Specificare un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-349">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="fdce1-350">Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="fdce1-350">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fdce1-351">`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="fdce1-351">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fdce1-352">L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.</span><span class="sxs-lookup"><span data-stu-id="fdce1-352">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fdce1-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fdce1-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fdce1-354">Specificare un URL e un delegato per configurare `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-354">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="fdce1-355">Produce lo stesso risultato di **StartWith(Action&lt;IApplicationBuilder&gt; app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-355">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="fdce1-356">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="fdce1-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="fdce1-357">L'[interfaccia IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-357">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="fdce1-358">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="fdce1-358">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="fdce1-359">È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#environment-based-startup-class-and-methods) per configurare l'app all'avvio in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="fdce1-359">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="fdce1-360">In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fdce1-360">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="fdce1-361">Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="fdce1-362">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fdce1-362">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="fdce1-363">Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="fdce1-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="fdce1-364">`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware/index#write-middleware) personalizzato:</span><span class="sxs-lookup"><span data-stu-id="fdce1-364">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="fdce1-365">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="fdce1-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="fdce1-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="fdce1-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="fdce1-367">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="fdce1-367">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="fdce1-368">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="fdce1-368">Cancellation Token</span></span>    | <span data-ttu-id="fdce1-369">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="fdce1-369">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="fdce1-370">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="fdce1-370">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="fdce1-371">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="fdce1-371">The host has fully started.</span></span> |
| [<span data-ttu-id="fdce1-372">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="fdce1-372">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="fdce1-373">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="fdce1-373">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="fdce1-374">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="fdce1-374">All requests should be processed.</span></span> <span data-ttu-id="fdce1-375">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="fdce1-375">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="fdce1-376">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="fdce1-376">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="fdce1-377">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="fdce1-377">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="fdce1-378">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fdce1-378">Requests may still be processing.</span></span> <span data-ttu-id="fdce1-379">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="fdce1-379">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="fdce1-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="fdce1-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="fdce1-381">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="fdce1-381">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="fdce1-382">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="fdce1-382">Scope validation</span></span>

<span data-ttu-id="fdce1-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) imposta [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) su `true` se l'ambiente dell'app è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="fdce1-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="fdce1-384">Quando `ValidateScopes` è impostato su `true`, il provider di servizi predefinito esegue dei controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="fdce1-384">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="fdce1-385">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="fdce1-385">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="fdce1-386">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="fdce1-386">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="fdce1-387">Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="fdce1-387">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="fdce1-388">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="fdce1-388">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="fdce1-389">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="fdce1-389">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="fdce1-390">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="fdce1-390">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="fdce1-391">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="fdce1-391">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="fdce1-392">Per convalidare sempre gli ambiti, inclusi quelli dell'ambiente di produzione, configurare [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) nel generatore di host:</span><span class="sxs-lookup"><span data-stu-id="fdce1-392">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="fdce1-393">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fdce1-393">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
