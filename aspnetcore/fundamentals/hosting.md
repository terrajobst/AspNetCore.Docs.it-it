---
title: Hosting in ASP.NET Core | Documenti Microsoft
author: ardalis
description: Introduzione agli host web in ASP.NET Core.
keywords: ASP.NET Core, host web, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="47b6b-104">Introduzione all'hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47b6b-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="47b6b-105">Da [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="47b6b-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="47b6b-106">Per eseguire un'applicazione ASP.NET di base, è necessario configurare e avviare un host utilizzando `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="47b6b-107">Che cos'è un Host?</span><span class="sxs-lookup"><span data-stu-id="47b6b-107">What is a Host?</span></span>

<span data-ttu-id="47b6b-108">Le applicazioni ASP.NET Core richiedono un *host* in cui si desidera eseguire.</span><span class="sxs-lookup"><span data-stu-id="47b6b-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="47b6b-109">Un host deve implementare il `IWebHost` interfaccia, che espone le raccolte di funzionalità e i servizi, e un `Start` metodo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="47b6b-110">L'host è in genere creato utilizzando un'istanza di un `WebHostBuilder`, che crea e restituisce un `WebHost` istanza.</span><span class="sxs-lookup"><span data-stu-id="47b6b-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="47b6b-111">Il `WebHost` fa riferimento al server che gestirà le richieste.</span><span class="sxs-lookup"><span data-stu-id="47b6b-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="47b6b-112">Altre informazioni, vedere [server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="47b6b-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="47b6b-113">Che cos'è la differenza tra un host e un server?</span><span class="sxs-lookup"><span data-stu-id="47b6b-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="47b6b-114">L'host è responsabile della gestione di avvio e la durata dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="47b6b-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="47b6b-115">Il server è responsabile per l'accettazione di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="47b6b-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="47b6b-116">Parte di responsabilità dell'host include la verifica che i servizi dell'applicazione e il server è disponibile e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="47b6b-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="47b6b-117">È possibile considerare l'host come un wrapper per il server.</span><span class="sxs-lookup"><span data-stu-id="47b6b-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="47b6b-118">L'host è configurato per utilizzare un server specifico. il server non è a conoscenza del relativo host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="47b6b-119">Impostazione di un Host</span><span class="sxs-lookup"><span data-stu-id="47b6b-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="47b6b-120">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="47b6b-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="47b6b-121">Creare un host utilizzando un'istanza di `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="47b6b-122">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione: `public static void Main` (che nei modelli di progetto si trova un *Program.cs* file).</span><span class="sxs-lookup"><span data-stu-id="47b6b-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="47b6b-123">Una tipica *Program.cs*, come illustrato di seguito viene illustrato come utilizzare un `WebHostBuilder` per compilare un host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="47b6b-124">[!code-csharp[Principale](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="47b6b-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="47b6b-125">Il `WebHostBuilder` è responsabile della creazione dell'host che verrà bootstrap del server per l'app.</span><span class="sxs-lookup"><span data-stu-id="47b6b-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="47b6b-126">`WebHostBuilder`è necessario fornire un server che implementa `IServer` (`UseKestrel` nel codice precedente).</span><span class="sxs-lookup"><span data-stu-id="47b6b-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="47b6b-127">`UseKestrel`Specifica che il server Kestrel verrà usato dall'app.</span><span class="sxs-lookup"><span data-stu-id="47b6b-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="47b6b-128">Il server *contenuto radice* determina in cui cercare i file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="47b6b-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="47b6b-129">La radice di contenuto predefinito è la cartella da cui viene eseguita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="47b6b-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="47b6b-130">Specifica di `Directory.GetCurrentDirectory` come radice del contenuto utilizzeranno cartella radice del progetto web come radice del contenuto dell'app quando l'applicazione viene avviata da questa cartella (ad esempio, la chiamata `dotnet run` dalla cartella del progetto web).</span><span class="sxs-lookup"><span data-stu-id="47b6b-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="47b6b-131">Questa è l'impostazione predefinita utilizzata in Visual Studio e `dotnet new` modelli.</span><span class="sxs-lookup"><span data-stu-id="47b6b-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="47b6b-132">Per utilizzare IIS come un proxy inverso, chiamare `UseIISIntegration` come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="47b6b-133">Si noti che `UseIISIntegration` non configurare un *server*, ad esempio `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="47b6b-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="47b6b-134">Per utilizzare IIS con ASP.NET Core, è necessario specificare sia `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="47b6b-135">`UseKestrel`Crea il server web e ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="47b6b-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="47b6b-136">`UseIISIntegration`esamina le variabili di ambiente utilizzate da IIS/IIS Express e configura le impostazioni, ad esempio la porta per l'ascolto e le intestazioni da usare.</span><span class="sxs-lookup"><span data-stu-id="47b6b-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="47b6b-137">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="47b6b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="47b6b-138">Creare un host utilizzando un'istanza di `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="47b6b-139">Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione: `public static void Main` (che nei modelli di progetto si trova un *Program.cs* file).</span><span class="sxs-lookup"><span data-stu-id="47b6b-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="47b6b-140">Una tipica *Program.cs*, come illustrato di seguito, le chiamate `CreateDefaultbuilder` per compilare un host:</span><span class="sxs-lookup"><span data-stu-id="47b6b-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="47b6b-141">[!code-csharp[Principale](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="47b6b-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="47b6b-142">`CreateDefaultbuilder`Crea un'istanza di `WebHostBuilder` per compilare l'host che avvia il server per l'app.</span><span class="sxs-lookup"><span data-stu-id="47b6b-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="47b6b-143">L'host richiede un [server che implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="47b6b-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="47b6b-144">Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` utilizzare Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="47b6b-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="47b6b-145">`CreateDefaultbuilder`esegue attività di configurazione oltre alla configurazione Kestrel come server web:</span><span class="sxs-lookup"><span data-stu-id="47b6b-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="47b6b-146">Imposta la radice del contenuto `Directory.GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="47b6b-147">Configurazione di caricamento da:</span><span class="sxs-lookup"><span data-stu-id="47b6b-147">Loads configuration from:</span></span>
  * <span data-ttu-id="47b6b-148">*appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="47b6b-148">*appsettings.json*</span></span>
  * <span data-ttu-id="47b6b-149">*appSettings. \<EnvironmentName > JSON*.</span><span class="sxs-lookup"><span data-stu-id="47b6b-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="47b6b-150">informazioni riservate dell'utente quando viene eseguita l'app nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="47b6b-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="47b6b-151">variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="47b6b-151">environment variables</span></span>
  * <span data-ttu-id="47b6b-152">argomenti della riga di comando fornito</span><span class="sxs-lookup"><span data-stu-id="47b6b-152">supplied command line args</span></span>
* <span data-ttu-id="47b6b-153">Configura la registrazione per l'output di console ed eseguire il debug, con le regole specificate in una sezione di configurazione della registrazione di filtro.</span><span class="sxs-lookup"><span data-stu-id="47b6b-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="47b6b-154">Consente l'integrazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="47b6b-154">Enables IIS integration.</span></span>
* <span data-ttu-id="47b6b-155">Aggiunge la pagina di eccezione sviluppatore quando viene eseguita l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="47b6b-156">Il server *contenuto radice* determina in cui cercare i file di contenuto, ad esempio i file di visualizzazione MVC.</span><span class="sxs-lookup"><span data-stu-id="47b6b-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="47b6b-157">La radice di contenuto predefinito è la cartella da cui viene eseguita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="47b6b-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="47b6b-158">Specifica di `Directory.GetCurrentDirectory` come radice del contenuto utilizzeranno cartella radice del progetto web come radice del contenuto dell'app quando l'applicazione viene avviata da questa cartella (ad esempio, la chiamata `dotnet run` dalla cartella del progetto web).</span><span class="sxs-lookup"><span data-stu-id="47b6b-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="47b6b-159">Questa è l'impostazione predefinita utilizzata in Visual Studio e `dotnet new` modelli.</span><span class="sxs-lookup"><span data-stu-id="47b6b-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="47b6b-160">Quando si utilizza IIS come un proxy inverso, ASP.NET Core chiama automaticamente `UseIISIntegration` come parte della creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="47b6b-161">Per ulteriori informazioni, vedere [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="47b6b-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="47b6b-162">Si noti che `UseIISIntegration` non configurare un *server*, ad esempio `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="47b6b-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="47b6b-163">`UseKestrel`Crea il server web e ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="47b6b-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="47b6b-164">`UseIISIntegration`esamina le variabili di ambiente utilizzate da IIS/IIS Express e configura le impostazioni, ad esempio la porta per l'ascolto e le intestazioni da usare.</span><span class="sxs-lookup"><span data-stu-id="47b6b-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="47b6b-165">Un'implementazione minima di configurazione di un host (e un'applicazione ASP.NET Core) include solo un server e una configurazione della pipeline di richieste dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="47b6b-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="47b6b-166">Quando si configura un host, è possibile fornire `Configure` e `ConfigureServices` metodi, anziché o oltre a specificare un `Startup` classe (che deve anche definire questi metodi, vedere [avvio dell'applicazione](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="47b6b-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="47b6b-167">Più chiamate al metodo `ConfigureServices` aggiungerà uno a altro; le chiamate a `Configure` o `UseStartup` sostituiranno le impostazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="47b6b-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="47b6b-168">Configurazione di un Host</span><span class="sxs-lookup"><span data-stu-id="47b6b-168">Configuring a Host</span></span>

<span data-ttu-id="47b6b-169">Il `WebHostBuilder` fornisce metodi per impostare la maggior parte dei valori di configurazione disponibili per l'host, che può essere impostata anche utilizzando direttamente `UseSetting` e la chiave associata.</span><span class="sxs-lookup"><span data-stu-id="47b6b-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="47b6b-170">Valori di configurazione di host</span><span class="sxs-lookup"><span data-stu-id="47b6b-170">Host Configuration Values</span></span>

<span data-ttu-id="47b6b-171">**Acquisire gli errori di avvio**`bool`</span><span class="sxs-lookup"><span data-stu-id="47b6b-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="47b6b-172">Chiave: `captureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="47b6b-173">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-173">Defaults to `false`.</span></span> <span data-ttu-id="47b6b-174">Quando `false`, errori durante il risultato di avvio nell'host di chiusura.</span><span class="sxs-lookup"><span data-stu-id="47b6b-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="47b6b-175">Quando `true`, l'host consente di acquisire tutte le eccezioni dalla `Startup` classe e si tenta di avviare il server.</span><span class="sxs-lookup"><span data-stu-id="47b6b-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="47b6b-176">Verrà visualizzata una pagina di errore (generico o di dettaglio, in base all'impostazione, gli errori dettagliati seguito) per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="47b6b-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="47b6b-177">Impostare l'utilizzo di `CaptureStartupErrors` metodo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="47b6b-178">Nota: Quando l'app viene eseguita con Kestrel e IIS, il comportamento predefinito è per acquisire gli errori di avvio.</span><span class="sxs-lookup"><span data-stu-id="47b6b-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="47b6b-179">**Contenuto radice**`string`</span><span class="sxs-lookup"><span data-stu-id="47b6b-179">**Content Root** `string`</span></span>

<span data-ttu-id="47b6b-180">Chiave: `contentRoot`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-180">Key: `contentRoot`.</span></span> <span data-ttu-id="47b6b-181">Valore predefinito è la cartella in cui si trova l'assembly dell'applicazione (per Kestrel; IIS utilizzerà la radice del progetto web per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="47b6b-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="47b6b-182">Questa impostazione determina in ASP.NET Core verrà avviata la ricerca di file di contenuto, ad esempio le visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="47b6b-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="47b6b-183">Utilizzato anche come percorso di base per il [impostazione radice Web](#web-root-setting).</span><span class="sxs-lookup"><span data-stu-id="47b6b-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="47b6b-184">Impostare l'utilizzo di `UseContentRoot` metodo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="47b6b-185">Percorso deve essere presente o non sarà possibile avviare host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="47b6b-186">**Errori dettagliati**`bool`</span><span class="sxs-lookup"><span data-stu-id="47b6b-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="47b6b-187">Chiave: `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="47b6b-188">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-188">Defaults to `false`.</span></span> <span data-ttu-id="47b6b-189">Quando `true` (o quando cui ambiente è impostato su "Development"), l'app sarà visualizzati i dettagli delle eccezioni di avvio, anziché una pagina di errore generico.</span><span class="sxs-lookup"><span data-stu-id="47b6b-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="47b6b-190">Impostare utilizzando `UseSetting`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="47b6b-191">Quando gli errori dettagliati è impostato su `false` e acquisire gli errori di avvio è `true`, viene visualizzata una pagina di errore generico in risposta a ogni richiesta al server.</span><span class="sxs-lookup"><span data-stu-id="47b6b-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![Pagina di errore generico](hosting/_static/generic-error-page.png)

<span data-ttu-id="47b6b-193">Quando gli errori dettagliati è impostato su `true` e acquisire gli errori di avvio è `true`, viene visualizzata una pagina di errore dettagliati in risposta a ogni richiesta al server.</span><span class="sxs-lookup"><span data-stu-id="47b6b-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![Pagina di errore dettagliato](hosting/_static/detailed-error-page.png)

<span data-ttu-id="47b6b-195">**Ambiente**`string`</span><span class="sxs-lookup"><span data-stu-id="47b6b-195">**Environment** `string`</span></span>

<span data-ttu-id="47b6b-196">Chiave: `environment`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-196">Key: `environment`.</span></span> <span data-ttu-id="47b6b-197">Il valore predefinito "Produzione".</span><span class="sxs-lookup"><span data-stu-id="47b6b-197">Defaults to "Production".</span></span> <span data-ttu-id="47b6b-198">Può essere impostata su qualsiasi valore.</span><span class="sxs-lookup"><span data-stu-id="47b6b-198">May be set to any value.</span></span> <span data-ttu-id="47b6b-199">I valori definiti dal framework includono "Sviluppo", "Gestione temporanea" e "Produzione".</span><span class="sxs-lookup"><span data-stu-id="47b6b-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="47b6b-200">Valori non fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="47b6b-200">Values are not case sensitive.</span></span> <span data-ttu-id="47b6b-201">Vedere [utilizzo di più ambienti](environments.md).</span><span class="sxs-lookup"><span data-stu-id="47b6b-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="47b6b-202">Impostare l'utilizzo di `UseEnvironment` metodo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="47b6b-203">Per impostazione predefinita, l'ambiente viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="47b6b-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="47b6b-204">Quando si utilizza Visual Studio, le variabili di ambiente possono essere impostate *launchSettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="47b6b-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="47b6b-205">**Gli URL del server**`string`</span><span class="sxs-lookup"><span data-stu-id="47b6b-205">**Server URLs** `string`</span></span>

<span data-ttu-id="47b6b-206">Chiave: `urls`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-206">Key: `urls`.</span></span> <span data-ttu-id="47b6b-207">Impostare un punto e virgola (;) separati prefissi di elenco di URL per cui il server deve rispondere.</span><span class="sxs-lookup"><span data-stu-id="47b6b-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="47b6b-208">Ad esempio `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="47b6b-209">Il nome di dominio o l'host può essere sostituito con "\*" per indicare il server deve restare in attesa delle richieste su qualsiasi indirizzo IP o host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000` o `https://*:5001`).</span><span class="sxs-lookup"><span data-stu-id="47b6b-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="47b6b-210">Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL.</span><span class="sxs-lookup"><span data-stu-id="47b6b-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="47b6b-211">I prefissi sono interpretati dal server configurato. i formati supportati variano tra i server.</span><span class="sxs-lookup"><span data-stu-id="47b6b-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="47b6b-212">In ASP.NET 2.0 Core Kestrel ha la propria configurazione di endpoint API e non supporta `https://` nel `urls` stringa.</span><span class="sxs-lookup"><span data-stu-id="47b6b-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="47b6b-213">Per ulteriori informazioni, vedere [Introduzione a Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="47b6b-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="47b6b-214">**Assembly di avvio**`string`</span><span class="sxs-lookup"><span data-stu-id="47b6b-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="47b6b-215">Chiave: `startupAssembly`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="47b6b-216">Determina l'assembly per la ricerca di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="47b6b-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="47b6b-217">Impostare l'utilizzo di `UseStartup` metodo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="47b6b-218">Può invece fare riferimento a un tipo specifico usando `WebHostBuilder.UseStartup<StartupType>`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="47b6b-219">Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="47b6b-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="47b6b-220">**Web radice**`string`</span><span class="sxs-lookup"><span data-stu-id="47b6b-220">**Web Root** `string`</span></span>

<span data-ttu-id="47b6b-221">Chiave: `webroot`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-221">Key: `webroot`.</span></span> <span data-ttu-id="47b6b-222">Se non è specificato il valore predefinito è `(Content Root Path)\wwwroot`, se presente.</span><span class="sxs-lookup"><span data-stu-id="47b6b-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="47b6b-223">Se questo percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="47b6b-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="47b6b-224">Impostare utilizzando `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="47b6b-225">Si esegue l'override di configurazione</span><span class="sxs-lookup"><span data-stu-id="47b6b-225">Overriding Configuration</span></span>

<span data-ttu-id="47b6b-226">Utilizzare [configurazione](configuration.md) per impostare i valori di configurazione da utilizzare dall'host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="47b6b-227">Questi valori possono essere successivamente sottoposto a override.</span><span class="sxs-lookup"><span data-stu-id="47b6b-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="47b6b-228">Viene specificato utilizzando `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="47b6b-229">Nell'esempio precedente, possono essere passati gli argomenti della riga di comando per configurare l'host o, facoltativamente, è possibile specificare le impostazioni di configurazione in un *hosting.json* file.</span><span class="sxs-lookup"><span data-stu-id="47b6b-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="47b6b-230">Per specificare l'host di eseguire in un URL specifico, è possibile passare il valore desiderato da un prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="47b6b-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="47b6b-231">Il `Run` metodo avvia l'app web e blocca il thread chiamante finché l'host viene arrestato.</span><span class="sxs-lookup"><span data-stu-id="47b6b-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="47b6b-232">È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:</span><span class="sxs-lookup"><span data-stu-id="47b6b-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="47b6b-233">Passare un elenco di URL per il `Start` (metodo) e sarà in ascolto l'URL specificato:</span><span class="sxs-lookup"><span data-stu-id="47b6b-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

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

<span data-ttu-id="47b6b-234">I formati di URL validi qui dipendono dal server in uso.</span><span class="sxs-lookup"><span data-stu-id="47b6b-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="47b6b-235">Per ulteriori informazioni, vedere [gli URL del Server](#server-urls) più indietro in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="47b6b-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="47b6b-236">Il `UseConfiguration` non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="47b6b-237">Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="47b6b-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="47b6b-238">Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="47b6b-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="47b6b-239">La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="47b6b-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="47b6b-240">Questo problema verrà risolto in una delle prossime versioni.</span><span class="sxs-lookup"><span data-stu-id="47b6b-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="47b6b-241">Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="47b6b-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="47b6b-242">Ordine di priorità</span><span class="sxs-lookup"><span data-stu-id="47b6b-242">Ordering Importance</span></span>

<span data-ttu-id="47b6b-243">`WebHostBuilder`le impostazioni vengono lette prima da alcune variabili di ambiente, se impostata.</span><span class="sxs-lookup"><span data-stu-id="47b6b-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="47b6b-244">Queste variabili di ambiente è necessario utilizzare il formato `ASPNETCORE_{configurationKey}`, ad esempio per impostare gli URL di server resterà in ascolto su per impostazione predefinita, è necessario impostare `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="47b6b-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="47b6b-245">È possibile sostituire qualsiasi di questi valori di variabili di ambiente specificando una configurazione (con `UseConfiguration`) o impostando il valore in modo esplicito (mediante `UseUrls` per l'istanza).</span><span class="sxs-lookup"><span data-stu-id="47b6b-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="47b6b-246">L'host utilizzerà indipendentemente dall'opzione imposta il valore dell'ultima.</span><span class="sxs-lookup"><span data-stu-id="47b6b-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="47b6b-247">Per questo motivo, `UseIISIntegration` devono essere visualizzate dopo `UseUrls`, perché l'URL che sostituisce con una condizione in modo dinamico da IIS.</span><span class="sxs-lookup"><span data-stu-id="47b6b-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="47b6b-248">Se si desidera impostare l'URL predefinito a un valore a livello di codice, ma consentono di eseguire l'override con la configurazione, è possibile configurare l'host come segue:</span><span class="sxs-lookup"><span data-stu-id="47b6b-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="47b6b-249">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="47b6b-249">Additional resources</span></span>

* [<span data-ttu-id="47b6b-250">Pubblicare in Windows tramite IIS</span><span class="sxs-lookup"><span data-stu-id="47b6b-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="47b6b-251">Pubblicare in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="47b6b-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="47b6b-252">Pubblicare in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="47b6b-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="47b6b-253">Host in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="47b6b-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

