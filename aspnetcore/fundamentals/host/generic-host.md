---
title: Host generico .NET
author: guardrex
description: Informazioni sull'host generico in .NET, che gestisce l'avvio e la durata delle app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 879f31a5916646a4d63f9f503173dc9ff4c53434
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894153"
---
# <a name="net-generic-host"></a><span data-ttu-id="8d148-103">Host generico .NET</span><span class="sxs-lookup"><span data-stu-id="8d148-103">.NET Generic Host</span></span>

<span data-ttu-id="8d148-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d148-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8d148-105">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="8d148-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="8d148-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="8d148-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8d148-107">Questo argomento illustra l'host generico di ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile per l'hosting di app che non elaborano le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="8d148-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="8d148-108">Per informazioni sull'host Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="8d148-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="8d148-109">L'obiettivo dell'host generico è separare la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host.</span><span class="sxs-lookup"><span data-stu-id="8d148-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="8d148-110">La messaggistica, le attività in background e altri carichi di lavoro non HTTP basati sull'host generico traggono vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="8d148-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="8d148-111">L'host generico è una nuova funzionalità introdotta in ASP.NET Core 2.1 e non è adatto agli scenari di hosting Web.</span><span class="sxs-lookup"><span data-stu-id="8d148-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="8d148-112">Per gli scenari di hosting Web usare l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="8d148-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="8d148-113">L'host generico è in fase di sviluppo e sostituirà l'host Web in una versione futura, funzionando come API host principale negli scenari sia HTTP che non HTTP.</span><span class="sxs-lookup"><span data-stu-id="8d148-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="8d148-114">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d148-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8d148-115">Quando si esegue l'app di esempio in [Visual Studio Code](https://code.visualstudio.com/), usare un *terminale esterno o integrato*.</span><span class="sxs-lookup"><span data-stu-id="8d148-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="8d148-116">Non eseguire l'esempio in un ambiente `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="8d148-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="8d148-117">Per impostare la console in Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="8d148-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="8d148-118">Aprire il file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="8d148-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="8d148-119">Nella configurazione **.NET Core Launch (console)** trovare la voce **console**.</span><span class="sxs-lookup"><span data-stu-id="8d148-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="8d148-120">Impostare il valore su `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="8d148-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="8d148-121">Introduzione</span><span class="sxs-lookup"><span data-stu-id="8d148-121">Introduction</span></span>

<span data-ttu-id="8d148-122">La libreria host generico è disponibile nello [spazio dei nomi Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e viene specificata dal pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="8d148-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="8d148-123">Il pacchetto `Microsoft.Extensions.Hosting` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="8d148-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="8d148-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) è il punto di ingresso per l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="8d148-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="8d148-125">Ogni implementazione `IHostedService` viene eseguita nell'ordine di [registrazione del servizio in ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="8d148-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="8d148-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) viene chiamato su ogni `IHostedService` quando viene avviato l'host e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) viene chiamato in ordine di registrazione inverso quando l'host viene chiuso normalmente.</span><span class="sxs-lookup"><span data-stu-id="8d148-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8d148-127">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="8d148-127">Set up a host</span></span>

<span data-ttu-id="8d148-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) è il componente principale che le librerie e le app usano per inizializzare, compilare ed eseguire l'host:</span><span class="sxs-lookup"><span data-stu-id="8d148-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="8d148-129">Configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="8d148-129">Host configuration</span></span>

<span data-ttu-id="8d148-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="8d148-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="8d148-131">Generatore di configurazioni</span><span class="sxs-lookup"><span data-stu-id="8d148-131">Configuration builder</span></span>
* <span data-ttu-id="8d148-132">Configurazione del metodo di estensione</span><span class="sxs-lookup"><span data-stu-id="8d148-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="8d148-133">Generatore di configurazioni</span><span class="sxs-lookup"><span data-stu-id="8d148-133">Configuration builder</span></span>

<span data-ttu-id="8d148-134">La configurazione del generatore host viene creata chiamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="8d148-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="8d148-135">`ConfigureHostConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'host.</span><span class="sxs-lookup"><span data-stu-id="8d148-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="8d148-136">Il generatore di configurazioni inizializza [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) per l'uso nel processo di compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="8d148-137">La configurazione della variabile di ambiente non viene aggiunta per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="8d148-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="8d148-138">Chiamare [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel generatore di host per configurare l'host dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="8d148-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="8d148-139">`AddEnvironmentVariables` accetta un prefisso facoltativo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="8d148-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="8d148-140">L'app di esempio usa un prefisso di `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="8d148-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="8d148-141">Il prefisso viene rimosso quando vengono lette le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="8d148-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="8d148-142">Quando viene configurato l'host dell'app di esempio, il valore della variabile di ambiente per `PREFIX_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.</span><span class="sxs-lookup"><span data-stu-id="8d148-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="8d148-143">Durante lo sviluppo quando si usa [Visual Studio](https://www.visualstudio.com/) o si esegue un'app con `dotnet run`, le variabili di ambiente possono essere impostate nel file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8d148-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="8d148-144">Durante lo sviluppo in [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="8d148-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="8d148-145">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8d148-145">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8d148-146">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="8d148-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8d148-147">L'host usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="8d148-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="8d148-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8d148-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="8d148-149">Configurazione di esempio di `HostBuilder` che usa `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8d148-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="8d148-150">Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="8d148-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8d148-151">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="8d148-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="8d148-152">Il metodo `AddConfiguration` prevede che le chiavi corrispondano alle chiavi `HostBuilder` (ad esempio `environment`).</span><span class="sxs-lookup"><span data-stu-id="8d148-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="8d148-153">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host.</span><span class="sxs-lookup"><span data-stu-id="8d148-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="8d148-154">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="8d148-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8d148-155">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="8d148-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="8d148-156">Configurazione del metodo di estensione</span><span class="sxs-lookup"><span data-stu-id="8d148-156">Extension method configuration</span></span>

<span data-ttu-id="8d148-157">I metodi di estensione vengono chiamati nell'implementazione `IHostBuilder` per configurare la radice del contenuto e l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="8d148-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="8d148-158">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="8d148-158">Application Key (Name)</span></span>

<span data-ttu-id="8d148-159">La proprietà [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) viene impostata dalla configurazione dell'host durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="8d148-159">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="8d148-160">Per impostare il valore in modo esplicito, usare [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="8d148-160">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="8d148-161">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="8d148-161">**Key**: applicationName</span></span>  
<span data-ttu-id="8d148-162">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8d148-162">**Type**: *string*</span></span>  
<span data-ttu-id="8d148-163">**Impostazione predefinita**: il nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-163">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="8d148-164">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8d148-164">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8d148-165">**Variabile di ambiente**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8d148-165">**Environment variable**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

#### <a name="content-root"></a><span data-ttu-id="8d148-166">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="8d148-166">Content Root</span></span>

<span data-ttu-id="8d148-167">Questa impostazione determina la posizione da cui l'host inizia la ricerca dei file di contenuto.</span><span class="sxs-lookup"><span data-stu-id="8d148-167">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="8d148-168">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="8d148-168">**Key**: contentRoot</span></span>  
<span data-ttu-id="8d148-169">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8d148-169">**Type**: *string*</span></span>  
<span data-ttu-id="8d148-170">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-170">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8d148-171">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8d148-171">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="8d148-172">**Variabile di ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8d148-172">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="8d148-173">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="8d148-173">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="8d148-174">Ambiente</span><span class="sxs-lookup"><span data-stu-id="8d148-174">Environment</span></span>

<span data-ttu-id="8d148-175">Imposta l'[ambiente](xref:fundamentals/environments) per l'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-175">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8d148-176">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="8d148-176">**Key**: environment</span></span>  
<span data-ttu-id="8d148-177">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8d148-177">**Type**: *string*</span></span>  
<span data-ttu-id="8d148-178">**Impostazione predefinita**: Production</span><span class="sxs-lookup"><span data-stu-id="8d148-178">**Default**: Production</span></span>  
<span data-ttu-id="8d148-179">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8d148-179">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="8d148-180">**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8d148-180">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="8d148-181">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="8d148-181">The environment can be set to any value.</span></span> <span data-ttu-id="8d148-182">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="8d148-182">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8d148-183">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="8d148-183">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="8d148-184">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="8d148-184">ConfigureAppConfiguration</span></span>

<span data-ttu-id="8d148-185">La configurazione del generatore app viene creata chiamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="8d148-185">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="8d148-186">`ConfigureAppConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-186">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="8d148-187">`ConfigureAppConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="8d148-187">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8d148-188">L'app usa l'ultima opzione che imposta un valore.</span><span class="sxs-lookup"><span data-stu-id="8d148-188">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="8d148-189">La configurazione creata da `ConfigureAppConfiguration` è disponibile in [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) per le operazioni successive e in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="8d148-189">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="8d148-190">Configurazione app di esempio che usa `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="8d148-190">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="8d148-191">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8d148-191">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="8d148-192">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="8d148-192">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="8d148-193">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="8d148-193">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="8d148-194">Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="8d148-194">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8d148-195">Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="8d148-195">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="8d148-196">Il metodo `AddConfiguration` prevede una corrispondenza esatta per le chiavi di configurazione (ad esempio `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="8d148-196">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="8d148-197">La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-197">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="8d148-198">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="8d148-198">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8d148-199">Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="8d148-199">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="8d148-200">Per spostare i file delle impostazioni nella directory di output, specificare i file delle impostazioni come [elementi di progetto MSBuild](/visualstudio/msbuild/common-msbuild-project-items) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="8d148-200">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="8d148-201">L'app di esempio sposta i file delle impostazioni dell'app JSON e *hostsettings.json* con l'elemento **&lt;Content:&gt;** seguente:</span><span class="sxs-lookup"><span data-stu-id="8d148-201">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="8d148-202">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="8d148-202">ConfigureServices</span></span>

<span data-ttu-id="8d148-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) aggiunge servizi al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8d148-204">`ConfigureServices` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="8d148-204">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="8d148-205">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="8d148-205">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="8d148-206">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="8d148-206">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="8d148-207">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa il metodo di estensione `AddHostedService` per aggiungere all'app un servizio per gli eventi di durata (`LifetimeEventsHostedService`) e un'attività in background con timeout (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="8d148-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="8d148-208">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="8d148-208">ConfigureLogging</span></span>

<span data-ttu-id="8d148-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) aggiunge un delegato per la configurazione dell'interfaccia [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) specificata.</span><span class="sxs-lookup"><span data-stu-id="8d148-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="8d148-210">`ConfigureLogging` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="8d148-210">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="8d148-211">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="8d148-211">UseConsoleLifetime</span></span>

<span data-ttu-id="8d148-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM e chiama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) per avviare il processo di arresto.</span><span class="sxs-lookup"><span data-stu-id="8d148-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="8d148-213">`UseConsoleLifetime` sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="8d148-213">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="8d148-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) è preregistrata come implementazione di durata predefinita.</span><span class="sxs-lookup"><span data-stu-id="8d148-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="8d148-215">Viene usata l'ultima durata registrata.</span><span class="sxs-lookup"><span data-stu-id="8d148-215">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="8d148-216">Configurazione contenitore</span><span class="sxs-lookup"><span data-stu-id="8d148-216">Container configuration</span></span>

<span data-ttu-id="8d148-217">Per supportare l'inserimento di altri contenitori, l'host può accettare un elemento [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="8d148-217">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="8d148-218">La disponibilità di una factory non fa parte della registrazione del contenitore DI, ma è una funzionalità intrinseca dell'host usata per creare il contenitore DI concreto.</span><span class="sxs-lookup"><span data-stu-id="8d148-218">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="8d148-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) esegue l'override della factory predefinita usata per la creazione del provider di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="8d148-220">La configurazione del contenitore personalizzato è gestita dal metodo [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="8d148-220">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="8d148-221">`ConfigureContainer` offre un'esperienza fortemente tipizzata per la configurazione del contenitore sulla base dell'API host sottostante.</span><span class="sxs-lookup"><span data-stu-id="8d148-221">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="8d148-222">`ConfigureContainer` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="8d148-222">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="8d148-223">Creare un contenitore di servizi per l'app:</span><span class="sxs-lookup"><span data-stu-id="8d148-223">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="8d148-224">Specificare una factory per il contenitore di servizi:</span><span class="sxs-lookup"><span data-stu-id="8d148-224">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="8d148-225">Usare la factory e configurare il contenitore di servizi personalizzato per l'app:</span><span class="sxs-lookup"><span data-stu-id="8d148-225">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="8d148-226">Estendibilità</span><span class="sxs-lookup"><span data-stu-id="8d148-226">Extensibility</span></span>

<span data-ttu-id="8d148-227">L'estensibilità host viene eseguita con i metodi di estensione su `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8d148-227">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="8d148-228">L'esempio seguente illustra come un metodo di estensione estende un'implementazione `IHostBuilder` con [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="8d148-228">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="8d148-229">Il metodo di estensione (altrove nell'app) registra un `IHostedService` di RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="8d148-229">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="8d148-230">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="8d148-230">Manage the host</span></span>

<span data-ttu-id="8d148-231">L'implementazione [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) è responsabile dell'avvio e arresto delle implementazioni `IHostedService` che sono registrate nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="8d148-231">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="8d148-232">Esegui</span><span class="sxs-lookup"><span data-stu-id="8d148-232">Run</span></span>

<span data-ttu-id="8d148-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) avvia l'app e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="8d148-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="8d148-234">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8d148-234">RunAsync</span></span>

<span data-ttu-id="8d148-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) esegue l'app e restituisce un elemento `Task` che viene completato quando viene attivato il token di annullamento o l'arresto:</span><span class="sxs-lookup"><span data-stu-id="8d148-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="8d148-236">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="8d148-236">RunConsoleAsync</span></span>

<span data-ttu-id="8d148-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) abilita il supporto della console, compila e avvia l'host e resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM per eseguire l'arresto.</span><span class="sxs-lookup"><span data-stu-id="8d148-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="8d148-238">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="8d148-238">Start and StopAsync</span></span>

<span data-ttu-id="8d148-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) avvia l'host in modo sincrono.</span><span class="sxs-lookup"><span data-stu-id="8d148-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="8d148-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) prova ad arrestare l'host entro il timeout specificato.</span><span class="sxs-lookup"><span data-stu-id="8d148-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="8d148-241">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="8d148-241">StartAsync and StopAsync</span></span>

<span data-ttu-id="8d148-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="8d148-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arresta l'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="8d148-244">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="8d148-244">WaitForShutdown</span></span>

<span data-ttu-id="8d148-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) viene attivato tramite [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), ad esempio [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (in ascolto di `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="8d148-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="8d148-246">`WaitForShutdown` chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="8d148-246">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="8d148-247">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="8d148-247">WaitForShutdownAsync</span></span>

<span data-ttu-id="8d148-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) restituisce un elemento `Task` che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="8d148-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="8d148-249">Controllo esterno</span><span class="sxs-lookup"><span data-stu-id="8d148-249">External control</span></span>

<span data-ttu-id="8d148-250">Il controllo esterno dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:</span><span class="sxs-lookup"><span data-stu-id="8d148-250">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="8d148-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) viene chiamato all'inizio di [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), che attende il completamento prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="8d148-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="8d148-252">Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.</span><span class="sxs-lookup"><span data-stu-id="8d148-252">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8d148-253">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="8d148-253">IHostingEnvironment interface</span></span>

<span data-ttu-id="8d148-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) include informazioni sull'ambiente di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="8d148-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="8d148-255">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="8d148-255">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="8d148-256">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8d148-256">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8d148-257">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8d148-257">IApplicationLifetime interface</span></span>

<span data-ttu-id="8d148-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) consente attività post-avvio e di arresto, incluse le richieste di arresto normale.</span><span class="sxs-lookup"><span data-stu-id="8d148-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="8d148-259">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="8d148-259">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="8d148-260">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="8d148-260">Cancellation Token</span></span> | <span data-ttu-id="8d148-261">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="8d148-261">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="8d148-262">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="8d148-262">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="8d148-263">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="8d148-263">The host has fully started.</span></span> |
| [<span data-ttu-id="8d148-264">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="8d148-264">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="8d148-265">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="8d148-265">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8d148-266">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="8d148-266">All requests should be processed.</span></span> <span data-ttu-id="8d148-267">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="8d148-267">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="8d148-268">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="8d148-268">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="8d148-269">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="8d148-269">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8d148-270">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8d148-270">Requests may still be processing.</span></span> <span data-ttu-id="8d148-271">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="8d148-271">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="8d148-272">Inserire tramite costruttore il servizio `IApplicationLifetime` in qualsiasi classe.</span><span class="sxs-lookup"><span data-stu-id="8d148-272">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="8d148-273">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa l'inserimento di costruttori in una classe `LifetimeEventsHostedService` (implementazione `IHostedService`) per registrare gli eventi.</span><span class="sxs-lookup"><span data-stu-id="8d148-273">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="8d148-274">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="8d148-274">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="8d148-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="8d148-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="8d148-276">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="8d148-276">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="8d148-277">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8d148-277">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="8d148-278">Esempi di repository Hosting in GitHub</span><span class="sxs-lookup"><span data-stu-id="8d148-278">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
