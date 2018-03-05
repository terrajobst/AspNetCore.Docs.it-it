---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Individuare i concetti fondamentali per la compilazione di applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: be37df7789354ac4ce8e373a1560366be157ffa5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="c7ccf-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7ccf-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="c7ccf-104">Un'applicazione ASP.NET Core è un'applicazione console che crea un server Web nel suo metodo `Main`:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7ccf-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c7ccf-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="c7ccf-106">Il metodo `Main` richiama `WebHost.CreateDefaultBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="c7ccf-107">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="c7ccf-108">Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="c7ccf-109">L'host Web di ASP.NET Core tenta l'esecuzione in IIS, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="c7ccf-110">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c7ccf-111">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c7ccf-112">`IWebHostBuilder`, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="c7ccf-113">Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="c7ccf-114">I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7ccf-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c7ccf-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="c7ccf-116">Il metodo `Main` usa `WebHostBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="c7ccf-117">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="c7ccf-118">Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="c7ccf-119">Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c7ccf-120">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c7ccf-121">`WebHostBuilder` offre molti metodi facoltativi, incluso `UseIISIntegration` per l'hosting in IIS e IIS Express e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="c7ccf-122">I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="c7ccf-123">Avvio</span><span class="sxs-lookup"><span data-stu-id="c7ccf-123">Startup</span></span>

<span data-ttu-id="c7ccf-124">Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7ccf-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c7ccf-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7ccf-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c7ccf-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="c7ccf-127">Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="c7ccf-128">La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="c7ccf-129">`ConfigureServices` definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="c7ccf-130">`Configure` definisce il [middleware](xref:fundamentals/middleware/index) nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="c7ccf-131">Per altre informazioni, vedere [Avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="c7ccf-132">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="c7ccf-132">Content root</span></span>

<span data-ttu-id="c7ccf-133">La radice del contenuto è il percorso di base per i contenuti usati dall'applicazione, ad esempio viste, [Razor Pages](xref:mvc/razor-pages/index)e asset statici.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="c7ccf-134">Per impostazione predefinita, la radice del contenuto è uguale al percorso di base dell'applicazione per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="c7ccf-135">Radice Web</span><span class="sxs-lookup"><span data-stu-id="c7ccf-135">Web root</span></span>

<span data-ttu-id="c7ccf-136">La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="c7ccf-137">Inserimento delle dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="c7ccf-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="c7ccf-138">Un servizio è un componente destinato a un uso comune in un'app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="c7ccf-139">I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c7ccf-140">ASP.NET Core include nativo **si**/nPer **o**f **C**contenitore ontrollo (IoC) che supporta [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="c7ccf-141">Se si vuole, è possibile sostituire il contenitore nativo predefinito.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="c7ccf-142">Oltre al suo ampio beneficio di accoppiamento, l'inserimento delle dipendenze rende disponibili i servizi in tutta l'app. Un esempio è dato dalla [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="c7ccf-143">Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="c7ccf-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="c7ccf-144">Middleware</span></span>

<span data-ttu-id="c7ccf-145">In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="c7ccf-146">ASP.NET Core middleware esegue la logica asincrona su un `HttpContext`, quindi richiama il middleware successivo nella sequenza oppure termina la richiesta direttamente.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="c7ccf-147">Viene aggiunto un componente del middleware denominato "XYZ" richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="c7ccf-148">ASP.NET Core include un'ampia gamma di middleware incorporato:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="c7ccf-149">File statici</span><span class="sxs-lookup"><span data-stu-id="c7ccf-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="c7ccf-150">Routing</span><span class="sxs-lookup"><span data-stu-id="c7ccf-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="c7ccf-151">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c7ccf-152">Middleware di compressione delle risposte</span><span class="sxs-lookup"><span data-stu-id="c7ccf-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="c7ccf-153">Middleware di riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="c7ccf-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="c7ccf-154">I middleware basati su [OWIN](http://owin.org) sono disponibili per le app ASP.NET Core ed è possibile scrivere il proprio middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="c7ccf-155">Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware/index) e [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) (Interfaccia Web aperta per .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="c7ccf-156">Ambienti</span><span class="sxs-lookup"><span data-stu-id="c7ccf-156">Environments</span></span>

<span data-ttu-id="c7ccf-157">Gli ambienti, come ad esempio "Sviluppo" e "Produzione", sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="c7ccf-158">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="c7ccf-159">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-159">Configuration</span></span>

<span data-ttu-id="c7ccf-160">ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="c7ccf-161">Il modello di configurazione non è basato su `System.Configuration` o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="c7ccf-162">I provider di configurazione integrati supportano un'ampia gamma di formati di file (XML, JSON, INI) e di variabili di ambiente per abilitare la configurazione basata sull'ambiente.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="c7ccf-163">È anche possibile scrivere i propri provider di configurazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="c7ccf-164">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="c7ccf-165">Registrazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-165">Logging</span></span>

<span data-ttu-id="c7ccf-166">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="c7ccf-167">I provider predefiniti supportano l'invio dei registri a una o più destinazioni.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="c7ccf-168">È possibile usare framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="c7ccf-169">Registrazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="c7ccf-170">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="c7ccf-170">Error handling</span></span>

<span data-ttu-id="c7ccf-171">ASP.NET Core ha funzionalità incorporate per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statico e la gestione delle eccezioni all'avvio.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="c7ccf-172">Per altre informazioni, vedere [Gestione degli errori](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="c7ccf-173">Routing</span><span class="sxs-lookup"><span data-stu-id="c7ccf-173">Routing</span></span>

<span data-ttu-id="c7ccf-174">ASP.NET Core offre funzionalità per il routing delle richieste di app ai gestori di route.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="c7ccf-175">Per altre informazioni, vedere [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="c7ccf-176">Provider di file</span><span class="sxs-lookup"><span data-stu-id="c7ccf-176">File providers</span></span>

<span data-ttu-id="c7ccf-177">ASP.NET Core astrae l'accesso al file system tramite l'uso di provider di file, che offrono un'interfaccia comune per l'uso di file tra le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="c7ccf-178">Per altre informazioni, vedere [Provider di file](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="c7ccf-179">File statici</span><span class="sxs-lookup"><span data-stu-id="c7ccf-179">Static files</span></span>

<span data-ttu-id="c7ccf-180">Il middleware dei file statici viene usato per i file statici, come ad esempio HTML, CSS, file di immagine e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="c7ccf-181">Per altre informazioni, vedere [Uso di file statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="c7ccf-182">Hosting</span><span class="sxs-lookup"><span data-stu-id="c7ccf-182">Hosting</span></span>

<span data-ttu-id="c7ccf-183">Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="c7ccf-184">Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="c7ccf-185">Stato di sessione e applicazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-185">Session and application state</span></span>

<span data-ttu-id="c7ccf-186">Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="c7ccf-187">Per altre informazioni, vedere [Stato di sessione e applicazione](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="c7ccf-188">Server</span><span class="sxs-lookup"><span data-stu-id="c7ccf-188">Servers</span></span>

<span data-ttu-id="c7ccf-189">Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="c7ccf-190">Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="c7ccf-191">La richiesta inoltrata viene inclusa come un set di oggetti di funzionalità cui è possibile accedere tramite le interfacce.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="c7ccf-192">ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="c7ccf-193">Kestrel viene spesso eseguito con un server Web di produzione, come ad esempio [IIS](https://www.iis.net/) o [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="c7ccf-194">Kestrel può essere eseguito come un server perimetrale.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="c7ccf-195">Per altre informazioni, vedere [Server](xref:fundamentals/servers/index) e gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="c7ccf-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c7ccf-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="c7ccf-197">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7ccf-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="c7ccf-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="c7ccf-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="c7ccf-199">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="c7ccf-199">Globalization and localization</span></span>

<span data-ttu-id="c7ccf-200">La creazione di un sito Web multilingue con ASP.NET Core consente al sito di raggiungere un gruppo di destinatari più ampio.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="c7ccf-201">AP.NET Core offre servizi e middleware per la localizzazione in diverse lingue e culture.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="c7ccf-202">Per altre informazioni, vedere [Globalizzazione e localizzazione](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="c7ccf-203">Funzionalità di richiesta</span><span class="sxs-lookup"><span data-stu-id="c7ccf-203">Request features</span></span>

<span data-ttu-id="c7ccf-204">I dettagli dell'implementazione di server Web relativi alle richieste e alle risposte HTTP vengono definiti nelle interfacce.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="c7ccf-205">Le interfacce vengono usate dalle implementazioni del server e dal middleware per creare e modificare la pipeline di hosting dell'app.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="c7ccf-206">Per altre informazioni, vedere [Funzionalità di richiesta](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="c7ccf-207">Attività in background</span><span class="sxs-lookup"><span data-stu-id="c7ccf-207">Background tasks</span></span>

<span data-ttu-id="c7ccf-208">Le attività in background vengono implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-208">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="c7ccf-209">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-209">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="c7ccf-210">Per altre informazioni, vedere [Attività in background con servizi ospitati](xref:fundamentals/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-210">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="c7ccf-211">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="c7ccf-211">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="c7ccf-212">ASP.NET Core supporta Open Web Interface for .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-212">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="c7ccf-213">OWIN consente alle app Web di essere disaccoppiate dai server Web.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-213">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="c7ccf-214">Per altre informazioni, vedere [OWIN](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-214">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="c7ccf-215">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="c7ccf-215">WebSockets</span></span>

<span data-ttu-id="c7ccf-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c7ccf-217">Viene usato per app come chat, teleborsa, giochi e per qualsiasi funzionalità in tempo reale in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-217">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="c7ccf-218">ASP.NET Core supporta le funzionalità WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-218">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="c7ccf-219">Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-219">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="c7ccf-220">Metapacchetto Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="c7ccf-220">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="c7ccf-221">Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:</span><span class="sxs-lookup"><span data-stu-id="c7ccf-221">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="c7ccf-222">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-222">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="c7ccf-223">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-223">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="c7ccf-224">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-224">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="c7ccf-225">Per altre informazioni, vedere [Metapacchetto Microsoft.AspNetCore.All ](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-225">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="c7ccf-226">Runtime .NET core e .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c7ccf-226">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="c7ccf-227">Un'app di ASP.NET Core può avere come destinazione il runtime di .NET Core o di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c7ccf-227">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="c7ccf-228">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-228">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="c7ccf-229">Scegliere tra ASP.NET Core e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7ccf-229">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="c7ccf-230">Per altre informazioni sulla scelta tra ASP.NET Core e ASP.NET, vedere [Scegliere tra ASP.NET Core e ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="c7ccf-230">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
