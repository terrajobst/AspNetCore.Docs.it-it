---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Scoprire i concetti fondamentali per la compilazione di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: ab140051648c1640b3c4f382bfd8201c5c0c2039
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207472"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="5ff72-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ff72-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="5ff72-104">Un'app ASP.NET Core è un'app console che crea un server Web nel suo metodo `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="5ff72-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="5ff72-105">Il metodo `Main` è il *punto di ingresso gestito* nell'app:</span><span class="sxs-lookup"><span data-stu-id="5ff72-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="5ff72-106">Host .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5ff72-106">The .NET Core Host:</span></span>

* <span data-ttu-id="5ff72-107">Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="5ff72-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="5ff72-108">Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="5ff72-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="5ff72-109">Il metodo `Main` richiama [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host Web.</span><span class="sxs-lookup"><span data-stu-id="5ff72-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="5ff72-110">Il generatore dispone di metodi che definiscono il server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="5ff72-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="5ff72-111">Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5ff72-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="5ff72-112">L'host Web di ASP.NET Core tenta l'esecuzione in IIS, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="5ff72-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="5ff72-113">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="5ff72-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="5ff72-114">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="5ff72-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="5ff72-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="5ff72-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="5ff72-116">Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="5ff72-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="5ff72-117">I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ff72-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="5ff72-118">Host .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5ff72-118">The .NET Core Host:</span></span>

* <span data-ttu-id="5ff72-119">Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="5ff72-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="5ff72-120">Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="5ff72-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="5ff72-121">Il metodo `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host app Web.</span><span class="sxs-lookup"><span data-stu-id="5ff72-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="5ff72-122">Il generatore dispone di metodi che definiscono il server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="5ff72-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="5ff72-123">Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5ff72-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="5ff72-124">Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="5ff72-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="5ff72-125">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="5ff72-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="5ff72-126">`WebHostBuilder` offre molti metodi facoltativi, incluso <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per l'hosting in IIS e IIS Express e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="5ff72-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="5ff72-127">I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ff72-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="5ff72-128">Avvio</span><span class="sxs-lookup"><span data-stu-id="5ff72-128">Startup</span></span>

<span data-ttu-id="5ff72-129">Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:</span><span class="sxs-lookup"><span data-stu-id="5ff72-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="5ff72-130">Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="5ff72-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="5ff72-131">La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ff72-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="5ff72-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity.</span><span class="sxs-lookup"><span data-stu-id="5ff72-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="5ff72-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definisce il [middleware](xref:fundamentals/middleware/index) chiamato nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5ff72-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="5ff72-134">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="5ff72-135">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="5ff72-135">Content root</span></span>

<span data-ttu-id="5ff72-136">La radice del contenuto è il percorso di base per i contenuti usati dall'app, ad esempio [Razor Pages](xref:razor-pages/index), visualizzazioni MVC e asset statici.</span><span class="sxs-lookup"><span data-stu-id="5ff72-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="5ff72-137">Per impostazione predefinita, la posizione della radice del contenuto è uguale al percorso di base dell'app per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="5ff72-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="5ff72-138">Radice Web (webroot)</span><span class="sxs-lookup"><span data-stu-id="5ff72-138">Web root (webroot)</span></span>

<span data-ttu-id="5ff72-139">La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="5ff72-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="5ff72-140">Per impostazione predefinita *wwwroot* è la radice Web.</span><span class="sxs-lookup"><span data-stu-id="5ff72-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="5ff72-141">Per i file Razor (*. cshtml*), il carattere tilde `~/` punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="5ff72-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="5ff72-142">I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.</span><span class="sxs-lookup"><span data-stu-id="5ff72-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="5ff72-143">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="5ff72-143">Dependency injection (services)</span></span>

<span data-ttu-id="5ff72-144">Un *servizio* è un componente destinato a un utilizzo comune in un'app.</span><span class="sxs-lookup"><span data-stu-id="5ff72-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="5ff72-145">I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5ff72-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5ff72-146">ASP.NET Core include un'inversione nativa del contenitore del controllo (IoC) che supporta l'[inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5ff72-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="5ff72-147">Se si vuole, è possibile sostituire il contenitore predefinito.</span><span class="sxs-lookup"><span data-stu-id="5ff72-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="5ff72-148">Oltre al vantaggio dell'[accoppiamento debole](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), l'iniezione delle dipendenze rende disponibili i servizi in tutta l'app, ad esempio la [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="5ff72-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="5ff72-149">Per ulteriori informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="5ff72-150">Middleware</span><span class="sxs-lookup"><span data-stu-id="5ff72-150">Middleware</span></span>

<span data-ttu-id="5ff72-151">In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5ff72-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="5ff72-152">Il middleware ASP.NET Core esegue le operazioni asincrone su un `HttpContext`, quindi richiama il middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5ff72-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="5ff72-153">Per convenzione viene aggiunto un componente middleware denominato "XYZ" alla pipeline richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5ff72-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="5ff72-154">ASP.NET Core include un ampio set di middleware predefinito ed è possibile scrivere middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="5ff72-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="5ff72-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), che consente di disaccoppiare le app Web dai server Web, è supportato nelle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ff72-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="5ff72-156">Per altre informazioni, vedere <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="5ff72-157">Inizializzare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="5ff72-157">Initiate HTTP requests</span></span>

<span data-ttu-id="5ff72-158"><xref:System.Net.Http.IHttpClientFactory> è disponibile per accedere alle istanze di <xref:System.Net.Http.HttpClient> per effettuare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ff72-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="5ff72-159">Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="5ff72-160">Ambienti</span><span class="sxs-lookup"><span data-stu-id="5ff72-160">Environments</span></span>

<span data-ttu-id="5ff72-161">Gli ambienti, come ad esempio *Sviluppo* e *Produzione*, sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando una variabile di ambiente, un file di impostazioni e un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5ff72-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="5ff72-162">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="5ff72-163">Hosting</span><span class="sxs-lookup"><span data-stu-id="5ff72-163">Hosting</span></span>

<span data-ttu-id="5ff72-164">Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="5ff72-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="5ff72-165">Per ulteriori informazioni, vedere <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="5ff72-166">Server</span><span class="sxs-lookup"><span data-stu-id="5ff72-166">Servers</span></span>

<span data-ttu-id="5ff72-167">Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5ff72-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="5ff72-168">Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="5ff72-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="5ff72-169">La richiesta inoltrata viene inclusa come un set di oggetti di funzionalità cui è possibile accedere tramite le interfacce.</span><span class="sxs-lookup"><span data-stu-id="5ff72-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="5ff72-170">ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5ff72-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="5ff72-171">Kestrel viene eseguito di solito con un server Web di produzione, ad esempio [IIS](https://www.iis.net/) oppure [Nginx](http://nginx.org) in una configurazione con proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="5ff72-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="5ff72-172">Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="5ff72-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="5ff72-173">Per ulteriori informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="5ff72-174">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5ff72-174">Configuration</span></span>

<span data-ttu-id="5ff72-175">ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="5ff72-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="5ff72-176">Il modello di configurazione non è basato su <xref:System.Configuration> o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5ff72-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="5ff72-177">I provider di configurazione predefiniti supportano un'ampia gamma di formati di file (XML, JSON, INI), di variabili di ambiente e di argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5ff72-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="5ff72-178">È anche possibile scrivere i propri provider di configurazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="5ff72-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="5ff72-179">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="5ff72-180">Registrazione</span><span class="sxs-lookup"><span data-stu-id="5ff72-180">Logging</span></span>

<span data-ttu-id="5ff72-181">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="5ff72-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="5ff72-182">I provider predefiniti supportano l'invio dei registri a una o più destinazioni.</span><span class="sxs-lookup"><span data-stu-id="5ff72-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="5ff72-183">È possibile usare framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="5ff72-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="5ff72-184">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="5ff72-185">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="5ff72-185">Error handling</span></span>

<span data-ttu-id="5ff72-186">ASP.NET Core include scenari predefiniti per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statici e la gestione delle eccezioni all'avvio.</span><span class="sxs-lookup"><span data-stu-id="5ff72-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="5ff72-187">Per ulteriori informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="5ff72-188">Routing</span><span class="sxs-lookup"><span data-stu-id="5ff72-188">Routing</span></span>

<span data-ttu-id="5ff72-189">ASP.NET Core offre scenari per il routing delle richieste di app ai gestori di route.</span><span class="sxs-lookup"><span data-stu-id="5ff72-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="5ff72-190">Per ulteriori informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="5ff72-191">Attività in background</span><span class="sxs-lookup"><span data-stu-id="5ff72-191">Background tasks</span></span>

<span data-ttu-id="5ff72-192">Le attività in background vengono implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="5ff72-192">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="5ff72-193">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-193">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="5ff72-194">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-194">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="5ff72-195">Accedere a HttpContext</span><span class="sxs-lookup"><span data-stu-id="5ff72-195">Access HttpContext</span></span>

<span data-ttu-id="5ff72-196">`HttpContext` è disponibile automaticamente durante l'elaborazione delle richieste con Razor Pages e MVC.</span><span class="sxs-lookup"><span data-stu-id="5ff72-196">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="5ff72-197">In circostanze in cui `HttpContext` non è immediatamente disponibile, è possibile accedere a `HttpContext` tramite l'interfaccia <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e la relativa implementazione predefinita, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-197">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="5ff72-198">Per ulteriori informazioni, vedere <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="5ff72-198">For more information, see <xref:fundamentals/httpcontext>.</span></span>
