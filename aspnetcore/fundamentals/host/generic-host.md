---
title: Host generico .NET
author: guardrex
description: Informazioni sull'host generico in ASP.NET Core, che gestisce l'avvio e la durata delle app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 817b7b3b420520992f18f1f207b412bc4555bdfa
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327391"
---
# <a name="net-generic-host"></a><span data-ttu-id="0776d-103">Host generico .NET</span><span class="sxs-lookup"><span data-stu-id="0776d-103">.NET Generic Host</span></span>

<span data-ttu-id="0776d-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0776d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="0776d-105">Le app ASP.NET Core configurano e avviano un host.</span><span class="sxs-lookup"><span data-stu-id="0776d-105">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="0776d-106">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="0776d-106">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="0776d-107">Questo articolo illustra l'host generico di ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), che si usa per le app che non elaborano le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="0776d-107">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="0776d-108">Lo scopo dell'host generico è separare la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host.</span><span class="sxs-lookup"><span data-stu-id="0776d-108">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="0776d-109">La messaggistica, le attività in background e altri carichi di lavoro non HTTP basati sull'host generico traggono vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="0776d-109">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="0776d-110">L'host generico è una nuova funzionalità introdotta in ASP.NET Core 2.1 e non è adatto agli scenari di hosting Web.</span><span class="sxs-lookup"><span data-stu-id="0776d-110">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="0776d-111">Per gli scenari di hosting Web usare l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="0776d-111">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="0776d-112">L'host generico sostituirà l'host Web in una versione futura, funzionando come API host principale negli scenari sia HTTP che non HTTP.</span><span class="sxs-lookup"><span data-stu-id="0776d-112">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="0776d-113">Le app ASP.NET Core configurano e avviano un host.</span><span class="sxs-lookup"><span data-stu-id="0776d-113">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="0776d-114">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="0776d-114">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="0776d-115">Questo articolo descrive l'host generico di .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span><span class="sxs-lookup"><span data-stu-id="0776d-115">This article covers the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

<span data-ttu-id="0776d-116">L'host generico è diverso dall'host Web per il fatto che separa la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host.</span><span class="sxs-lookup"><span data-stu-id="0776d-116">Generic Host differs from Web Host in that it decouples the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="0776d-117">La messaggistica, le attività in background e altri carichi di lavoro non HTTP possono usare l'host generico e trarre vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="0776d-117">Messaging, background tasks, and other non-HTTP workloads can use Generic Host and benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="0776d-118">A partire da ASP.NET Core 3.0, è consigliabile usare l'host generico per i carichi di lavoro HTTP e non HTTP.</span><span class="sxs-lookup"><span data-stu-id="0776d-118">Starting in ASP.NET Core 3.0, Generic Host is recommended for both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="0776d-119">Un'implementazione del server HTTP, se inclusa, viene eseguita come implementazione di <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="0776d-119">An HTTP server implementation, if included, runs as an implementation of <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="0776d-120">`IHostedService` è un'interfaccia che può essere usata anche per altri carichi di lavoro.</span><span class="sxs-lookup"><span data-stu-id="0776d-120">`IHostedService` is an interface that can be used for other workloads as well.</span></span>

<span data-ttu-id="0776d-121">L'host Web non è più consigliato per le app Web, ma resta disponibile per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="0776d-121">Web Host is no longer recommended for web apps but remains available for backward compatibility.</span></span>

> [!NOTE]
> <span data-ttu-id="0776d-122">Il resto di questo articolo non è ancora stato aggiornato per la versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="0776d-122">This remainder of this article has not yet been updated for 3.0.</span></span>

::: moniker-end

<span data-ttu-id="0776d-123">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0776d-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0776d-124">Quando si esegue l'app di esempio in [Visual Studio Code](https://code.visualstudio.com/), usare un *terminale esterno o integrato*.</span><span class="sxs-lookup"><span data-stu-id="0776d-124">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="0776d-125">Non eseguire l'esempio in un ambiente `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="0776d-125">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="0776d-126">Per impostare la console in Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="0776d-126">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="0776d-127">Aprire il file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="0776d-127">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="0776d-128">Nella configurazione **.NET Core Launch (console)** trovare la voce **console**.</span><span class="sxs-lookup"><span data-stu-id="0776d-128">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="0776d-129">Impostare il valore su `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="0776d-129">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="0776d-130">Introduzione</span><span class="sxs-lookup"><span data-stu-id="0776d-130">Introduction</span></span>

<span data-ttu-id="0776d-131">La libreria host generico è disponibile nello spazio dei nomi <xref:Microsoft.Extensions.Hosting> e viene specificata dal pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="0776d-131">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="0776d-132">Il pacchetto `Microsoft.Extensions.Hosting` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="0776d-132">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="0776d-133"><xref:Microsoft.Extensions.Hosting.IHostedService> è il punto di ingresso per l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="0776d-133"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="0776d-134">Ogni implementazione `IHostedService` viene eseguita nell'ordine di [registrazione del servizio in ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="0776d-134">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="0776d-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> viene chiamato su ogni `IHostedService` quando viene avviato l'host e <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> viene chiamato in ordine di registrazione inverso quando l'host viene chiuso normalmente.</span><span class="sxs-lookup"><span data-stu-id="0776d-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0776d-136">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="0776d-136">Set up a host</span></span>

<span data-ttu-id="0776d-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> è il componente principale che le librerie e le app usano per inizializzare, compilare ed eseguire l'host:</span><span class="sxs-lookup"><span data-stu-id="0776d-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="0776d-138">Opzioni</span><span class="sxs-lookup"><span data-stu-id="0776d-138">Options</span></span>

<span data-ttu-id="0776d-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configura le opzioni per <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="0776d-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="0776d-140">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="0776d-140">Shutdown timeout</span></span>

<span data-ttu-id="0776d-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> imposta il timeout per <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="0776d-142">Il valore predefinito è cinque secondi.</span><span class="sxs-lookup"><span data-stu-id="0776d-142">The default value is five seconds.</span></span>

<span data-ttu-id="0776d-143">La configurazione delle opzioni seguenti in `Program.Main` aumenta il timeout di arresto di cinque secondi a 20 secondi:</span><span class="sxs-lookup"><span data-stu-id="0776d-143">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="0776d-144">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="0776d-144">Default services</span></span>

<span data-ttu-id="0776d-145">I servizi seguenti vengono registrati durante l'inizializzazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="0776d-145">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="0776d-146">[Ambiente](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="0776d-146">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="0776d-147">[Configurazione](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="0776d-147">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="0776d-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="0776d-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="0776d-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="0776d-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="0776d-150">[Opzioni](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="0776d-150">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="0776d-151">[Registrazione](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="0776d-151">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="0776d-152">Configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="0776d-152">Host configuration</span></span>

<span data-ttu-id="0776d-153">La configurazione dell'host viene creata tramite:</span><span class="sxs-lookup"><span data-stu-id="0776d-153">Host configuration is created by:</span></span>

* <span data-ttu-id="0776d-154">Chiamata ai metodi di estensione su <xref:Microsoft.Extensions.Hosting.IHostBuilder> per impostare la [radice del contenuto](#content-root) e l'[ambiente](#environment).</span><span class="sxs-lookup"><span data-stu-id="0776d-154">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="0776d-155">Lettura della configurazione dai provider di configurazione in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-155">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="0776d-156">Metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="0776d-156">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="0776d-157">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="0776d-157">Application key (name)</span></span>

<span data-ttu-id="0776d-158">La proprietà [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) viene impostata dalla configurazione dell'host durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="0776d-158">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="0776d-159">Per impostare il valore in modo esplicito, usare [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="0776d-159">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="0776d-160">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="0776d-160">**Key**: applicationName</span></span>  
<span data-ttu-id="0776d-161">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="0776d-161">**Type**: *string*</span></span>  
<span data-ttu-id="0776d-162">**Impostazione predefinita**: nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-162">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="0776d-163">**Impostare usando**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="0776d-163">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="0776d-164">**Variabile di ambiente**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0776d-164">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="0776d-165">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="0776d-165">Content root</span></span>

<span data-ttu-id="0776d-166">Questa impostazione determina la posizione da cui l'host inizia la ricerca dei file di contenuto.</span><span class="sxs-lookup"><span data-stu-id="0776d-166">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="0776d-167">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="0776d-167">**Key**: contentRoot</span></span>  
<span data-ttu-id="0776d-168">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="0776d-168">**Type**: *string*</span></span>  
<span data-ttu-id="0776d-169">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-169">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0776d-170">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0776d-170">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0776d-171">**Variabile di ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0776d-171">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="0776d-172">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="0776d-172">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="0776d-173">Ambiente</span><span class="sxs-lookup"><span data-stu-id="0776d-173">Environment</span></span>

<span data-ttu-id="0776d-174">Imposta l'[ambiente](xref:fundamentals/environments) per l'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-174">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0776d-175">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="0776d-175">**Key**: environment</span></span>  
<span data-ttu-id="0776d-176">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="0776d-176">**Type**: *string*</span></span>  
<span data-ttu-id="0776d-177">**Impostazione predefinita**: Produzione</span><span class="sxs-lookup"><span data-stu-id="0776d-177">**Default**: Production</span></span>  
<span data-ttu-id="0776d-178">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0776d-178">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0776d-179">**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0776d-179">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="0776d-180">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="0776d-180">The environment can be set to any value.</span></span> <span data-ttu-id="0776d-181">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="0776d-181">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0776d-182">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="0776d-182">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="0776d-183">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="0776d-183">ConfigureHostConfiguration</span></span>

<span data-ttu-id="0776d-184">`ConfigureHostConfiguration` usa un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per creare un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfiguration> per l'host.</span><span class="sxs-lookup"><span data-stu-id="0776d-184">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="0776d-185">La configurazione dell'host viene usata per inizializzare l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> da usare nel processo di compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-185">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="0776d-186">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="0776d-186">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0776d-187">L'host usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="0776d-187">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="0776d-188">La configurazione dell'host passa automaticamente alla configurazione dell'app ([ConfigureAppConfiguration](#configureappconfiguration) e il resto dell'app).</span><span class="sxs-lookup"><span data-stu-id="0776d-188">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="0776d-189">Nessun provider è incluso per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0776d-189">No providers are included by default.</span></span> <span data-ttu-id="0776d-190">È necessario specificare in modo esplicito tutti i provider di configurazione richiesti dall'app in `ConfigureHostConfiguration`, inclusi:</span><span class="sxs-lookup"><span data-stu-id="0776d-190">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="0776d-191">Configurazione dei file (ad esempio, da un file *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="0776d-191">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="0776d-192">Configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0776d-192">Environment variable configuration.</span></span>
* <span data-ttu-id="0776d-193">Configurazione degli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="0776d-193">Command-line argument configuration.</span></span>
* <span data-ttu-id="0776d-194">Tutti gli altri provider di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="0776d-194">Any other required configuration providers.</span></span>

<span data-ttu-id="0776d-195">La configurazione file dell'host viene abilitata specificando il percorso di base dell'app con `SetBasePath` seguito da una chiamata a uno dei [provider di configurazione dei file](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0776d-195">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="0776d-196">L'app di esempio usa un file JSON, *hostsettings.json*, e chiama <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> per utilizzare le impostazioni di configurazione host del file.</span><span class="sxs-lookup"><span data-stu-id="0776d-196">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="0776d-197">Per aggiungere la [configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider) dell'host, chiamare <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sul generatore di host.</span><span class="sxs-lookup"><span data-stu-id="0776d-197">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="0776d-198">`AddEnvironmentVariables` accetta un prefisso facoltativo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="0776d-198">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="0776d-199">L'app di esempio usa un prefisso di `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="0776d-199">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="0776d-200">Il prefisso viene rimosso quando vengono lette le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0776d-200">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="0776d-201">Quando viene configurato l'host dell'app di esempio, il valore della variabile di ambiente per `PREFIX_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.</span><span class="sxs-lookup"><span data-stu-id="0776d-201">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="0776d-202">Durante lo sviluppo quando si usa [Visual Studio](https://www.visualstudio.com/) o si esegue un'app con `dotnet run`, le variabili di ambiente possono essere impostate nel file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0776d-202">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="0776d-203">Durante lo sviluppo in [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="0776d-203">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="0776d-204">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0776d-204">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="0776d-205">La [configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) viene aggiunta chiamando <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-205">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="0776d-206">La configurazione della riga di comando viene aggiunta per ultima per consentire agli argomenti della riga di comando di eseguire l'override della configurazione fornita dai provider di configurazione precedenti.</span><span class="sxs-lookup"><span data-stu-id="0776d-206">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="0776d-207">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0776d-207">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="0776d-208">È possibile fornire una configurazione aggiuntiva con le chiavi [applicationName](#application-key-name) e [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="0776d-208">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="0776d-209">Configurazione di esempio di `HostBuilder` che usa `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="0776d-209">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="0776d-210">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="0776d-210">ConfigureAppConfiguration</span></span>

<span data-ttu-id="0776d-211">La configurazione dell'app viene creata chiamando <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sull'implementazione <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0776d-211">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="0776d-212">`ConfigureAppConfiguration` usa un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per creare un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfiguration> per l'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-212">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="0776d-213">`ConfigureAppConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="0776d-213">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0776d-214">L'app usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="0776d-214">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="0776d-215">La configurazione creata da `ConfigureAppConfiguration` è disponibile in [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) per le operazioni successive e in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-215">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="0776d-216">La configurazione dell'app riceve automaticamente la configurazione dell'host fornita da [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0776d-216">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="0776d-217">Configurazione app di esempio che usa `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="0776d-217">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="0776d-218">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0776d-218">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="0776d-219">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="0776d-219">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="0776d-220">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="0776d-220">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="0776d-221">Per spostare i file delle impostazioni nella directory di output, specificare i file delle impostazioni come [elementi di progetto MSBuild](/visualstudio/msbuild/common-msbuild-project-items) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="0776d-221">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="0776d-222">L'app di esempio sposta i file delle impostazioni dell'app JSON e *hostsettings.json* con l'elemento `<Content>` seguente:</span><span class="sxs-lookup"><span data-stu-id="0776d-222">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="0776d-223">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="0776d-223">ConfigureServices</span></span>

<span data-ttu-id="0776d-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> aggiunge servizi al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0776d-225">`ConfigureServices` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="0776d-225">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="0776d-226">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="0776d-226">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="0776d-227">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="0776d-227">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="0776d-228">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa il metodo di estensione `AddHostedService` per aggiungere all'app un servizio per gli eventi di durata (`LifetimeEventsHostedService`) e un'attività in background con timeout (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="0776d-228">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="0776d-229">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="0776d-229">ConfigureLogging</span></span>

<span data-ttu-id="0776d-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> aggiunge un delegato per configurare l'interfaccia <xref:Microsoft.Extensions.Logging.ILoggingBuilder> fornita.</span><span class="sxs-lookup"><span data-stu-id="0776d-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="0776d-231">`ConfigureLogging` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="0776d-231">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="0776d-232">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="0776d-232">UseConsoleLifetime</span></span>

<span data-ttu-id="0776d-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM e chiama <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> per avviare il processo di arresto.</span><span class="sxs-lookup"><span data-stu-id="0776d-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="0776d-234">`UseConsoleLifetime` sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="0776d-234">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="0776d-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> è preregistrata come implementazione di durata predefinita.</span><span class="sxs-lookup"><span data-stu-id="0776d-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="0776d-236">Viene usata l'ultima durata registrata.</span><span class="sxs-lookup"><span data-stu-id="0776d-236">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="0776d-237">Configurazione contenitore</span><span class="sxs-lookup"><span data-stu-id="0776d-237">Container configuration</span></span>

<span data-ttu-id="0776d-238">Per supportare l'inserimento di altri contenitori, l'host può accettare un elemento <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="0776d-238">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="0776d-239">La disponibilità di una factory non fa parte della registrazione del contenitore DI, ma è una funzionalità intrinseca dell'host usata per creare il contenitore DI concreto.</span><span class="sxs-lookup"><span data-stu-id="0776d-239">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="0776d-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) esegue l'override della factory predefinita usata per la creazione del provider di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="0776d-241">La configurazione del contenitore personalizzato è gestita dal metodo <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-241">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="0776d-242">`ConfigureContainer` offre un'esperienza fortemente tipizzata per la configurazione del contenitore sulla base dell'API host sottostante.</span><span class="sxs-lookup"><span data-stu-id="0776d-242">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="0776d-243">`ConfigureContainer` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="0776d-243">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="0776d-244">Creare un contenitore di servizi per l'app:</span><span class="sxs-lookup"><span data-stu-id="0776d-244">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="0776d-245">Specificare una factory per il contenitore di servizi:</span><span class="sxs-lookup"><span data-stu-id="0776d-245">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="0776d-246">Usare la factory e configurare il contenitore di servizi personalizzato per l'app:</span><span class="sxs-lookup"><span data-stu-id="0776d-246">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="0776d-247">Estendibilità</span><span class="sxs-lookup"><span data-stu-id="0776d-247">Extensibility</span></span>

<span data-ttu-id="0776d-248">L'estensibilità host viene eseguita con i metodi di estensione su `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0776d-248">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="0776d-249">L'esempio seguente illustra come un metodo di estensione estende un'implementazione `IHostBuilder` con l'esempio [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) dimostrato in <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="0776d-249">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="0776d-250">Un'app stabilisce il metodo di estensione `UseHostedService` per registrare il servizio ospitato passato in `T`:</span><span class="sxs-lookup"><span data-stu-id="0776d-250">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="0776d-251">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="0776d-251">Manage the host</span></span>

<span data-ttu-id="0776d-252">L'implementazione <xref:Microsoft.Extensions.Hosting.IHost> è responsabile dell'avvio e arresto delle implementazioni `IHostedService` che sono registrate nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="0776d-252">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="0776d-253">Esegui</span><span class="sxs-lookup"><span data-stu-id="0776d-253">Run</span></span>

<span data-ttu-id="0776d-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> esegue l'app e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="0776d-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="0776d-255">RunAsync</span><span class="sxs-lookup"><span data-stu-id="0776d-255">RunAsync</span></span>

<span data-ttu-id="0776d-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> esegue l'app e restituisce un elemento `Task` che viene completato quando viene attivato il token di annullamento o l'arresto:</span><span class="sxs-lookup"><span data-stu-id="0776d-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="0776d-257">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="0776d-257">RunConsoleAsync</span></span>

<span data-ttu-id="0776d-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> abilita il supporto della console, compila e avvia l'host e resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM per eseguire l'arresto.</span><span class="sxs-lookup"><span data-stu-id="0776d-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="0776d-259">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="0776d-259">Start and StopAsync</span></span>

<span data-ttu-id="0776d-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> avvia l'host in modo sincrono.</span><span class="sxs-lookup"><span data-stu-id="0776d-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="0776d-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> prova ad arrestare l'host entro il timeout specificato.</span><span class="sxs-lookup"><span data-stu-id="0776d-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="0776d-262">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="0776d-262">StartAsync and StopAsync</span></span>

<span data-ttu-id="0776d-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="0776d-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> arresta l'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="0776d-265">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="0776d-265">WaitForShutdown</span></span>

<span data-ttu-id="0776d-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> viene attivato tramite l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostLifetime>, ad esempio <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (è in ascolto di `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0776d-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="0776d-267">`WaitForShutdown` chiama <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-267">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="0776d-268">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="0776d-268">WaitForShutdownAsync</span></span>

<span data-ttu-id="0776d-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> restituisce un elemento `Task` che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0776d-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="0776d-270">Controllo esterno</span><span class="sxs-lookup"><span data-stu-id="0776d-270">External control</span></span>

<span data-ttu-id="0776d-271">Il controllo esterno dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:</span><span class="sxs-lookup"><span data-stu-id="0776d-271">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="0776d-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> viene chiamato all'avvio di <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, che attende il completamento prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="0776d-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="0776d-273">Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.</span><span class="sxs-lookup"><span data-stu-id="0776d-273">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0776d-274">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="0776d-274">IHostingEnvironment interface</span></span>

<span data-ttu-id="0776d-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> include informazioni sull'ambiente di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="0776d-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="0776d-276">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="0776d-276">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0776d-277">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0776d-277">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0776d-278">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0776d-278">IApplicationLifetime interface</span></span>

<span data-ttu-id="0776d-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> consente attività post-avvio e di arresto, incluse le richieste di arresto normale.</span><span class="sxs-lookup"><span data-stu-id="0776d-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="0776d-280">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="0776d-280">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="0776d-281">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="0776d-281">Cancellation Token</span></span> | <span data-ttu-id="0776d-282">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="0776d-282">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="0776d-283">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="0776d-283">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="0776d-284">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="0776d-284">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0776d-285">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="0776d-285">All requests should be processed.</span></span> <span data-ttu-id="0776d-286">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="0776d-286">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="0776d-287">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="0776d-287">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0776d-288">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0776d-288">Requests may still be processing.</span></span> <span data-ttu-id="0776d-289">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="0776d-289">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="0776d-290">Inserire tramite costruttore il servizio `IApplicationLifetime` in qualsiasi classe.</span><span class="sxs-lookup"><span data-stu-id="0776d-290">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="0776d-291">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa l'inserimento di costruttori in una classe `LifetimeEventsHostedService` (implementazione `IHostedService`) per registrare gli eventi.</span><span class="sxs-lookup"><span data-stu-id="0776d-291">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="0776d-292">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="0776d-292">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="0776d-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="0776d-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="0776d-294">La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="0776d-294">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="0776d-295">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0776d-295">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="0776d-296">Esempi di repository Hosting in GitHub</span><span class="sxs-lookup"><span data-stu-id="0776d-296">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
