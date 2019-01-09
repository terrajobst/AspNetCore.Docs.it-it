---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Scoprire i concetti fondamentali per la compilazione di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: fundamentals/index
ms.openlocfilehash: a56beebd796448705c7b84f47699e9739f451419
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099234"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="ca3c6-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca3c6-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="ca3c6-104">Un'app ASP.NET Core è un'app console che crea un server Web nel suo metodo `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="ca3c6-105">Il metodo `Main` è il *punto di ingresso gestito* nell'app:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="ca3c6-106">Host .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-106">The .NET Core Host:</span></span>

* <span data-ttu-id="ca3c6-107">Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="ca3c6-108">Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="ca3c6-109">Il metodo `Main` richiama [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host Web.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="ca3c6-110">Il generatore dispone di metodi che definiscono un server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="ca3c6-111">Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="ca3c6-112">L'host Web di ASP.NET Core tenta l'esecuzione su [Internet Information Services (IIS)](https://www.iis.net/), se disponibile.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="ca3c6-113">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="ca3c6-114">`UseStartup` viene spiegato dettagliatamente nella sezione [Avvio](#startup).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="ca3c6-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="ca3c6-116">Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="ca3c6-117">I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="ca3c6-118">Host .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-118">The .NET Core Host:</span></span>

* <span data-ttu-id="ca3c6-119">Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="ca3c6-120">Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="ca3c6-121">Il metodo `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host app Web.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="ca3c6-122">Il generatore dispone di metodi che definiscono il server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="ca3c6-123">Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="ca3c6-124">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="ca3c6-125">`UseStartup` viene spiegato dettagliatamente nella sezione [Avvio](#startup).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="ca3c6-126">`WebHostBuilder` offre molti metodi facoltativi, incluso <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per l'hosting in IIS e IIS Express e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="ca3c6-127">I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="ca3c6-128">Avvio</span><span class="sxs-lookup"><span data-stu-id="ca3c6-128">Startup</span></span>

<span data-ttu-id="ca3c6-129">Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="ca3c6-130">La classe `Startup` è la posizione in cui vengono configurati eventuali servizi richiesti dall'app e viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-130">The `Startup` class is where any services required by the app are configured and the request handling pipeline is defined.</span></span> <span data-ttu-id="ca3c6-131">La classe `Startup` deve essere pubblica e contiene in genere i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-131">The `Startup` class must be public and usually contains the following methods.</span></span> <span data-ttu-id="ca3c6-132">`Startup.ConfigureServices` è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-132">`Startup.ConfigureServices` is optional.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="ca3c6-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="ca3c6-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definisce il [middleware](xref:fundamentals/middleware/index) chiamato nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="ca3c6-135">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-135">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="ca3c6-136">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="ca3c6-136">Content root</span></span>

<span data-ttu-id="ca3c6-137">La radice del contenuto è il percorso di base per i contenuti usati dall'app, ad esempio [Razor Pages](xref:razor-pages/index), visualizzazioni MVC e asset statici.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-137">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="ca3c6-138">Per impostazione predefinita, la posizione della radice del contenuto è uguale al percorso di base dell'app per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-138">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="ca3c6-139">Radice Web (webroot)</span><span class="sxs-lookup"><span data-stu-id="ca3c6-139">Web root (webroot)</span></span>

<span data-ttu-id="ca3c6-140">La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-140">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="ca3c6-141">Per impostazione predefinita *wwwroot* è la radice Web.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-141">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="ca3c6-142">Per i file Razor (*. cshtml*), il carattere tilde `~/` punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-142">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="ca3c6-143">I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-143">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="ca3c6-144">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="ca3c6-144">Dependency injection (services)</span></span>

<span data-ttu-id="ca3c6-145">Un *servizio* è un componente destinato a un utilizzo comune in un'app.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-145">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="ca3c6-146">I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-146">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ca3c6-147">ASP.NET Core include un'inversione nativa del contenitore del controllo (IoC) che supporta l'[inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-147">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="ca3c6-148">Se si vuole, è possibile sostituire il contenitore predefinito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-148">You can replace the default container if you wish.</span></span> <span data-ttu-id="ca3c6-149">Oltre al vantaggio dell'[accoppiamento debole](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), l'iniezione delle dipendenze rende disponibili i servizi in tutta l'app, ad esempio la [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-149">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="ca3c6-150">Per ulteriori informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-150">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="ca3c6-151">Middleware</span><span class="sxs-lookup"><span data-stu-id="ca3c6-151">Middleware</span></span>

<span data-ttu-id="ca3c6-152">In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-152">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="ca3c6-153">Il middleware ASP.NET Core esegue le operazioni asincrone su un `HttpContext`, quindi richiama il middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-153">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="ca3c6-154">Per convenzione viene aggiunto un componente middleware denominato "XYZ" alla pipeline richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-154">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="ca3c6-155">ASP.NET Core include un ampio set di middleware predefinito ed è possibile scrivere middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-155">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="ca3c6-156">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), che consente di disaccoppiare le app Web dai server Web, è supportato nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-156">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="ca3c6-157">Per altre informazioni, vedere <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-157">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="ca3c6-158">Inizializzare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="ca3c6-158">Initiate HTTP requests</span></span>

<span data-ttu-id="ca3c6-159"><xref:System.Net.Http.IHttpClientFactory> è disponibile per accedere alle istanze di <xref:System.Net.Http.HttpClient> per effettuare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-159"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="ca3c6-160">Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-160">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="ca3c6-161">Ambienti</span><span class="sxs-lookup"><span data-stu-id="ca3c6-161">Environments</span></span>

<span data-ttu-id="ca3c6-162">Gli ambienti, come ad esempio *Sviluppo* e *Produzione*, sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando una variabile di ambiente, un file di impostazioni e un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-162">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="ca3c6-163">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-163">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="ca3c6-164">Hosting</span><span class="sxs-lookup"><span data-stu-id="ca3c6-164">Hosting</span></span>

<span data-ttu-id="ca3c6-165">Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-165">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="ca3c6-166">Per ulteriori informazioni, vedere <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-166">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="ca3c6-167">Server</span><span class="sxs-lookup"><span data-stu-id="ca3c6-167">Servers</span></span>

<span data-ttu-id="ca3c6-168">Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-168">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="ca3c6-169">Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-169">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ca3c6-170">Windows</span><span class="sxs-lookup"><span data-stu-id="ca3c6-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="ca3c6-171">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-171">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ca3c6-172">Il server [Kestrel](xref:fundamentals/servers/kestrel) è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-172">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-173">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-173">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ca3c6-174">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-174">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="ca3c6-175">Il server HTTP IIS (`IISHttpServer`) è un [server in-process](xref:fundamentals/servers/index#in-process-hosting-model) per IIS.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-175">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="ca3c6-176">Il server [HTTP.sys](xref:fundamentals/servers/httpsys) è un server Web per ASP.NET Core in Windows.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-176">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ca3c6-177">macOS</span><span class="sxs-lookup"><span data-stu-id="ca3c6-177">macOS</span></span>](#tab/macos)

<span data-ttu-id="ca3c6-178">ASP.NET Core usa l'implementazione del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-178">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="ca3c6-179">Kestrel è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-179">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-180">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-180">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ca3c6-181">Linux</span><span class="sxs-lookup"><span data-stu-id="ca3c6-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="ca3c6-182">ASP.NET Core usa l'implementazione del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-182">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="ca3c6-183">Kestrel è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-183">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-184">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-184">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="ca3c6-185">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-185">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ca3c6-186">Windows</span><span class="sxs-lookup"><span data-stu-id="ca3c6-186">Windows</span></span>](#tab/windows)

<span data-ttu-id="ca3c6-187">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="ca3c6-187">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="ca3c6-188">Il server [Kestrel](xref:fundamentals/servers/kestrel) è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-188">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-189">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-189">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="ca3c6-190">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-190">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="ca3c6-191">Il server [HTTP.sys](xref:fundamentals/servers/httpsys) è un server Web per ASP.NET Core in Windows.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-191">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ca3c6-192">macOS</span><span class="sxs-lookup"><span data-stu-id="ca3c6-192">macOS</span></span>](#tab/macos)

<span data-ttu-id="ca3c6-193">ASP.NET Core usa l'implementazione del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-193">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="ca3c6-194">Kestrel è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-194">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-195">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-195">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ca3c6-196">Linux</span><span class="sxs-lookup"><span data-stu-id="ca3c6-196">Linux</span></span>](#tab/linux)

<span data-ttu-id="ca3c6-197">ASP.NET Core usa l'implementazione del server [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-197">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="ca3c6-198">Kestrel è un server Web multipiattaforma gestito.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-198">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="ca3c6-199">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ca3c6-199">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="ca3c6-200">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-200">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="ca3c6-201">Per ulteriori informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-201">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="ca3c6-202">Configurazione</span><span class="sxs-lookup"><span data-stu-id="ca3c6-202">Configuration</span></span>

<span data-ttu-id="ca3c6-203">ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-203">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="ca3c6-204">Il modello di configurazione non è basato su <xref:System.Configuration> o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-204">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="ca3c6-205">I provider di configurazione predefiniti supportano un'ampia gamma di formati di file (XML, JSON, INI), di variabili di ambiente e di argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-205">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="ca3c6-206">È anche possibile scrivere i propri provider di configurazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-206">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="ca3c6-207">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-207">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="ca3c6-208">Registrazione</span><span class="sxs-lookup"><span data-stu-id="ca3c6-208">Logging</span></span>

<span data-ttu-id="ca3c6-209">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-209">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="ca3c6-210">I provider predefiniti supportano l'invio dei registri a una o più destinazioni.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-210">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="ca3c6-211">È possibile usare framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-211">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="ca3c6-212">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="ca3c6-213">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="ca3c6-213">Error handling</span></span>

<span data-ttu-id="ca3c6-214">ASP.NET Core include scenari predefiniti per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statici e la gestione delle eccezioni all'avvio.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-214">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="ca3c6-215">Per ulteriori informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-215">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="ca3c6-216">Routing</span><span class="sxs-lookup"><span data-stu-id="ca3c6-216">Routing</span></span>

<span data-ttu-id="ca3c6-217">ASP.NET Core offre scenari per il routing delle richieste di app ai gestori di route.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-217">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="ca3c6-218">Per ulteriori informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-218">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="ca3c6-219">Attività in background</span><span class="sxs-lookup"><span data-stu-id="ca3c6-219">Background tasks</span></span>

<span data-ttu-id="ca3c6-220">Le attività in background vengono implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-220">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="ca3c6-221">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-221">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="ca3c6-222">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-222">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="ca3c6-223">Accedere a HttpContext</span><span class="sxs-lookup"><span data-stu-id="ca3c6-223">Access HttpContext</span></span>

<span data-ttu-id="ca3c6-224">`HttpContext` è disponibile automaticamente durante l'elaborazione delle richieste con Razor Pages e MVC.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-224">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="ca3c6-225">In circostanze in cui `HttpContext` non è immediatamente disponibile, è possibile accedere a `HttpContext` tramite l'interfaccia <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e la relativa implementazione predefinita, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-225">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="ca3c6-226">Per ulteriori informazioni, vedere <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="ca3c6-226">For more information, see <xref:fundamentals/httpcontext>.</span></span>
