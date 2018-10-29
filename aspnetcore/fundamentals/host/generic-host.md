---
title: Host generico .NET
author: guardrex
description: Informazioni sull'host generico in .NET, che gestisce l'avvio e la durata delle app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0924e2764958911dc1711d5427f6dd58e8873739
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477605"
---
# <a name="net-generic-host"></a><span data-ttu-id="4499e-103">Host generico .NET</span><span class="sxs-lookup"><span data-stu-id="4499e-103">.NET Generic Host</span></span>

<span data-ttu-id="4499e-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4499e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4499e-105">Le app .NET Core configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="4499e-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="4499e-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="4499e-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4499e-107">Questo argomento illustra l'host generico di ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile per l'hosting di app che non elaborano le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="4499e-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="4499e-108">Per informazioni sull'host Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="4499e-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="4499e-109">L'obiettivo dell'host generico è separare la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host.</span><span class="sxs-lookup"><span data-stu-id="4499e-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="4499e-110">La messaggistica, le attività in background e altri carichi di lavoro non HTTP basati sull'host generico traggono vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="4499e-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="4499e-111">L'host generico è una nuova funzionalità introdotta in ASP.NET Core 2.1 e non è adatto agli scenari di hosting Web.</span><span class="sxs-lookup"><span data-stu-id="4499e-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="4499e-112">Per gli scenari di hosting Web usare l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="4499e-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="4499e-113">L'host generico è in fase di sviluppo e sostituirà l'host Web in una versione futura, funzionando come API host principale negli scenari sia HTTP che non HTTP.</span><span class="sxs-lookup"><span data-stu-id="4499e-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="4499e-114">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4499e-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4499e-115">Quando si esegue l'app di esempio in [Visual Studio Code](https://code.visualstudio.com/), usare un *terminale esterno o integrato*.</span><span class="sxs-lookup"><span data-stu-id="4499e-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="4499e-116">Non eseguire l'esempio in un ambiente `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="4499e-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="4499e-117">Per impostare la console in Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="4499e-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="4499e-118">Aprire il file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="4499e-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="4499e-119">Nella configurazione **.NET Core Launch (console)** trovare la voce **console**.</span><span class="sxs-lookup"><span data-stu-id="4499e-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="4499e-120">Impostare il valore su `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="4499e-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="4499e-121">Introduzione</span><span class="sxs-lookup"><span data-stu-id="4499e-121">Introduction</span></span>

<span data-ttu-id="4499e-122">La libreria host generico è disponibile nello [spazio dei nomi Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e viene specificata dal pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="4499e-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="4499e-123">Il pacchetto `Microsoft.Extensions.Hosting` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="4499e-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="4499e-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) è il punto di ingresso per l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="4499e-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="4499e-125">Ogni implementazione `IHostedService` viene eseguita nell'ordine di [registrazione del servizio in ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="4499e-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="4499e-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) viene chiamato su ogni `IHostedService` quando viene avviato l'host e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) viene chiamato in ordine di registrazione inverso quando l'host viene chiuso normalmente.</span><span class="sxs-lookup"><span data-stu-id="4499e-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="4499e-127">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="4499e-127">Set up a host</span></span>

<span data-ttu-id="4499e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) è il componente principale che le librerie e le app usano per inizializzare, compilare ed eseguire l'host:</span><span class="sxs-lookup"><span data-stu-id="4499e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="4499e-129">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="4499e-129">Default services</span></span>

<span data-ttu-id="4499e-130">I servizi seguenti vengono registrati durante l'inizializzazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="4499e-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="4499e-131">[Ambiente](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="4499e-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="4499e-132">[Configurazione](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="4499e-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="4499e-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="4499e-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="4499e-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="4499e-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="4499e-135">[Opzioni](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="4499e-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="4499e-136">[Registrazione](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="4499e-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="4499e-137">Configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="4499e-137">Host configuration</span></span>

<span data-ttu-id="4499e-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="4499e-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="4499e-139">Generatore di configurazioni</span><span class="sxs-lookup"><span data-stu-id="4499e-139">Configuration builder</span></span>
* <span data-ttu-id="4499e-140">Configurazione del metodo di estensione</span><span class="sxs-lookup"><span data-stu-id="4499e-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="4499e-141">Generatore di configurazioni</span><span class="sxs-lookup"><span data-stu-id="4499e-141">Configuration builder</span></span>

<span data-ttu-id="4499e-142">La configurazione del generatore host viene creata chiamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4499e-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="4499e-143">`ConfigureHostConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'host.</span><span class="sxs-lookup"><span data-stu-id="4499e-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="4499e-144">Il generatore di configurazioni inizializza [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) per l'uso nel processo di compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="4499e-145">La configurazione della variabile di ambiente non viene aggiunta per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4499e-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="4499e-146">Chiamare [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel generatore di host per configurare l'host dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4499e-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="4499e-147">`AddEnvironmentVariables` accetta un prefisso facoltativo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="4499e-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="4499e-148">L'app di esempio usa un prefisso di `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="4499e-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="4499e-149">Il prefisso viene rimosso quando vengono lette le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4499e-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="4499e-150">Quando viene configurato l'host dell'app di esempio, il valore della variabile di ambiente per `PREFIX_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.</span><span class="sxs-lookup"><span data-stu-id="4499e-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="4499e-151">Durante lo sviluppo quando si usa [Visual Studio](https://www.visualstudio.com/) o si esegue un'app con `dotnet run`, le variabili di ambiente possono essere impostate nel file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4499e-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="4499e-152">Durante lo sviluppo in [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="4499e-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="4499e-153">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4499e-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="4499e-154">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="4499e-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="4499e-155">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="4499e-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="4499e-156">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="4499e-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="4499e-157">Configurazione di esempio di `HostBuilder` che usa `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="4499e-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="4499e-158">Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="4499e-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4499e-159">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="4499e-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="4499e-160">Il metodo `AddConfiguration` prevede che le chiavi corrispondano alle chiavi `HostBuilder` (ad esempio `environment`).</span><span class="sxs-lookup"><span data-stu-id="4499e-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="4499e-161">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="4499e-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="4499e-162">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="4499e-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4499e-163">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4499e-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="4499e-164">Configurazione del metodo di estensione</span><span class="sxs-lookup"><span data-stu-id="4499e-164">Extension method configuration</span></span>

<span data-ttu-id="4499e-165">I metodi di estensione vengono chiamati nell'implementazione `IHostBuilder` per configurare la radice del contenuto e l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="4499e-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="4499e-166">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="4499e-166">Application Key (Name)</span></span>

<span data-ttu-id="4499e-167">La proprietà [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) viene impostata dalla configurazione dell'host durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="4499e-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="4499e-168">Per impostare il valore in modo esplicito, usare [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="4499e-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="4499e-169">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="4499e-169">**Key**: applicationName</span></span>  
<span data-ttu-id="4499e-170">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="4499e-170">**Type**: *string*</span></span>  
<span data-ttu-id="4499e-171">**Impostazione predefinita**: il nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="4499e-172">**Impostare usando**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="4499e-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="4499e-173">**Variabile di ambiente**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="4499e-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="4499e-174">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="4499e-174">Content Root</span></span>

<span data-ttu-id="4499e-175">Questa impostazione determina la posizione da cui l'host inizia la ricerca dei file di contenuto.</span><span class="sxs-lookup"><span data-stu-id="4499e-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="4499e-176">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="4499e-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="4499e-177">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="4499e-177">**Type**: *string*</span></span>  
<span data-ttu-id="4499e-178">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="4499e-179">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="4499e-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="4499e-180">**Variabile di ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="4499e-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="4499e-181">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="4499e-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="4499e-182">Ambiente</span><span class="sxs-lookup"><span data-stu-id="4499e-182">Environment</span></span>

<span data-ttu-id="4499e-183">Imposta l'[ambiente](xref:fundamentals/environments) per l'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="4499e-184">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="4499e-184">**Key**: environment</span></span>  
<span data-ttu-id="4499e-185">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="4499e-185">**Type**: *string*</span></span>  
<span data-ttu-id="4499e-186">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="4499e-186">**Default**: Production</span></span>  
<span data-ttu-id="4499e-187">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="4499e-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="4499e-188">**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="4499e-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="4499e-189">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="4499e-189">The environment can be set to any value.</span></span> <span data-ttu-id="4499e-190">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="4499e-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="4499e-191">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4499e-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="4499e-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="4499e-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="4499e-193">La configurazione del generatore app viene creata chiamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4499e-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="4499e-194">`ConfigureAppConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="4499e-195">`ConfigureAppConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="4499e-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="4499e-196">L'app usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="4499e-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="4499e-197">La configurazione creata da `ConfigureAppConfiguration` è disponibile in [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) per le operazioni successive e in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="4499e-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="4499e-198">Configurazione app di esempio che usa `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="4499e-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="4499e-199">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="4499e-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="4499e-200">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="4499e-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="4499e-201">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="4499e-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="4499e-202">Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="4499e-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4499e-203">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="4499e-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="4499e-204">Il metodo `AddConfiguration` prevede una corrispondenza esatta per le chiavi di configurazione (ad esempio `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="4499e-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="4499e-205">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="4499e-206">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="4499e-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4499e-207">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4499e-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="4499e-208">Per spostare i file delle impostazioni nella directory di output, specificare i file delle impostazioni come [elementi di progetto MSBuild](/visualstudio/msbuild/common-msbuild-project-items) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="4499e-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="4499e-209">L'app di esempio sposta i file delle impostazioni dell'app JSON e *hostsettings.json* con l'elemento **&lt;Content:&gt;** seguente:</span><span class="sxs-lookup"><span data-stu-id="4499e-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="4499e-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="4499e-210">ConfigureServices</span></span>

<span data-ttu-id="4499e-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) aggiunge servizi al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4499e-212">`ConfigureServices` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="4499e-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="4499e-213">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="4499e-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="4499e-214">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="4499e-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="4499e-215">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa il metodo di estensione `AddHostedService` per aggiungere all'app un servizio per gli eventi di durata (`LifetimeEventsHostedService`) e un'attività in background con timeout (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="4499e-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="4499e-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="4499e-216">ConfigureLogging</span></span>

<span data-ttu-id="4499e-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) aggiunge un delegato per la configurazione dell'interfaccia [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) specificata.</span><span class="sxs-lookup"><span data-stu-id="4499e-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="4499e-218">`ConfigureLogging` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="4499e-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="4499e-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="4499e-219">UseConsoleLifetime</span></span>

<span data-ttu-id="4499e-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM e chiama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) per avviare il processo di arresto.</span><span class="sxs-lookup"><span data-stu-id="4499e-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="4499e-221">`UseConsoleLifetime` sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="4499e-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="4499e-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) è preregistrata come implementazione di durata predefinita.</span><span class="sxs-lookup"><span data-stu-id="4499e-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="4499e-223">Viene usata l'ultima durata registrata.</span><span class="sxs-lookup"><span data-stu-id="4499e-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="4499e-224">Configurazione contenitore</span><span class="sxs-lookup"><span data-stu-id="4499e-224">Container configuration</span></span>

<span data-ttu-id="4499e-225">Per supportare l'inserimento di altri contenitori, l'host può accettare un elemento [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="4499e-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="4499e-226">La disponibilità di una factory non fa parte della registrazione del contenitore DI, ma è una funzionalità intrinseca dell'host usata per creare il contenitore DI concreto.</span><span class="sxs-lookup"><span data-stu-id="4499e-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="4499e-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) esegue l'override della factory predefinita usata per la creazione del provider di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="4499e-228">La configurazione del contenitore personalizzato è gestita dal metodo [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="4499e-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="4499e-229">`ConfigureContainer` offre un'esperienza fortemente tipizzata per la configurazione del contenitore sulla base dell'API host sottostante.</span><span class="sxs-lookup"><span data-stu-id="4499e-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="4499e-230">`ConfigureContainer` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="4499e-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="4499e-231">Creare un contenitore di servizi per l'app:</span><span class="sxs-lookup"><span data-stu-id="4499e-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="4499e-232">Specificare una factory per il contenitore di servizi:</span><span class="sxs-lookup"><span data-stu-id="4499e-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="4499e-233">Usare la factory e configurare il contenitore di servizi personalizzato per l'app:</span><span class="sxs-lookup"><span data-stu-id="4499e-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="4499e-234">Estendibilità</span><span class="sxs-lookup"><span data-stu-id="4499e-234">Extensibility</span></span>

<span data-ttu-id="4499e-235">L'estensibilità host viene eseguita con i metodi di estensione su `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4499e-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="4499e-236">L'esempio seguente illustra come un metodo di estensione estende un'implementazione `IHostBuilder` con l'esempio [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) dimostrato in <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="4499e-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="4499e-237">Un'app stabilisce il metodo di estensione `UseHostedService` per registrare il servizio ospitato passato in `T`:</span><span class="sxs-lookup"><span data-stu-id="4499e-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="4499e-238">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="4499e-238">Manage the host</span></span>

<span data-ttu-id="4499e-239">L'implementazione [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) è responsabile dell'avvio e arresto delle implementazioni `IHostedService` che sono registrate nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="4499e-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="4499e-240">Esegui</span><span class="sxs-lookup"><span data-stu-id="4499e-240">Run</span></span>

<span data-ttu-id="4499e-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) avvia l'app e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="4499e-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="4499e-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="4499e-242">RunAsync</span></span>

<span data-ttu-id="4499e-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) esegue l'app e restituisce un elemento `Task` che viene completato quando viene attivato il token di annullamento o l'arresto:</span><span class="sxs-lookup"><span data-stu-id="4499e-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="4499e-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="4499e-244">RunConsoleAsync</span></span>

<span data-ttu-id="4499e-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) abilita il supporto della console, compila e avvia l'host e resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM per eseguire l'arresto.</span><span class="sxs-lookup"><span data-stu-id="4499e-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="4499e-246">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="4499e-246">Start and StopAsync</span></span>

<span data-ttu-id="4499e-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) avvia l'host in modo sincrono.</span><span class="sxs-lookup"><span data-stu-id="4499e-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="4499e-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) prova ad arrestare l'host entro il timeout specificato.</span><span class="sxs-lookup"><span data-stu-id="4499e-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="4499e-249">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="4499e-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="4499e-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="4499e-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arresta l'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="4499e-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="4499e-252">WaitForShutdown</span></span>

<span data-ttu-id="4499e-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) viene attivato tramite [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), ad esempio [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (in ascolto di `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4499e-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="4499e-254">`WaitForShutdown` chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="4499e-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="4499e-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="4499e-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="4499e-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) restituisce un elemento `Task` che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="4499e-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="4499e-257">Controllo esterno</span><span class="sxs-lookup"><span data-stu-id="4499e-257">External control</span></span>

<span data-ttu-id="4499e-258">Il controllo esterno dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:</span><span class="sxs-lookup"><span data-stu-id="4499e-258">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="4499e-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) viene chiamato all'inizio di [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), che attende il completamento prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="4499e-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="4499e-260">Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.</span><span class="sxs-lookup"><span data-stu-id="4499e-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="4499e-261">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="4499e-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="4499e-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) include informazioni sull'ambiente di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="4499e-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="4499e-263">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="4499e-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="4499e-264">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4499e-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="4499e-265">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="4499e-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="4499e-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) consente attività post-avvio e di arresto, incluse le richieste di arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4499e-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="4499e-267">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="4499e-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="4499e-268">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="4499e-268">Cancellation Token</span></span> | <span data-ttu-id="4499e-269">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="4499e-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="4499e-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="4499e-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="4499e-271">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="4499e-271">The host has fully started.</span></span> |
| [<span data-ttu-id="4499e-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="4499e-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="4499e-273">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4499e-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="4499e-274">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="4499e-274">All requests should be processed.</span></span> <span data-ttu-id="4499e-275">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="4499e-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="4499e-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="4499e-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="4499e-277">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4499e-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="4499e-278">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4499e-278">Requests may still be processing.</span></span> <span data-ttu-id="4499e-279">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="4499e-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="4499e-280">Inserire tramite costruttore il servizio `IApplicationLifetime` in qualsiasi classe.</span><span class="sxs-lookup"><span data-stu-id="4499e-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="4499e-281">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa l'inserimento di costruttori in una classe `LifetimeEventsHostedService` (implementazione `IHostedService`) per registrare gli eventi.</span><span class="sxs-lookup"><span data-stu-id="4499e-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="4499e-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="4499e-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="4499e-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="4499e-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="4499e-284">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="4499e-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="4499e-285">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4499e-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="4499e-286">Esempi di repository Hosting in GitHub</span><span class="sxs-lookup"><span data-stu-id="4499e-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
