---
title: Host generico .NET
author: tdykstra
description: Informazioni sull'host generico .NET Core, che gestisce l'avvio e la durata delle app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 1582955cd18e6739111af05c9a892cd5cb4e270d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007235"
---
# <a name="net-generic-host"></a><span data-ttu-id="42f6f-103">Host generico .NET</span><span class="sxs-lookup"><span data-stu-id="42f6f-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="42f6f-104">Questo articolo introduce l'host generico .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) e fornisce indicazioni su come usarlo.</span><span class="sxs-lookup"><span data-stu-id="42f6f-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="42f6f-105">Che cos'è un host?</span><span class="sxs-lookup"><span data-stu-id="42f6f-105">What's a host?</span></span>

<span data-ttu-id="42f6f-106">Un *host* è un oggetto che incapsula le risorse di un'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42f6f-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="42f6f-107">Inserimento di dipendenze (DI)</span><span class="sxs-lookup"><span data-stu-id="42f6f-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="42f6f-108">Registrazione</span><span class="sxs-lookup"><span data-stu-id="42f6f-108">Logging</span></span>
* <span data-ttu-id="42f6f-109">Configurazione</span><span class="sxs-lookup"><span data-stu-id="42f6f-109">Configuration</span></span>
* <span data-ttu-id="42f6f-110">`IHostedService` Implementazioni</span><span class="sxs-lookup"><span data-stu-id="42f6f-110">`IHostedService` implementations</span></span>

<span data-ttu-id="42f6f-111">Quando un host viene avviato, chiama `IHostedService.StartAsync` in ogni implementazione di <xref:Microsoft.Extensions.Hosting.IHostedService> che trova nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="42f6f-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="42f6f-112">In un'app Web, una delle implementazioni di `IHostedService` è un servizio web che avvia un'[implementazione del server HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="42f6f-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="42f6f-113">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="42f6f-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="42f6f-114">Nelle versioni di ASP.NET Core precedenti alla 3.0, il [Web Host](xref:fundamentals/host/web-host) viene usato per i carichi di lavoro HTTP.</span><span class="sxs-lookup"><span data-stu-id="42f6f-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="42f6f-115">L'host Web non è più consigliato per le app Web e resta disponibile solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="42f6f-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="42f6f-116">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="42f6f-116">Set up a host</span></span>

<span data-ttu-id="42f6f-117">L'host è in genere configurato, compilato ed eseguito da codice nella classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="42f6f-118">Il metodo `Main`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-118">The `Main` method:</span></span>

* <span data-ttu-id="42f6f-119">Chiama un metodo `CreateHostBuilder` per creare e configurare un oggetto generatore.</span><span class="sxs-lookup"><span data-stu-id="42f6f-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="42f6f-120">Chiamate `Build` e metodi `Run` nell'oggetto generatore.</span><span class="sxs-lookup"><span data-stu-id="42f6f-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="42f6f-121">Di seguito è riportato il codice *Program.cs* per un carico di lavoro non HTTP, con un'unica implementazione di `IHostedService` aggiunta al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="42f6f-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="42f6f-122">Per un carico di lavoro HTTP, il metodo `Main` è lo stesso ma `CreateHostBuilder` chiama `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="42f6f-123">Se l'app usa Entity Framework Core, non modificare il nome o la firma del metodo `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="42f6f-124">Gli [strumenti di Entity Framework Core](/ef/core/miscellaneous/cli/) si aspettano di trovare un metodo `CreateHostBuilder` che configura l'host senza eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="42f6f-125">Per altre informazioni, vedere [Creazione DbContext in fase di progettazione](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="42f6f-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="42f6f-126">Impostazioni predefinite del generatore</span><span class="sxs-lookup"><span data-stu-id="42f6f-126">Default builder settings</span></span> 

<span data-ttu-id="42f6f-127">Il metodo <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="42f6f-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="42f6f-128">Imposta la [radice del contenuto](xref:fundamentals/index#content-root) sul percorso restituito da <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="42f6f-129">Carica la configurazione dell'host da:</span><span class="sxs-lookup"><span data-stu-id="42f6f-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="42f6f-130">Le variabili di ambiente con prefisso "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="42f6f-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="42f6f-131">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="42f6f-131">Command-line arguments.</span></span>
* <span data-ttu-id="42f6f-132">Carica la configurazione dell'app da:</span><span class="sxs-lookup"><span data-stu-id="42f6f-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="42f6f-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="42f6f-134">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="42f6f-135">[Segreti del manager](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="42f6f-136">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-136">Environment variables.</span></span>
  * <span data-ttu-id="42f6f-137">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="42f6f-137">Command-line arguments.</span></span>
* <span data-ttu-id="42f6f-138">Aggiunge i provider di [log](xref:fundamentals/logging/index) seguenti:</span><span class="sxs-lookup"><span data-stu-id="42f6f-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="42f6f-139">Console</span><span class="sxs-lookup"><span data-stu-id="42f6f-139">Console</span></span>
  * <span data-ttu-id="42f6f-140">Debug</span><span class="sxs-lookup"><span data-stu-id="42f6f-140">Debug</span></span>
  * <span data-ttu-id="42f6f-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="42f6f-141">EventSource</span></span>
  * <span data-ttu-id="42f6f-142">EventLog (solo quando è in esecuzione su Windows)</span><span class="sxs-lookup"><span data-stu-id="42f6f-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="42f6f-143">Abilita la [convalida dell'ambito](xref:fundamentals/dependency-injection#scope-validation) e la [convalida delle dipendenze](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) quando l'ambiente è lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="42f6f-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="42f6f-144">Il metodo `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="42f6f-145">carica la configurazione dell'host dalle variabili di ambiente con prefisso "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="42f6f-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="42f6f-146">Imposta il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web e lo configura usando i provider di configurazione dell'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="42f6f-147">Per le opzioni predefinite del server Kestrel, vedere <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="42f6f-148">Aggiunge [il middleware dell'applicazione di filtri dell'host](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="42f6f-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="42f6f-149">Aggiunge [il middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) se ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="42f6f-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="42f6f-150">Abilita l'integrazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="42f6f-150">Enables IIS integration.</span></span> <span data-ttu-id="42f6f-151">Per le opzioni predefinite di IIS, vedere <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="42f6f-152">Le sezioni [Impostazioni per tutti i tipi di app](#settings-for-all-app-types) e [Impostazioni per le app Web](#settings-for-web-apps) più avanti in questo articolo mostrano come eseguire l'override delle impostazioni predefinite del generatore.</span><span class="sxs-lookup"><span data-stu-id="42f6f-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="42f6f-153">Servizi forniti dal framework</span><span class="sxs-lookup"><span data-stu-id="42f6f-153">Framework-provided services</span></span>

<span data-ttu-id="42f6f-154">I servizi che vengono registrati automaticamente includono:</span><span class="sxs-lookup"><span data-stu-id="42f6f-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="42f6f-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="42f6f-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="42f6f-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="42f6f-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="42f6f-158">Per ulteriori informazioni sui servizi forniti dal Framework, vedere <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="42f6f-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="42f6f-160">Inserire il servizio <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (in precedenza `IApplicationLifetime`) in qualsiasi classe per gestire le attività post-avvio e di arresto normale.</span><span class="sxs-lookup"><span data-stu-id="42f6f-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="42f6f-161">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi del gestore dell'evento di avvio e di arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="42f6f-162">L'interfaccia include anche un metodo `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="42f6f-163">L'esempio seguente è un'implementazione di `IHostedService` che registra gli eventi `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="42f6f-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-164">IHostLifetime</span></span>

<span data-ttu-id="42f6f-165">L'implementazione <xref:Microsoft.Extensions.Hosting.IHostLifetime> controlla quando l'host viene avviato e quando si arresta.</span><span class="sxs-lookup"><span data-stu-id="42f6f-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="42f6f-166">Viene usata l'ultima implementazione registrata.</span><span class="sxs-lookup"><span data-stu-id="42f6f-166">The last implementation registered is used.</span></span>

<span data-ttu-id="42f6f-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> è l'implementazione `IHostLifetime` predefinita.</span><span class="sxs-lookup"><span data-stu-id="42f6f-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="42f6f-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="42f6f-169">resta in ascolto di CTRL+C/SIGINT o SIGTERM e chiama <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> per avviare il processo di arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="42f6f-170">Sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="42f6f-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="42f6f-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="42f6f-171">IHostEnvironment</span></span>

<span data-ttu-id="42f6f-172">Inserire il servizio <xref:Microsoft.Extensions.Hosting.IHostEnvironment> in una classe per ottenere informazioni su quanto segue:</span><span class="sxs-lookup"><span data-stu-id="42f6f-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="42f6f-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="42f6f-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="42f6f-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="42f6f-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="42f6f-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="42f6f-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="42f6f-176">Le app Web implementano l'interfaccia `IWebHostEnvironment`, che eredita `IHostEnvironment` e aggiunge:</span><span class="sxs-lookup"><span data-stu-id="42f6f-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="42f6f-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="42f6f-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="42f6f-178">Configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="42f6f-178">Host configuration</span></span>

<span data-ttu-id="42f6f-179">La configurazione dell'host viene usata per le proprietà dell'implementazione <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="42f6f-180">La configurazione dell'host è disponibile da [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) all'interno di <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="42f6f-181">Dopo `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` viene sostituito con la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="42f6f-182">Per aggiungere la configurazione dell'host, chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> su `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="42f6f-183">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="42f6f-184">L'host usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="42f6f-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="42f6f-185">Il provider di variabili di ambiente con prefisso `DOTNET_` e gli argomenti della riga di comando vengono inclusi da CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="42f6f-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="42f6f-186">Per le app Web, viene aggiunto il provider di variabili di ambiente con prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="42f6f-187">Il prefisso viene rimosso quando vengono lette le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="42f6f-188">Ad esempio, il valore della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="42f6f-189">L'esempio seguente crea la configurazione host:</span><span class="sxs-lookup"><span data-stu-id="42f6f-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="42f6f-190">Configurazione dell'app</span><span class="sxs-lookup"><span data-stu-id="42f6f-190">App configuration</span></span>

<span data-ttu-id="42f6f-191">La configurazione dell'app viene creata chiamando <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> su `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="42f6f-192">`ConfigureAppConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="42f6f-193">L'app usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="42f6f-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="42f6f-194">La configurazione creata da `ConfigureAppConfiguration` è disponibile in [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) per le operazioni successive e come servizio da DI.</span><span class="sxs-lookup"><span data-stu-id="42f6f-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="42f6f-195">La configurazione dell'host viene aggiunta anche alla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="42f6f-196">Per altre informazioni, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="42f6f-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="42f6f-197">Impostazioni per tutti i tipi di app</span><span class="sxs-lookup"><span data-stu-id="42f6f-197">Settings for all app types</span></span>

<span data-ttu-id="42f6f-198">Questa sezione elenca le impostazioni dell'host che si applicano ai carichi di lavoro HTTP e non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="42f6f-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="42f6f-199">Per impostazione predefinita, le variabili di ambiente usate per configurare queste impostazioni possono avere un prefisso `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="42f6f-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="42f6f-200">ApplicationName</span></span>

<span data-ttu-id="42f6f-201">La proprietà [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) viene impostata dalla configurazione dell'host durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="42f6f-202">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="42f6f-202">**Key**: applicationName</span></span>  
<span data-ttu-id="42f6f-203">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-203">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-204">**Predefinito**: nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="42f6f-205">**Variabile di ambiente**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="42f6f-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="42f6f-206">per impostare questo valore usare la variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="42f6f-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="42f6f-207">ContentRootPath</span></span>

<span data-ttu-id="42f6f-208">La proprietà [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) determina il punto in cui l'host inizia a cercare i file di contenuto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="42f6f-209">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="42f6f-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="42f6f-210">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="42f6f-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="42f6f-211">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-211">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-212">**Predefinito**: la cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="42f6f-213">**Variabile di ambiente**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="42f6f-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="42f6f-214">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseContentRoot` su `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="42f6f-215">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="42f6f-215">For more information, see:</span></span>

* <span data-ttu-id="42f6f-216">[Fundamentals: Radice del contenuto @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="42f6f-216">[Fundamentals: Content root](xref:fundamentals/index#content-root)</span></span>
* [<span data-ttu-id="42f6f-217">WebRoot</span><span class="sxs-lookup"><span data-stu-id="42f6f-217">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="42f6f-218">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="42f6f-218">EnvironmentName</span></span>

<span data-ttu-id="42f6f-219">La proprietà [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) può essere impostata su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="42f6f-219">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="42f6f-220">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-220">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="42f6f-221">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="42f6f-221">Values aren't case sensitive.</span></span>

<span data-ttu-id="42f6f-222">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="42f6f-222">**Key**: environment</span></span>  
<span data-ttu-id="42f6f-223">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-223">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-224">**Impostazione predefinita**: Produzione</span><span class="sxs-lookup"><span data-stu-id="42f6f-224">**Default**: Production</span></span>  
<span data-ttu-id="42f6f-225">**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="42f6f-225">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="42f6f-226">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseEnvironment` su `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-226">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="42f6f-227">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="42f6f-227">ShutdownTimeout</span></span>

<span data-ttu-id="42f6f-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) imposta il timeout per <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="42f6f-229">Il valore predefinito è cinque secondi.</span><span class="sxs-lookup"><span data-stu-id="42f6f-229">The default value is five seconds.</span></span>  <span data-ttu-id="42f6f-230">Durante il periodo di timeout, l'host:</span><span class="sxs-lookup"><span data-stu-id="42f6f-230">During the timeout period, the host:</span></span>

* <span data-ttu-id="42f6f-231">attiva [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="42f6f-231">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="42f6f-232">Tenta di arrestare i servizi ospitati, registrando gli errori per i servizi che non si interrompono.</span><span class="sxs-lookup"><span data-stu-id="42f6f-232">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="42f6f-233">Se il periodo di timeout scade prima che siano stati arrestati tutti i servizi ospitati, gli eventuali servizi attivi rimanenti vengono interrotti quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="42f6f-233">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="42f6f-234">I servizi si arrestano anche se non hanno completato l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="42f6f-234">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="42f6f-235">Se l'arresto dei servizi richiede più tempo, aumentare il valore di timeout.</span><span class="sxs-lookup"><span data-stu-id="42f6f-235">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="42f6f-236">**Chiave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="42f6f-236">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="42f6f-237">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="42f6f-237">**Type**: *int*</span></span>  
<span data-ttu-id="42f6f-238">**Predefinito**: 5 secondi **variabile di ambiente**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="42f6f-238">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="42f6f-239">Per impostare questo valore, usare la variabile di ambiente o configurare `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="42f6f-240">Nell'esempio seguente il timeout viene impostato su 20 secondi:</span><span class="sxs-lookup"><span data-stu-id="42f6f-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="42f6f-241">Impostazioni per le app Web</span><span class="sxs-lookup"><span data-stu-id="42f6f-241">Settings for web apps</span></span>

<span data-ttu-id="42f6f-242">Alcune impostazioni host si applicano solo ai carichi di lavoro HTTP.</span><span class="sxs-lookup"><span data-stu-id="42f6f-242">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="42f6f-243">Per impostazione predefinita, le variabili di ambiente usate per configurare queste impostazioni possono avere un prefisso `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-243">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="42f6f-244">Per queste impostazioni sono disponibili i metodi di estensione su `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-244">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="42f6f-245">Gli esempi di codice che illustrano come chiamare i metodi di estensione presumono che `webBuilder` sia un'istanza di `IWebHostBuilder`, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="42f6f-245">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="42f6f-246">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="42f6f-246">CaptureStartupErrors</span></span>

<span data-ttu-id="42f6f-247">Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-247">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="42f6f-248">Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="42f6f-248">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="42f6f-249">**Chiave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="42f6f-249">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="42f6f-250">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="42f6f-250">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="42f6f-251">**Impostazione predefinita**: il valore predefinito è `false`, a meno che l'app non venga eseguita con Kestrel in IIS; in tal caso il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-251">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="42f6f-252">**Variabile di ambiente**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="42f6f-252">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="42f6f-253">Per impostare questo valore, usare la configurazione o chiamare `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-253">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="42f6f-254">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="42f6f-254">DetailedErrors</span></span>

<span data-ttu-id="42f6f-255">Quando è abilitata o quando l'ambiente è `Development`, l'app acquisisce gli errori dettagliati.</span><span class="sxs-lookup"><span data-stu-id="42f6f-255">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="42f6f-256">**Chiave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="42f6f-256">**Key**: detailedErrors</span></span>  
<span data-ttu-id="42f6f-257">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="42f6f-257">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="42f6f-258">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="42f6f-258">**Default**: false</span></span>  
<span data-ttu-id="42f6f-259">**Variabile di ambiente**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="42f6f-259">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="42f6f-260">Per impostare questo valore, usare la configurazione o chiamare `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-260">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="42f6f-261">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="42f6f-261">HostingStartupAssemblies</span></span>

<span data-ttu-id="42f6f-262">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio.</span><span class="sxs-lookup"><span data-stu-id="42f6f-262">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="42f6f-263">Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-263">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="42f6f-264">Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.</span><span class="sxs-lookup"><span data-stu-id="42f6f-264">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="42f6f-265">**Chiave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="42f6f-265">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="42f6f-266">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-266">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-267">**Impostazione predefinita**: Stringa vuota</span><span class="sxs-lookup"><span data-stu-id="42f6f-267">**Default**: Empty string</span></span>  
<span data-ttu-id="42f6f-268">**Variabile di ambiente**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="42f6f-268">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="42f6f-269">Per impostare questo valore, usare la configurazione o chiamare `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-269">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="42f6f-270">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="42f6f-270">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="42f6f-271">Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da escludere all'avvio.</span><span class="sxs-lookup"><span data-stu-id="42f6f-271">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="42f6f-272">**Chiave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="42f6f-272">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="42f6f-273">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-273">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-274">**Impostazione predefinita**: Stringa vuota</span><span class="sxs-lookup"><span data-stu-id="42f6f-274">**Default**: Empty string</span></span>  
<span data-ttu-id="42f6f-275">**Variabile di ambiente**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="42f6f-275">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="42f6f-276">Per impostare questo valore, usare la configurazione o chiamare `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-276">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="42f6f-277">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="42f6f-277">HTTPS_Port</span></span>

<span data-ttu-id="42f6f-278">Porta di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="42f6f-278">The HTTPS redirect port.</span></span> <span data-ttu-id="42f6f-279">Usata per [imporre HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="42f6f-279">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="42f6f-280">**Chiave**: https_port</span><span class="sxs-lookup"><span data-stu-id="42f6f-280">**Key**: https_port</span></span>  
<span data-ttu-id="42f6f-281">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-281">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-282">**Predefinito**: non è impostato nessun valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="42f6f-282">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="42f6f-283">**Variabile di ambiente**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="42f6f-283">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="42f6f-284">Per impostare questo valore, usare la configurazione o chiamare `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-284">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="42f6f-285">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="42f6f-285">PreferHostingUrls</span></span>

<span data-ttu-id="42f6f-286">Indica se l'host deve eseguire l'ascolto sugli URL configurati con `IWebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-286">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="42f6f-287">**Chiave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="42f6f-287">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="42f6f-288">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="42f6f-288">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="42f6f-289">**Impostazione predefinita**: true</span><span class="sxs-lookup"><span data-stu-id="42f6f-289">**Default**: true</span></span>  
<span data-ttu-id="42f6f-290">**Variabile di ambiente**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="42f6f-290">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="42f6f-291">Per impostare questo valore, usare la variabile di ambiente o chiamare `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-291">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="42f6f-292">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="42f6f-292">PreventHostingStartup</span></span>

<span data-ttu-id="42f6f-293">Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-293">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="42f6f-294">Per altre informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-294">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="42f6f-295">**Chiave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="42f6f-295">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="42f6f-296">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="42f6f-296">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="42f6f-297">**Impostazione predefinita**: false</span><span class="sxs-lookup"><span data-stu-id="42f6f-297">**Default**: false</span></span>  
<span data-ttu-id="42f6f-298">**Variabile di ambiente**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="42f6f-298">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="42f6f-299">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-299">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="42f6f-300">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="42f6f-300">StartupAssembly</span></span>

<span data-ttu-id="42f6f-301">L'assembly per la ricerca della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-301">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="42f6f-302">**Chiave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="42f6f-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="42f6f-303">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-303">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-304">**Impostazione predefinita**: assembly dell'app</span><span class="sxs-lookup"><span data-stu-id="42f6f-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="42f6f-305">**Variabile di ambiente**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="42f6f-305">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="42f6f-306">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-306">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="42f6f-307">`UseStartup` può richiedere un nome di assembly (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="42f6f-307">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="42f6f-308">Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="42f6f-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="42f6f-309">URL</span><span class="sxs-lookup"><span data-stu-id="42f6f-309">URLs</span></span>

<span data-ttu-id="42f6f-310">Elenco delimitato da punto e virgola degli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="42f6f-310">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="42f6f-311">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-311">For example, `http://localhost:123`.</span></span> <span data-ttu-id="42f6f-312">Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="42f6f-312">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="42f6f-313">Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL.</span><span class="sxs-lookup"><span data-stu-id="42f6f-313">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="42f6f-314">I formati supportati variano a seconda del server.</span><span class="sxs-lookup"><span data-stu-id="42f6f-314">Supported formats vary among servers.</span></span>

<span data-ttu-id="42f6f-315">**Chiave**: urls</span><span class="sxs-lookup"><span data-stu-id="42f6f-315">**Key**: urls</span></span>  
<span data-ttu-id="42f6f-316">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-316">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-317">**Impostazione predefinita**: `http://localhost:5000` e `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="42f6f-317">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="42f6f-318">**Variabile di ambiente**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="42f6f-318">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="42f6f-319">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-319">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="42f6f-320">Kestrel ha una propria API di configurazione degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="42f6f-320">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="42f6f-321">Per altre informazioni, vedere <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-321">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="42f6f-322">WebRoot</span><span class="sxs-lookup"><span data-stu-id="42f6f-322">WebRoot</span></span>

<span data-ttu-id="42f6f-323">Il percorso relativo degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-323">The relative path to the app's static assets.</span></span>

<span data-ttu-id="42f6f-324">**Chiave**: webroot</span><span class="sxs-lookup"><span data-stu-id="42f6f-324">**Key**: webroot</span></span>  
<span data-ttu-id="42f6f-325">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-325">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-326">**Predefinito**: Il valore predefinito è `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-326">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="42f6f-327">Il percorso di *{radice del contenuto}/wwwroot* deve esistere.</span><span class="sxs-lookup"><span data-stu-id="42f6f-327">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="42f6f-328">Se il percorso non esiste, viene usato un provider di file no-op.</span><span class="sxs-lookup"><span data-stu-id="42f6f-328">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="42f6f-329">**Variabile di ambiente**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="42f6f-329">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="42f6f-330">Per impostare questo valore, usare la variabile di ambiente o chiamare `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-330">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="42f6f-331">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="42f6f-331">For more information, see:</span></span>

* <span data-ttu-id="42f6f-332">[Fundamentals: Radice Web @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="42f6f-332">[Fundamentals: Web root](xref:fundamentals/index#web-root)</span></span>
* [<span data-ttu-id="42f6f-333">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="42f6f-333">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="42f6f-334">Gestire la durata dell'host</span><span class="sxs-lookup"><span data-stu-id="42f6f-334">Manage the host lifetime</span></span>

<span data-ttu-id="42f6f-335">Chiamare metodi sull'implementazione <xref:Microsoft.Extensions.Hosting.IHost> incorporata per avviare e arrestare l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-335">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="42f6f-336">Questi metodi influiscono su tutte le implementazioni <xref:Microsoft.Extensions.Hosting.IHostedService> che vengono registrate nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="42f6f-336">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="42f6f-337">Esegui</span><span class="sxs-lookup"><span data-stu-id="42f6f-337">Run</span></span>

<span data-ttu-id="42f6f-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> esegue l'app e blocca il thread di chiamata fino all'arresto dell'host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="42f6f-339">RunAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-339">RunAsync</span></span>

<span data-ttu-id="42f6f-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> esegue l'app e restituisce un elemento <xref:System.Threading.Tasks.Task> che viene completato quando viene attivato il token di annullamento o l'arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="42f6f-341">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-341">RunConsoleAsync</span></span>

<span data-ttu-id="42f6f-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> abilita il supporto della console, compila e avvia l'host e resta in ascolto di CTRL+C/SIGINT o SIGTERM per eseguire l'arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="42f6f-343">Start</span><span class="sxs-lookup"><span data-stu-id="42f6f-343">Start</span></span>

<span data-ttu-id="42f6f-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> avvia l'host in modo sincrono.</span><span class="sxs-lookup"><span data-stu-id="42f6f-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="42f6f-345">StartAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-345">StartAsync</span></span>

<span data-ttu-id="42f6f-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> avvia l'host e restituisce un elemento <xref:System.Threading.Tasks.Task> che viene completato quando viene attivato il token di annullamento o l'arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="42f6f-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> viene chiamato all'avvio di `StartAsync`, che attende il completamento prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="42f6f-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="42f6f-348">Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.</span><span class="sxs-lookup"><span data-stu-id="42f6f-348">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="42f6f-349">StopAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-349">StopAsync</span></span>

<span data-ttu-id="42f6f-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> prova ad arrestare l'host entro il timeout specificato.</span><span class="sxs-lookup"><span data-stu-id="42f6f-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="42f6f-351">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="42f6f-351">WaitForShutdown</span></span>

<span data-ttu-id="42f6f-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocca il thread chiamante finché non viene attivato l'arresto da IHostLifetime, ad esempio tramite Ctrl + C/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="42f6f-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="42f6f-353">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-353">WaitForShutdownAsync</span></span>

<span data-ttu-id="42f6f-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> restituisce un elemento <xref:System.Threading.Tasks.Task> che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="42f6f-355">Controllo esterno</span><span class="sxs-lookup"><span data-stu-id="42f6f-355">External control</span></span>

<span data-ttu-id="42f6f-356">Il controllo diretto della durata dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:</span><span class="sxs-lookup"><span data-stu-id="42f6f-356">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="42f6f-357">Le app ASP.NET Core configurano e avviano un host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-357">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="42f6f-358">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-358">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="42f6f-359">Questo articolo illustra l'host generico di ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), che si usa per le app che non elaborano le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="42f6f-359">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="42f6f-360">Lo scopo dell'host generico è separare la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-360">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="42f6f-361">La messaggistica, le attività in background e altri carichi di lavoro non HTTP basati sull'host generico traggono vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="42f6f-361">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="42f6f-362">L'host generico è una nuova funzionalità introdotta in ASP.NET Core 2.1 e non è adatto agli scenari di hosting Web.</span><span class="sxs-lookup"><span data-stu-id="42f6f-362">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="42f6f-363">Per gli scenari di hosting Web usare l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="42f6f-363">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="42f6f-364">L'host generico sostituirà l'host Web in una versione futura, funzionando come API host principale negli scenari sia HTTP che non HTTP.</span><span class="sxs-lookup"><span data-stu-id="42f6f-364">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="42f6f-365">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="42f6f-365">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="42f6f-366">Quando si esegue l'app di esempio in [Visual Studio Code](https://code.visualstudio.com/), usare un *terminale esterno o integrato*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-366">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="42f6f-367">Non eseguire l'esempio in un ambiente `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-367">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="42f6f-368">Per impostare la console in Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="42f6f-368">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="42f6f-369">Aprire il file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-369">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="42f6f-370">Nella configurazione **.NET Core Launch (console)** trovare la voce **console**.</span><span class="sxs-lookup"><span data-stu-id="42f6f-370">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="42f6f-371">Impostare il valore su `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-371">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="42f6f-372">Introduzione</span><span class="sxs-lookup"><span data-stu-id="42f6f-372">Introduction</span></span>

<span data-ttu-id="42f6f-373">La libreria host generico è disponibile nello spazio dei nomi <xref:Microsoft.Extensions.Hosting> e viene specificata dal pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="42f6f-373">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="42f6f-374">Il pacchetto `Microsoft.Extensions.Hosting` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).</span><span class="sxs-lookup"><span data-stu-id="42f6f-374">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="42f6f-375"><xref:Microsoft.Extensions.Hosting.IHostedService> è il punto di ingresso per l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="42f6f-375"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="42f6f-376">Ogni implementazione <xref:Microsoft.Extensions.Hosting.IHostedService> viene eseguita nell'ordine di [registrazione del servizio in ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="42f6f-376">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="42f6f-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> viene chiamato su ogni <xref:Microsoft.Extensions.Hosting.IHostedService> quando viene avviato l'host e <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> viene chiamato in ordine di registrazione inverso quando l'host viene chiuso normalmente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="42f6f-378">Configurare un host</span><span class="sxs-lookup"><span data-stu-id="42f6f-378">Set up a host</span></span>

<span data-ttu-id="42f6f-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> è il componente principale che le librerie e le app usano per inizializzare, compilare ed eseguire l'host:</span><span class="sxs-lookup"><span data-stu-id="42f6f-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="42f6f-380">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42f6f-380">Options</span></span>

<span data-ttu-id="42f6f-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configura le opzioni per <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="42f6f-382">Timeout di arresto</span><span class="sxs-lookup"><span data-stu-id="42f6f-382">Shutdown timeout</span></span>

<span data-ttu-id="42f6f-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> imposta il timeout per <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="42f6f-384">Il valore predefinito è cinque secondi.</span><span class="sxs-lookup"><span data-stu-id="42f6f-384">The default value is five seconds.</span></span>

<span data-ttu-id="42f6f-385">La configurazione delle opzioni seguenti in `Program.Main` aumenta il timeout di arresto di cinque secondi a 20 secondi:</span><span class="sxs-lookup"><span data-stu-id="42f6f-385">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="42f6f-386">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="42f6f-386">Default services</span></span>

<span data-ttu-id="42f6f-387">I servizi seguenti vengono registrati durante l'inizializzazione dell'host:</span><span class="sxs-lookup"><span data-stu-id="42f6f-387">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="42f6f-388">[Ambiente](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-388">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="42f6f-389">[Configurazione](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-389">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="42f6f-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="42f6f-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="42f6f-392">[Opzioni](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-392">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="42f6f-393">[Registrazione](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="42f6f-393">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="42f6f-394">Configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="42f6f-394">Host configuration</span></span>

<span data-ttu-id="42f6f-395">La configurazione dell'host viene creata tramite:</span><span class="sxs-lookup"><span data-stu-id="42f6f-395">Host configuration is created by:</span></span>

* <span data-ttu-id="42f6f-396">Chiamata ai metodi di estensione su <xref:Microsoft.Extensions.Hosting.IHostBuilder> per impostare la [radice del contenuto](#content-root) e l'[ambiente](#environment).</span><span class="sxs-lookup"><span data-stu-id="42f6f-396">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="42f6f-397">Lettura della configurazione dai provider di configurazione in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-397">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="42f6f-398">Metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="42f6f-398">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="42f6f-399">Chiave applicazione (nome)</span><span class="sxs-lookup"><span data-stu-id="42f6f-399">Application key (name)</span></span>

<span data-ttu-id="42f6f-400">La proprietà [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) viene impostata dalla configurazione dell'host durante la costruzione dell'host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-400">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="42f6f-401">Per impostare il valore in modo esplicito, usare [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="42f6f-401">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="42f6f-402">**Chiave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="42f6f-402">**Key**: applicationName</span></span>  
<span data-ttu-id="42f6f-403">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-403">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-404">**Impostazione predefinita**: nome dell'assembly contenente il punto di ingresso dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-404">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="42f6f-405">**Impostare usando**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="42f6f-405">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="42f6f-406">**Variabile di ambiente**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="42f6f-406">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="42f6f-407">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="42f6f-407">Content root</span></span>

<span data-ttu-id="42f6f-408">Questa impostazione determina la posizione da cui l'host inizia la ricerca dei file di contenuto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-408">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="42f6f-409">**Chiave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="42f6f-409">**Key**: contentRoot</span></span>  
<span data-ttu-id="42f6f-410">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-410">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-411">**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-411">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="42f6f-412">**Impostare usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="42f6f-412">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="42f6f-413">**Variabile di ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="42f6f-413">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="42f6f-414">Se il percorso non esiste, l'host non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="42f6f-414">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="42f6f-415">Per ulteriori informazioni, vedere [Fundamentals: Radice del contenuto @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="42f6f-415">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="42f6f-416">Ambiente</span><span class="sxs-lookup"><span data-stu-id="42f6f-416">Environment</span></span>

<span data-ttu-id="42f6f-417">Imposta l'[ambiente](xref:fundamentals/environments) per l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-417">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="42f6f-418">**Chiave**: environment</span><span class="sxs-lookup"><span data-stu-id="42f6f-418">**Key**: environment</span></span>  
<span data-ttu-id="42f6f-419">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="42f6f-419">**Type**: *string*</span></span>  
<span data-ttu-id="42f6f-420">**Impostazione predefinita**: Produzione</span><span class="sxs-lookup"><span data-stu-id="42f6f-420">**Default**: Production</span></span>  
<span data-ttu-id="42f6f-421">**Impostare usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="42f6f-421">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="42f6f-422">**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="42f6f-422">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="42f6f-423">L'ambiente può essere impostato su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="42f6f-423">The environment can be set to any value.</span></span> <span data-ttu-id="42f6f-424">I valori definiti dal framework includono `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-424">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="42f6f-425">Nei valori non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="42f6f-425">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="42f6f-426">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="42f6f-426">ConfigureHostConfiguration</span></span>

<span data-ttu-id="42f6f-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> usa un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per creare un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfiguration> per l'host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="42f6f-428">La configurazione dell'host viene usata per inizializzare l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> da usare nel processo di compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-428">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="42f6f-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="42f6f-430">L'host usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="42f6f-430">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="42f6f-431">Nessun provider è incluso per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="42f6f-431">No providers are included by default.</span></span> <span data-ttu-id="42f6f-432">È necessario specificare in modo esplicito tutti i provider di configurazione richiesti dall'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, inclusi:</span><span class="sxs-lookup"><span data-stu-id="42f6f-432">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="42f6f-433">Configurazione dei file (ad esempio, da un file *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="42f6f-433">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="42f6f-434">Configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-434">Environment variable configuration.</span></span>
* <span data-ttu-id="42f6f-435">Configurazione degli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="42f6f-435">Command-line argument configuration.</span></span>
* <span data-ttu-id="42f6f-436">Tutti gli altri provider di configurazione necessari.</span><span class="sxs-lookup"><span data-stu-id="42f6f-436">Any other required configuration providers.</span></span>

<span data-ttu-id="42f6f-437">La configurazione file dell'host viene abilitata specificando il percorso di base dell'app con `SetBasePath` seguito da una chiamata a uno dei [provider di configurazione dei file](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="42f6f-437">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="42f6f-438">L'app di esempio usa un file JSON, *hostsettings.json*, e chiama <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> per utilizzare le impostazioni di configurazione host del file.</span><span class="sxs-lookup"><span data-stu-id="42f6f-438">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="42f6f-439">Per aggiungere la [configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider) dell'host, chiamare <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sul generatore di host.</span><span class="sxs-lookup"><span data-stu-id="42f6f-439">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="42f6f-440">`AddEnvironmentVariables` accetta un prefisso facoltativo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-440">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="42f6f-441">L'app di esempio usa un prefisso di `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-441">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="42f6f-442">Il prefisso viene rimosso quando vengono lette le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-442">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="42f6f-443">Quando viene configurato l'host dell'app di esempio, il valore della variabile di ambiente per `PREFIX_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.</span><span class="sxs-lookup"><span data-stu-id="42f6f-443">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="42f6f-444">Durante lo sviluppo quando si usa [Visual Studio](https://visualstudio.microsoft.com) o si esegue un'app con `dotnet run`, le variabili di ambiente possono essere impostate nel file *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-444">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="42f6f-445">Durante lo sviluppo in [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="42f6f-445">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="42f6f-446">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-446">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="42f6f-447">La [configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) viene aggiunta chiamando <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-447">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="42f6f-448">La configurazione della riga di comando viene aggiunta per ultima per consentire agli argomenti della riga di comando di eseguire l'override della configurazione fornita dai provider di configurazione precedenti.</span><span class="sxs-lookup"><span data-stu-id="42f6f-448">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="42f6f-449">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="42f6f-449">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="42f6f-450">È possibile fornire una configurazione aggiuntiva con le chiavi [applicationName](#application-key-name) e [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="42f6f-450">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="42f6f-451">Configurazione di esempio di `HostBuilder` che usa <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="42f6f-451">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="42f6f-452">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="42f6f-452">ConfigureAppConfiguration</span></span>

<span data-ttu-id="42f6f-453">La configurazione dell'app viene creata chiamando <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sull'implementazione <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="42f6f-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> usa un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per creare un'interfaccia <xref:Microsoft.Extensions.Configuration.IConfiguration> per l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="42f6f-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="42f6f-456">L'app usa l'ultima opzione che imposta un valore in una determinata chiave.</span><span class="sxs-lookup"><span data-stu-id="42f6f-456">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="42f6f-457">La configurazione creata da <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> è disponibile in [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) per le operazioni successive e in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-457">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="42f6f-458">La configurazione dell'app riceve automaticamente la configurazione dell'host fornita da [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="42f6f-458">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="42f6f-459">Configurazione app di esempio che usa <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="42f6f-459">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="42f6f-460">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="42f6f-460">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="42f6f-461">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="42f6f-461">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="42f6f-462">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="42f6f-462">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="42f6f-463">Per spostare i file delle impostazioni nella directory di output, specificare i file delle impostazioni come [elementi di progetto MSBuild](/visualstudio/msbuild/common-msbuild-project-items) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-463">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="42f6f-464">L'app di esempio sposta i file delle impostazioni dell'app JSON e *hostsettings.json* con l'elemento `<Content>` seguente:</span><span class="sxs-lookup"><span data-stu-id="42f6f-464">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="42f6f-465">I metodi di estensione di configurazione, ad esempio <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> e <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, necessitano di altri pacchetti NuGet, ad esempio [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) e [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="42f6f-465">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="42f6f-466">A meno che l'app non usi il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), questi pacchetti devono essere aggiunti al progetto oltre al pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) di base.</span><span class="sxs-lookup"><span data-stu-id="42f6f-466">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="42f6f-467">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-467">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="42f6f-468">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="42f6f-468">ConfigureServices</span></span>

<span data-ttu-id="42f6f-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> aggiunge servizi al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="42f6f-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="42f6f-471">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-471">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="42f6f-472">Per altre informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-472">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="42f6f-473">L'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa il metodo di estensione `AddHostedService` per aggiungere all'app un servizio per gli eventi di durata (`LifetimeEventsHostedService`) e un'attività in background con timeout (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="42f6f-473">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="42f6f-474">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="42f6f-474">ConfigureLogging</span></span>

<span data-ttu-id="42f6f-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> aggiunge un delegato per configurare l'interfaccia <xref:Microsoft.Extensions.Logging.ILoggingBuilder> fornita.</span><span class="sxs-lookup"><span data-stu-id="42f6f-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="42f6f-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="42f6f-477">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-477">UseConsoleLifetime</span></span>

<span data-ttu-id="42f6f-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> resta in ascolto di CTRL+C/SIGINT o SIGTERM e chiama <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> per avviare il processo di arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="42f6f-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="42f6f-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="42f6f-480"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> è preregistrata come implementazione di durata predefinita.</span><span class="sxs-lookup"><span data-stu-id="42f6f-480"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="42f6f-481">Viene usata l'ultima durata registrata.</span><span class="sxs-lookup"><span data-stu-id="42f6f-481">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="42f6f-482">Configurazione contenitore</span><span class="sxs-lookup"><span data-stu-id="42f6f-482">Container configuration</span></span>

<span data-ttu-id="42f6f-483">Per supportare l'inserimento di altri contenitori, l'host può accettare un elemento <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-483">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="42f6f-484">La disponibilità di una factory non fa parte della registrazione del contenitore DI, ma è una funzionalità intrinseca dell'host usata per creare il contenitore DI concreto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-484">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="42f6f-485">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) esegue l'override della factory predefinita usata per la creazione del provider di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-485">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="42f6f-486">La configurazione del contenitore personalizzato è gestita dal metodo <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-486">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="42f6f-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> offre un'esperienza fortemente tipizzata per la configurazione del contenitore sulla base dell'API host sottostante.</span><span class="sxs-lookup"><span data-stu-id="42f6f-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="42f6f-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="42f6f-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="42f6f-489">Creare un contenitore di servizi per l'app:</span><span class="sxs-lookup"><span data-stu-id="42f6f-489">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="42f6f-490">Specificare una factory per il contenitore di servizi:</span><span class="sxs-lookup"><span data-stu-id="42f6f-490">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="42f6f-491">Usare la factory e configurare il contenitore di servizi personalizzato per l'app:</span><span class="sxs-lookup"><span data-stu-id="42f6f-491">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="42f6f-492">Estendibilità</span><span class="sxs-lookup"><span data-stu-id="42f6f-492">Extensibility</span></span>

<span data-ttu-id="42f6f-493">L'estensibilità host viene eseguita con i metodi di estensione su <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-493">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="42f6f-494">L'esempio seguente illustra come un metodo di estensione estende un'implementazione <xref:Microsoft.Extensions.Hosting.IHostBuilder> con l'esempio [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) dimostrato in <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-494">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="42f6f-495">Un'app stabilisce il metodo di estensione `UseHostedService` per registrare il servizio ospitato passato in `T`:</span><span class="sxs-lookup"><span data-stu-id="42f6f-495">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="42f6f-496">Gestire l'host</span><span class="sxs-lookup"><span data-stu-id="42f6f-496">Manage the host</span></span>

<span data-ttu-id="42f6f-497">L'implementazione <xref:Microsoft.Extensions.Hosting.IHost> è responsabile dell'avvio e arresto delle implementazioni <xref:Microsoft.Extensions.Hosting.IHostedService> che sono registrate nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="42f6f-497">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="42f6f-498">Esegui</span><span class="sxs-lookup"><span data-stu-id="42f6f-498">Run</span></span>

<span data-ttu-id="42f6f-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> esegue l'app e blocca il thread di chiamata fino all'arresto dell'host:</span><span class="sxs-lookup"><span data-stu-id="42f6f-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="42f6f-500">RunAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-500">RunAsync</span></span>

<span data-ttu-id="42f6f-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> esegue l'app e restituisce un elemento <xref:System.Threading.Tasks.Task> che viene completato quando viene attivato il token di annullamento o l'arresto:</span><span class="sxs-lookup"><span data-stu-id="42f6f-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="42f6f-502">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-502">RunConsoleAsync</span></span>

<span data-ttu-id="42f6f-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> abilita il supporto della console, compila e avvia l'host e resta in ascolto di CTRL+C/SIGINT o SIGTERM per eseguire l'arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="42f6f-504">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-504">Start and StopAsync</span></span>

<span data-ttu-id="42f6f-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> avvia l'host in modo sincrono.</span><span class="sxs-lookup"><span data-stu-id="42f6f-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="42f6f-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> prova ad arrestare l'host entro il timeout specificato.</span><span class="sxs-lookup"><span data-stu-id="42f6f-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="42f6f-507">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-507">StartAsync and StopAsync</span></span>

<span data-ttu-id="42f6f-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> avvia l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="42f6f-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> arresta l'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="42f6f-510">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="42f6f-510">WaitForShutdown</span></span>

<span data-ttu-id="42f6f-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> viene attivato tramite <xref:Microsoft.Extensions.Hosting.IHostLifetime>, ad esempio <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (in ascolto di CTRL+C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="42f6f-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="42f6f-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> chiama <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="42f6f-513">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="42f6f-513">WaitForShutdownAsync</span></span>

<span data-ttu-id="42f6f-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> restituisce un elemento <xref:System.Threading.Tasks.Task> che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="42f6f-515">Controllo esterno</span><span class="sxs-lookup"><span data-stu-id="42f6f-515">External control</span></span>

<span data-ttu-id="42f6f-516">Il controllo esterno dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:</span><span class="sxs-lookup"><span data-stu-id="42f6f-516">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="42f6f-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> viene chiamato all'avvio di <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, che attende il completamento prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="42f6f-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="42f6f-518">Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.</span><span class="sxs-lookup"><span data-stu-id="42f6f-518">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="42f6f-519">Interfaccia IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="42f6f-519">IHostingEnvironment interface</span></span>

<span data-ttu-id="42f6f-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> include informazioni sull'ambiente di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="42f6f-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="42f6f-521">Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> per poterne usare le proprietà e i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="42f6f-521">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="42f6f-522">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="42f6f-522">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="42f6f-523">Interfaccia IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="42f6f-523">IApplicationLifetime interface</span></span>

<span data-ttu-id="42f6f-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> consente attività post-avvio e di arresto, incluse le richieste di arresto normale.</span><span class="sxs-lookup"><span data-stu-id="42f6f-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="42f6f-525">Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi <xref:System.Action> che definiscono gli eventi di avvio e arresto.</span><span class="sxs-lookup"><span data-stu-id="42f6f-525">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="42f6f-526">Token di annullamento</span><span class="sxs-lookup"><span data-stu-id="42f6f-526">Cancellation Token</span></span> | <span data-ttu-id="42f6f-527">Attivato quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="42f6f-527">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="42f6f-528">L'host è stato completamente avviato.</span><span class="sxs-lookup"><span data-stu-id="42f6f-528">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="42f6f-529">L'host sta completando un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="42f6f-529">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="42f6f-530">Tutte le richieste devono essere elaborate.</span><span class="sxs-lookup"><span data-stu-id="42f6f-530">All requests should be processed.</span></span> <span data-ttu-id="42f6f-531">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="42f6f-531">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="42f6f-532">L'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="42f6f-532">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="42f6f-533">È possibile che le richieste siano ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="42f6f-533">Requests may still be processing.</span></span> <span data-ttu-id="42f6f-534">L'arresto è bloccato fino al completamento di questo evento.</span><span class="sxs-lookup"><span data-stu-id="42f6f-534">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="42f6f-535">Inserire tramite costruttore il servizio <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> in qualsiasi classe.</span><span class="sxs-lookup"><span data-stu-id="42f6f-535">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="42f6f-536">L'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa l'inserimento di costruttori in una classe `LifetimeEventsHostedService` (implementazione <xref:Microsoft.Extensions.Hosting.IHostedService>) per registrare gli eventi.</span><span class="sxs-lookup"><span data-stu-id="42f6f-536">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="42f6f-537">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="42f6f-537">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="42f6f-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> richiede la chiusura dell'applicazione corrente.</span><span class="sxs-lookup"><span data-stu-id="42f6f-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="42f6f-539">La classe seguente usa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:</span><span class="sxs-lookup"><span data-stu-id="42f6f-539">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="42f6f-540">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42f6f-540">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
