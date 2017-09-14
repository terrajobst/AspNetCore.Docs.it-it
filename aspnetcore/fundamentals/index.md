---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Questo articolo offre una panoramica generale dei concetti fondamentali da comprendere nella creazione di applicazioni ASP.NET Core.
keywords: ASP.NET Core, nozioni di base, panoramica
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="d4049-104">ASP.NET Core, nozioni di base, panoramica</span><span class="sxs-lookup"><span data-stu-id="d4049-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="d4049-105">Un'applicazione ASP.NET Core è un'applicazione console che crea un server Web nel suo metodo `Main`:</span><span class="sxs-lookup"><span data-stu-id="d4049-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d4049-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d4049-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d4049-107">[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4049-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="d4049-108">Il metodo `Main` richiama `WebHost.CreateDefaultBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="d4049-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d4049-109">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d4049-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d4049-110">Nell'esempio precedente, viene allocato automaticamente un server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d4049-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d4049-111">L'host Web di ASP.NET Core tenterà l'esecuzione in IIS, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="d4049-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="d4049-112">Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="d4049-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d4049-113">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="d4049-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d4049-114">`IWebHostBuilder`, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="d4049-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d4049-115">Alcuni di questi metodi includono `UseHttpSys` per ospitare l'applicazione in HTTP.sys e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="d4049-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d4049-116">I metodi `Build` e `Run` generano l'oggetto `IWebHost` che ospiterà l'applicazione e inizierà a essere in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4049-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d4049-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d4049-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d4049-118">[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="d4049-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="d4049-119">Il metodo `Main` usa `WebHostBuilder`, che segue il modello di generatore per creare un host applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="d4049-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d4049-120">Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d4049-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d4049-121">Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d4049-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d4049-122">Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato.</span><span class="sxs-lookup"><span data-stu-id="d4049-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d4049-123">`UseStartup` viene spiegato dettagliatamente nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="d4049-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d4049-124">`WebHostBuilder` offre molti metodi facoltativi, incluso `UseIISIntegration` per l'hosting in IIS e IIS Express e `UseContentRoot` per specificare la directory del contenuto radice.</span><span class="sxs-lookup"><span data-stu-id="d4049-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d4049-125">I metodi `Build` e `Run` generano l'oggetto `IWebHost` che ospiterà l'applicazione e inizierà a essere in ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4049-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="d4049-126">Avvio</span><span class="sxs-lookup"><span data-stu-id="d4049-126">Startup</span></span>

<span data-ttu-id="d4049-127">Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:</span><span class="sxs-lookup"><span data-stu-id="d4049-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d4049-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d4049-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d4049-129">[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d4049-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d4049-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d4049-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d4049-131">[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d4049-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="d4049-132">Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="d4049-133">La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d4049-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="d4049-134">`ConfigureServices` definisce i [servizi](#services) usati dall'applicazione in uso (come ASP.NET Core MVC, Entity Framework Core, Identity, ecc.).</span><span class="sxs-lookup"><span data-stu-id="d4049-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="d4049-135">`Configure` definisce il [middleware](xref:fundamentals/middleware) nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="d4049-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="d4049-136">Per altre informazioni, vedere [Avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d4049-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="d4049-137">Servizi</span><span class="sxs-lookup"><span data-stu-id="d4049-137">Services</span></span>

<span data-ttu-id="d4049-138">Un servizio è un componente destinato a un utilizzo comune in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="d4049-139">I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d4049-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="d4049-140">ASP.NET Core include un'inversione nativa del contenitore del controllo (IoC) che supporta l'[inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d4049-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d4049-141">Il contenitore nativo può essere sostituito dal contenitore scelto dall'utente.</span><span class="sxs-lookup"><span data-stu-id="d4049-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="d4049-142">Oltre al suo ampio beneficio di accoppiamento, l'inserimento delle dipendenze rende disponibili i servizi in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="d4049-143">Ad esempio, la [registrazione](xref:fundamentals/logging) è disponibile in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="d4049-144">Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="d4049-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d4049-145">Middleware</span><span class="sxs-lookup"><span data-stu-id="d4049-145">Middleware</span></span>

<span data-ttu-id="d4049-146">In ASP.NET Core, la richiesta di pipeline viene composta usando [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="d4049-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="d4049-147">ASP.NET Core middleware esegue la logica asincrona su un `HttpContext`, quindi richiama il middleware successivo nella sequenza oppure termina la richiesta direttamente.</span><span class="sxs-lookup"><span data-stu-id="d4049-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="d4049-148">Viene aggiunto un componente del middleware denominato "XYZ" richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4049-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d4049-149">ASP.NET Core viene fornito con un'ampia gamma di middleware incorporato:</span><span class="sxs-lookup"><span data-stu-id="d4049-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="d4049-150">File statici</span><span class="sxs-lookup"><span data-stu-id="d4049-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="d4049-151">Routing</span><span class="sxs-lookup"><span data-stu-id="d4049-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="d4049-152">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="d4049-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="d4049-153">È possibile utilizzare qualunque middleware basato su [OWIN](http://owin.org) con ASP.NET Core ed è possibile scrivere il proprio middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="d4049-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="d4049-154">Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware) e [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) (Interfaccia Web aperta per .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="d4049-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="d4049-155">Server</span><span class="sxs-lookup"><span data-stu-id="d4049-155">Servers</span></span>

<span data-ttu-id="d4049-156">Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste; si basa piuttosto su un'implementazione del server HTTP per inoltrare la richiesta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="d4049-157">La richiesta inoltrata viene inclusa come un insieme di oggetti di funzionalità cui è possibile accedere tramite le interfacce.</span><span class="sxs-lookup"><span data-stu-id="d4049-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="d4049-158">L'applicazione compone questo insieme in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="d4049-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="d4049-159">ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d4049-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d4049-160">Kestrel viene eseguito in genere protetto da un server Web di produzione come [IIS](https://www.iis.net/) o [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="d4049-160">Kestrel is typically run behind a production web server like [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="d4049-161">Per altre informazioni, vedere [Server](xref:fundamentals/servers/index) e [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="d4049-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="d4049-162">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="d4049-162">Content root</span></span>

<span data-ttu-id="d4049-163">La radice del contenuto è il percorso di base per i contenuti usati dall'applicazione, ad esempio viste, [Razor Pages](xref:mvc/razor-pages/index)e asset statici.</span><span class="sxs-lookup"><span data-stu-id="d4049-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="d4049-164">Per impostazione predefinita, la radice del contenuto è uguale al percorso di base dell'applicazione per il file eseguibile che ospita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="d4049-165">Viene specificato un percorso alternativo per la radice del contenuto con `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4049-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="d4049-166">Radice Web</span><span class="sxs-lookup"><span data-stu-id="d4049-166">Web root</span></span>

<span data-ttu-id="d4049-167">La radice Web di un'applicazione è la directory del progetto contenente risorse statiche pubbliche, come CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="d4049-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d4049-168">Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della directory radice Web e relative sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="d4049-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="d4049-169">Per altre informazioni, vedere [Uso di file statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="d4049-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="d4049-170">Il percorso predefinito della radice Web è */wwwroot*, ma è possibile specificare un percorso diverso tramite `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4049-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="d4049-171">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d4049-171">Configuration</span></span>

<span data-ttu-id="d4049-172">ASP.NET Core usa un nuovo modello di configurazione per la gestione di coppie nome-valore semplici.</span><span class="sxs-lookup"><span data-stu-id="d4049-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="d4049-173">Il nuovo modello di configurazione non è basato su `System.Configuration` o *web.config*, ma deriva invece da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d4049-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="d4049-174">I provider di configurazione integrati supportano un'ampia gamma di formati di file (XML, JSON, INI) e di variabili di ambiente per abilitare la configurazione basata sull'ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4049-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="d4049-175">È anche possibile scrivere i propri provider di configurazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="d4049-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d4049-176">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="d4049-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="d4049-177">Ambienti</span><span class="sxs-lookup"><span data-stu-id="d4049-177">Environments</span></span>

<span data-ttu-id="d4049-178">Gli ambienti come "Sviluppo" e "Produzione" sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4049-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="d4049-179">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d4049-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d4049-180">Runtime .NET core e .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d4049-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d4049-181">Un'applicazione ASP.NET Core può essere finalizzata al runtime di .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d4049-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="d4049-182">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d4049-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="d4049-183">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d4049-183">Additional information</span></span>

<span data-ttu-id="d4049-184">Vedere anche gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d4049-184">See also the following topics:</span></span>

- [<span data-ttu-id="d4049-185">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="d4049-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="d4049-186">Provider di file</span><span class="sxs-lookup"><span data-stu-id="d4049-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="d4049-187">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="d4049-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="d4049-188">Registrazione</span><span class="sxs-lookup"><span data-stu-id="d4049-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="d4049-189">Gestione dello stato di un'applicazione</span><span class="sxs-lookup"><span data-stu-id="d4049-189">Managing Application State</span></span>](xref:fundamentals/app-state)
