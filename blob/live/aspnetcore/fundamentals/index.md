---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Individuare i concetti fondamentali per la compilazione di applicazioni ASP.NET Core.
keywords: ASP.NET Core, nozioni di base, panoramica
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 83bed4676be3ca752442da3fe560f1f2a4d728a1
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="31880-104">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31880-104">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="31880-105">Un'applicazione ASP.NET Core è un'applicazione console che crea un server Web nel suo metodo `Main`:</span><span class="sxs-lookup"><span data-stu-id="31880-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="31880-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="31880-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="31880-107">Il metodo `Main` richiama `WebHost.CreateDefaultBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="31880-107">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="31880-108">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="31880-108">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="31880-109">Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="31880-109">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="31880-110">L'host Web di ASP.NET Core tenta l'esecuzione in IIS, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="31880-110">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="31880-111">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="31880-111">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="31880-112">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="31880-112">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="31880-113">`IWebHostBuilder`, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="31880-113">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="31880-114">Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="31880-114">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="31880-115">I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="31880-115">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="31880-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="31880-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="31880-117">Il metodo `Main` usa `WebHostBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="31880-117">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="31880-118">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="31880-118">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="31880-119">Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="31880-119">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="31880-120">Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="31880-120">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="31880-121">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="31880-121">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="31880-122">`WebHostBuilder` offre molti metodi facoltativi, incluso `UseIISIntegration` per l'hosting in IIS e IIS Express e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="31880-122">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="31880-123">I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="31880-123">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="31880-124">Avvio</span><span class="sxs-lookup"><span data-stu-id="31880-124">Startup</span></span>

<span data-ttu-id="31880-125">Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:</span><span class="sxs-lookup"><span data-stu-id="31880-125">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="31880-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="31880-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="31880-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="31880-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="31880-128">Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="31880-128">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="31880-129">La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="31880-129">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="31880-130">`ConfigureServices` definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity.</span><span class="sxs-lookup"><span data-stu-id="31880-130">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="31880-131">`Configure` definisce il [middleware](xref:fundamentals/middleware) nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="31880-131">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="31880-132">Per altre informazioni, vedere [Avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="31880-132">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="31880-133">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="31880-133">Content root</span></span>

<span data-ttu-id="31880-134">La radice del contenuto è il percorso di base per i contenuti usati dall'applicazione, ad esempio viste, [Razor Pages](xref:mvc/razor-pages/index)e asset statici.</span><span class="sxs-lookup"><span data-stu-id="31880-134">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="31880-135">Per impostazione predefinita, la radice del contenuto è uguale al percorso di base dell'applicazione per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="31880-135">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="31880-136">Radice Web</span><span class="sxs-lookup"><span data-stu-id="31880-136">Web root</span></span>

<span data-ttu-id="31880-137">La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="31880-137">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="31880-138">Inserimento delle dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="31880-138">Dependency Injection (Services)</span></span>

<span data-ttu-id="31880-139">Un servizio è un componente destinato a un uso comune in un'app.</span><span class="sxs-lookup"><span data-stu-id="31880-139">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="31880-140">I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="31880-140">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="31880-141">ASP.NET Core include nativo **si**/nPer **o**f **C**contenitore ontrollo (IoC) che supporta [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="31880-141">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="31880-142">Se si vuole, è possibile sostituire il contenitore nativo predefinito.</span><span class="sxs-lookup"><span data-stu-id="31880-142">You can replace the default native container if you wish.</span></span> <span data-ttu-id="31880-143">Oltre al suo ampio beneficio di accoppiamento, l'inserimento delle dipendenze rende disponibili i servizi in tutta l'app. Un esempio è dato dalla [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="31880-143">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="31880-144">Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="31880-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="31880-145">Middleware</span><span class="sxs-lookup"><span data-stu-id="31880-145">Middleware</span></span>

<span data-ttu-id="31880-146">In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="31880-146">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="31880-147">ASP.NET Core middleware esegue la logica asincrona su un `HttpContext`, quindi richiama il middleware successivo nella sequenza oppure termina la richiesta direttamente.</span><span class="sxs-lookup"><span data-stu-id="31880-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="31880-148">Viene aggiunto un componente del middleware denominato "XYZ" richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="31880-148">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="31880-149">ASP.NET Core viene fornito con un'ampia gamma di middleware incorporato:</span><span class="sxs-lookup"><span data-stu-id="31880-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="31880-150">File statici</span><span class="sxs-lookup"><span data-stu-id="31880-150">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="31880-151">Routing</span><span class="sxs-lookup"><span data-stu-id="31880-151">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="31880-152">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="31880-152">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="31880-153">Middleware di compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="31880-153">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="31880-154">Middleware di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="31880-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="31880-155">I middleware basati su [OWIN](http://owin.org) sono disponibili per le app ASP.NET Core ed è possibile scrivere il proprio middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="31880-155">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="31880-156">Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware) e [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) (Interfaccia Web aperta per .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="31880-156">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="31880-157">Ambienti</span><span class="sxs-lookup"><span data-stu-id="31880-157">Environments</span></span>

<span data-ttu-id="31880-158">Gli ambienti, come ad esempio "Sviluppo" e "Produzione", sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="31880-158">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="31880-159">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="31880-159">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="31880-160">Configurazione</span><span class="sxs-lookup"><span data-stu-id="31880-160">Configuration</span></span>

<span data-ttu-id="31880-161">ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="31880-161">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="31880-162">Il modello di configurazione non è basato su `System.Configuration` o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="31880-162">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="31880-163">I provider di configurazione integrati supportano un'ampia gamma di formati di file (XML, JSON, INI) e di variabili di ambiente per abilitare la configurazione basata sull'ambiente.</span><span class="sxs-lookup"><span data-stu-id="31880-163">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="31880-164">È anche possibile scrivere i propri provider di configurazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="31880-164">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="31880-165">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="31880-165">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="31880-166">Registrazione</span><span class="sxs-lookup"><span data-stu-id="31880-166">Logging</span></span>

<span data-ttu-id="31880-167">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="31880-167">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="31880-168">I provider predefiniti supportano l'invio dei registri a una o più destinazioni.</span><span class="sxs-lookup"><span data-stu-id="31880-168">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="31880-169">È possibile usare framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="31880-169">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="31880-170">Registrazione</span><span class="sxs-lookup"><span data-stu-id="31880-170">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="31880-171">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="31880-171">Error handling</span></span>

<span data-ttu-id="31880-172">ASP.NET Core ha funzionalità incorporate per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statico e la gestione delle eccezioni all'avvio.</span><span class="sxs-lookup"><span data-stu-id="31880-172">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="31880-173">Per altre informazioni, vedere [Gestione degli errori](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="31880-173">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="31880-174">Routing</span><span class="sxs-lookup"><span data-stu-id="31880-174">Routing</span></span>

<span data-ttu-id="31880-175">ASP.NET Core offre funzionalità per il routing delle richieste di app ai gestori di route.</span><span class="sxs-lookup"><span data-stu-id="31880-175">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="31880-176">Per altre informazioni, vedere [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="31880-176">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="31880-177">Provider di file</span><span class="sxs-lookup"><span data-stu-id="31880-177">File providers</span></span>

<span data-ttu-id="31880-178">ASP.NET Core astrae l'accesso al file system tramite l'uso di provider di file, che offrono un'interfaccia comune per l'uso di file tra le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="31880-178">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="31880-179">Per altre informazioni, vedere [Provider di file](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="31880-179">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="31880-180">File statici</span><span class="sxs-lookup"><span data-stu-id="31880-180">Static files</span></span>

<span data-ttu-id="31880-181">Il middleware dei file statici viene usato per i file statici, come ad esempio HTML, CSS, file di immagine e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="31880-181">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="31880-182">Per altre informazioni, vedere [Uso di file statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="31880-182">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="31880-183">Hosting</span><span class="sxs-lookup"><span data-stu-id="31880-183">Hosting</span></span>

<span data-ttu-id="31880-184">Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="31880-184">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="31880-185">Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="31880-185">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="31880-186">Stato di sessione e applicazione</span><span class="sxs-lookup"><span data-stu-id="31880-186">Session and application state</span></span>

<span data-ttu-id="31880-187">Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web.</span><span class="sxs-lookup"><span data-stu-id="31880-187">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="31880-188">Per altre informazioni, vedere [Stato di sessione e applicazione](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="31880-188">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="31880-189">Server</span><span class="sxs-lookup"><span data-stu-id="31880-189">Servers</span></span>

<span data-ttu-id="31880-190">Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste.</span><span class="sxs-lookup"><span data-stu-id="31880-190">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="31880-191">Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="31880-191">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="31880-192">La richiesta inoltrata viene inclusa come un set di oggetti di funzionalità cui è possibile accedere tramite le interfacce.</span><span class="sxs-lookup"><span data-stu-id="31880-192">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="31880-193">ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="31880-193">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="31880-194">Kestrel viene spesso eseguito con un server Web di produzione, come ad esempio [IIS](https://www.iis.net/) o [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="31880-194">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span> <span data-ttu-id="31880-195">Kestrel può essere eseguito come un server perimetrale.</span><span class="sxs-lookup"><span data-stu-id="31880-195">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="31880-196">Per altre informazioni, vedere [Server](xref:fundamentals/servers/index) e gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="31880-196">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="31880-197">Kestrel</span><span class="sxs-lookup"><span data-stu-id="31880-197">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="31880-198">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31880-198">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="31880-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="31880-199">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="31880-200">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="31880-200">Globalization and localization</span></span>

<span data-ttu-id="31880-201">La creazione di un sito Web multilingue con ASP.NET Core consente al sito di raggiungere un gruppo di destinatari più ampio.</span><span class="sxs-lookup"><span data-stu-id="31880-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="31880-202">AP.NET Core offre servizi e middleware per la localizzazione in diverse lingue e culture.</span><span class="sxs-lookup"><span data-stu-id="31880-202">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="31880-203">Per altre informazioni, vedere [Globalizzazione e localizzazione](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="31880-203">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="31880-204">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="31880-204">Request features</span></span>

<span data-ttu-id="31880-205">I dettagli dell'implementazione di server Web relativi alle richieste e alle risposte HTTP vengono definiti nelle interfacce.</span><span class="sxs-lookup"><span data-stu-id="31880-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="31880-206">Le interfacce vengono usate dalle implementazioni del server e dal middleware per creare e modificare la pipeline di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="31880-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="31880-207">Per altre informazioni, vedere [Funzionalità di richiesta](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="31880-207">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="31880-208">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="31880-208">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="31880-209">ASP.NET Core supporta Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="31880-209">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="31880-210">OWIN consente alle app Web di essere disaccoppiate dai server Web.</span><span class="sxs-lookup"><span data-stu-id="31880-210">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="31880-211">Per altre informazioni, vedere [OWIN](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="31880-211">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="31880-212">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="31880-212">WebSockets</span></span>

<span data-ttu-id="31880-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="31880-213">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="31880-214">Viene usato per app come chat, teleborsa, giochi e per qualsiasi funzionalità in tempo reale in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="31880-214">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="31880-215">ASP.NET Core supporta le funzionalità WebSocket.</span><span class="sxs-lookup"><span data-stu-id="31880-215">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="31880-216">Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="31880-216">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="31880-217">Metapacchetto Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="31880-217">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="31880-218">Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:</span><span class="sxs-lookup"><span data-stu-id="31880-218">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="31880-219">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31880-219">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="31880-220">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="31880-220">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="31880-221">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="31880-221">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="31880-222">Per altre informazioni, vedere [Metapacchetto Microsoft.AspNetCore.All ](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="31880-222">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="31880-223">Runtime .NET core e .NET Framework</span><span class="sxs-lookup"><span data-stu-id="31880-223">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="31880-224">Un'app di ASP.NET Core può avere come destinazione il runtime di .NET Core o di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="31880-224">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="31880-225">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="31880-225">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="31880-226">Scegliere tra ASP.NET Core e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="31880-226">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="31880-227">Per altre informazioni sulla scelta tra ASP.NET Core e ASP.NET, vedere [Scegliere tra ASP.NET Core e ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="31880-227">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
