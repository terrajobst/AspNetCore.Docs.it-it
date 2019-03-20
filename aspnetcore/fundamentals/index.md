---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="bbecd-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbecd-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="bbecd-104">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="bbecd-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="bbecd-105">The Startup class</span></span>

<span data-ttu-id="bbecd-106">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bbecd-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="bbecd-107">Viene eseguita la configurazione di tutti i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="bbecd-108">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="bbecd-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="bbecd-109">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bbecd-110">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="bbecd-111">Un oggetto di contesto Entity Framework Core, ad esempio, è un servizio.</span><span class="sxs-lookup"><span data-stu-id="bbecd-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="bbecd-112">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="bbecd-113">La pipeline è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="bbecd-114">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="bbecd-115">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="bbecd-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bbecd-116">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bbecd-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

<span data-ttu-id="bbecd-117">Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="bbecd-117">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="bbecd-118">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="bbecd-118">Dependency injection (services)</span></span>

<span data-ttu-id="bbecd-119">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="bbecd-120">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="bbecd-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="bbecd-121">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="bbecd-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="bbecd-122">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="bbecd-122">The DI system provides the service at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bbecd-123">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="bbecd-124">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="bbecd-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

<span data-ttu-id="bbecd-125">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="bbecd-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="bbecd-126">Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="bbecd-126">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="bbecd-127">Middleware</span><span class="sxs-lookup"><span data-stu-id="bbecd-127">Middleware</span></span>

<span data-ttu-id="bbecd-128">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="bbecd-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="bbecd-129">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="bbecd-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="bbecd-130">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="bbecd-131">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bbecd-132">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="bbecd-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

<span data-ttu-id="bbecd-133">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="bbecd-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="bbecd-134">Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="bbecd-134">For more information, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="bbecd-135">L'host</span><span class="sxs-lookup"><span data-stu-id="bbecd-135">The host</span></span>

<span data-ttu-id="bbecd-136">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="bbecd-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="bbecd-137">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bbecd-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="bbecd-138">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="bbecd-138">An HTTP server implementation</span></span>
* <span data-ttu-id="bbecd-139">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="bbecd-139">Middleware components</span></span>
* <span data-ttu-id="bbecd-140">Registrazione</span><span class="sxs-lookup"><span data-stu-id="bbecd-140">Logging</span></span>
* <span data-ttu-id="bbecd-141">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="bbecd-141">DI</span></span>
* <span data-ttu-id="bbecd-142">Configurazione</span><span class="sxs-lookup"><span data-stu-id="bbecd-142">Configuration</span></span>

<span data-ttu-id="bbecd-143">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="bbecd-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="bbecd-144">Il codice usato per creare un host si trova in `Program.Main` e segue il pattern [Builder](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="bbecd-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="bbecd-145">Per configurare ogni risorsa che fa parte dell'host vengono chiamati dei metodi.</span><span class="sxs-lookup"><span data-stu-id="bbecd-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="bbecd-146">Un metodo builder viene chiamato per combinare il tutto e creare un'istanza dell'oggetto host.</span><span class="sxs-lookup"><span data-stu-id="bbecd-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="bbecd-147">ASP.NET Core 2.x usa un host Web (classe `WebHost`) per le app Web.</span><span class="sxs-lookup"><span data-stu-id="bbecd-147">ASP.NET Core 2.x uses Web Host (the `WebHost` class) for web apps.</span></span> <span data-ttu-id="bbecd-148">Il framework offre `CreateDefaultBuilder` per configurare un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bbecd-148">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="bbecd-149">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-149">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="bbecd-150">Caricare la configurazione da *appsettings.json*, variabili di ambiente, argomenti della riga di comando e altre origini.</span><span class="sxs-lookup"><span data-stu-id="bbecd-150">Load configuration from *appsettings.json*, environment variables, command line arguments, and other sources.</span></span>
* <span data-ttu-id="bbecd-151">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="bbecd-151">Send logging output to the console and debug providers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="bbecd-152">Ecco un esempio di codice per creare un host:</span><span class="sxs-lookup"><span data-stu-id="bbecd-152">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="bbecd-153">Per altre informazioni, vedere [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="bbecd-153">For more information, see [Web Host](xref:fundamentals/host/web-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="bbecd-154">Nelle app Web in ASP.NET Core 3.0 è possibile usare un host Web (classe `WebHost`) o un host generico (classe `Host`).</span><span class="sxs-lookup"><span data-stu-id="bbecd-154">In ASP.NET Core 3.0, Web Host (`WebHost` class) or Generic Host (`Host` class) can be used in a web app.</span></span> <span data-ttu-id="bbecd-155">È consigliato l'uso di un host generico, mentre l'host Web è disponibile per garantire la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="bbecd-155">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="bbecd-156">Il framework offre i metodi `CreateDefaultBuilder` e `ConfigureWebHostDefaults` per configurare un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bbecd-156">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="bbecd-157">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-157">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="bbecd-158">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="bbecd-158">Load configuration from *appsettings.json*, *appsettings.[EnvironmentName].json*, environment variables, and command line arguments.</span></span>
* <span data-ttu-id="bbecd-159">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="bbecd-159">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="bbecd-160">Ecco un esempio di codice per creare un host.</span><span class="sxs-lookup"><span data-stu-id="bbecd-160">Here's sample code that builds a host.</span></span> <span data-ttu-id="bbecd-161">I metodi che configurano l'host con le opzioni usate comunemente sono evidenziati.</span><span class="sxs-lookup"><span data-stu-id="bbecd-161">The methods that set up the host with commonly used options are highlighted.</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="bbecd-162">Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host) e [Host Web](xref:fundamentals/host/web-host)</span><span class="sxs-lookup"><span data-stu-id="bbecd-162">For more information, see [Generic Host](xref:fundamentals/host/generic-host) and [Web Host](xref:fundamentals/host/web-host)</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="bbecd-163">Scenari host avanzati</span><span class="sxs-lookup"><span data-stu-id="bbecd-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="bbecd-164">L'host Web è progettato per includere un'implementazione del server HTTP che non è necessaria per altri tipi di app .NET.</span><span class="sxs-lookup"><span data-stu-id="bbecd-164">Web Host is designed to include an HTTP server implementation, which isn't needed for other kinds of .NET apps.</span></span> <span data-ttu-id="bbecd-165">A partire dalla versione 2.1, l'host generico (classe `Host`) è disponibile per qualsiasi app .NET Core, non solo per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-165">Starting in 2.1, Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="bbecd-166">L'host generico consente di usare funzionalità trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app in altri tipi di app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-166">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="bbecd-167">Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="bbecd-167">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="bbecd-168">L'host generico è disponibile per qualsiasi app .NET Core, non solo per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-168">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="bbecd-169">L'host generico consente di usare funzionalità trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app in altri tipi di app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-169">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="bbecd-170">Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="bbecd-170">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

<span data-ttu-id="bbecd-171">È anche possibile usare l'host per eseguire attività in background.</span><span class="sxs-lookup"><span data-stu-id="bbecd-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="bbecd-172">Per altre informazioni, vedere [Attività in background](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="bbecd-172">For more information, see [Background tasks](xref:fundamentals/host/hosted-services).</span></span>

## <a name="servers"></a><span data-ttu-id="bbecd-173">Server</span><span class="sxs-lookup"><span data-stu-id="bbecd-173">Servers</span></span>

<span data-ttu-id="bbecd-174">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="bbecd-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="bbecd-175">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="bbecd-176">Windows</span><span class="sxs-lookup"><span data-stu-id="bbecd-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="bbecd-177">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="bbecd-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="bbecd-178">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="bbecd-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="bbecd-179">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="bbecd-180">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="bbecd-181">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="bbecd-182">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="bbecd-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="bbecd-183">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="bbecd-184">macOS</span><span class="sxs-lookup"><span data-stu-id="bbecd-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="bbecd-185">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bbecd-186">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bbecd-187">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bbecd-188">Linux</span><span class="sxs-lookup"><span data-stu-id="bbecd-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="bbecd-189">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bbecd-190">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bbecd-191">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="bbecd-192">Windows</span><span class="sxs-lookup"><span data-stu-id="bbecd-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="bbecd-193">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="bbecd-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="bbecd-194">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="bbecd-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="bbecd-195">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="bbecd-196">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="bbecd-197">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="bbecd-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="bbecd-198">macOS</span><span class="sxs-lookup"><span data-stu-id="bbecd-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="bbecd-199">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bbecd-200">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bbecd-201">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="bbecd-202">Linux</span><span class="sxs-lookup"><span data-stu-id="bbecd-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="bbecd-203">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bbecd-204">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="bbecd-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bbecd-205">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bbecd-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="bbecd-206">Per altre informazioni, vedere [Server](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="bbecd-206">For more information, see [Servers](xref:fundamentals/servers/index).</span></span>

## <a name="configuration"></a><span data-ttu-id="bbecd-207">Configurazione</span><span class="sxs-lookup"><span data-stu-id="bbecd-207">Configuration</span></span>

<span data-ttu-id="bbecd-208">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="bbecd-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="bbecd-209">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="bbecd-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="bbecd-210">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="bbecd-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="bbecd-211">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="bbecd-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="bbecd-212">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bbecd-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="bbecd-213">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="bbecd-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="bbecd-214">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bbecd-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="bbecd-215">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="bbecd-215">For production secrets, we recommend [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span></span>

<span data-ttu-id="bbecd-216">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bbecd-216">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="options"></a><span data-ttu-id="bbecd-217">Opzioni</span><span class="sxs-lookup"><span data-stu-id="bbecd-217">Options</span></span>

<span data-ttu-id="bbecd-218">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="bbecd-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="bbecd-219">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="bbecd-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="bbecd-220">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="bbecd-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="bbecd-221">Per altre informazioni, vedere [Opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="bbecd-221">For more information, see [Options](xref:fundamentals/configuration/options).</span></span>

## <a name="environments"></a><span data-ttu-id="bbecd-222">Ambienti</span><span class="sxs-lookup"><span data-stu-id="bbecd-222">Environments</span></span>

<span data-ttu-id="bbecd-223">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="bbecd-224">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="bbecd-225">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="bbecd-226">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="bbecd-226">The environment object is available anywhere in the app via DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bbecd-227">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="bbecd-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

<span data-ttu-id="bbecd-228">Per altre informazioni, vedere [Ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="bbecd-228">For more information, see [Environments](xref:fundamentals/environments).</span></span>

## <a name="logging"></a><span data-ttu-id="bbecd-229">Registrazione</span><span class="sxs-lookup"><span data-stu-id="bbecd-229">Logging</span></span>

<span data-ttu-id="bbecd-230">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="bbecd-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="bbecd-231">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="bbecd-231">Available providers include the following:</span></span>

* <span data-ttu-id="bbecd-232">Console</span><span class="sxs-lookup"><span data-stu-id="bbecd-232">Console</span></span>
* <span data-ttu-id="bbecd-233">Debug</span><span class="sxs-lookup"><span data-stu-id="bbecd-233">Debug</span></span>
* <span data-ttu-id="bbecd-234">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="bbecd-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="bbecd-235">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="bbecd-235">Windows Event Log</span></span>
* <span data-ttu-id="bbecd-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bbecd-236">TraceSource</span></span>
* <span data-ttu-id="bbecd-237">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="bbecd-237">Azure App Service</span></span>
* <span data-ttu-id="bbecd-238">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="bbecd-238">Azure Application Insights</span></span>

<span data-ttu-id="bbecd-239">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="bbecd-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bbecd-240">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="bbecd-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

<span data-ttu-id="bbecd-241">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="bbecd-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="bbecd-242">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="bbecd-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="bbecd-243">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="bbecd-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="bbecd-244">Per altre informazioni, vedere [Registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="bbecd-244">For more information, see [Logging](xref:fundamentals/logging/index).</span></span>

## <a name="routing"></a><span data-ttu-id="bbecd-245">Routing</span><span class="sxs-lookup"><span data-stu-id="bbecd-245">Routing</span></span>

<span data-ttu-id="bbecd-246">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="bbecd-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="bbecd-247">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="bbecd-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="bbecd-248">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="bbecd-249">Per altre informazioni, vedere [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="bbecd-249">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="error-handling"></a><span data-ttu-id="bbecd-250">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="bbecd-250">Error handling</span></span>

<span data-ttu-id="bbecd-251">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bbecd-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="bbecd-252">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="bbecd-252">A developer exception page</span></span>
* <span data-ttu-id="bbecd-253">Pagine degli errori personalizzate</span><span class="sxs-lookup"><span data-stu-id="bbecd-253">Custom error pages</span></span>
* <span data-ttu-id="bbecd-254">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="bbecd-254">Static status code pages</span></span>
* <span data-ttu-id="bbecd-255">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="bbecd-255">Startup exception handling</span></span>

<span data-ttu-id="bbecd-256">Per altre informazioni, vedere [Gestione degli errori](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="bbecd-256">For more information, see [Error handling](xref:fundamentals/error-handling).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a><span data-ttu-id="bbecd-257">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="bbecd-257">Make HTTP requests</span></span>

<span data-ttu-id="bbecd-258">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="bbecd-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="bbecd-259">Il factory:</span><span class="sxs-lookup"><span data-stu-id="bbecd-259">The factory:</span></span>

* <span data-ttu-id="bbecd-260">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="bbecd-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="bbecd-261">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="bbecd-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="bbecd-262">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="bbecd-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="bbecd-263">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="bbecd-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="bbecd-264">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbecd-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="bbecd-265">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="bbecd-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="bbecd-266">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="bbecd-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="bbecd-267">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="bbecd-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="bbecd-268">Aggiunge un'esperienza di registrazione configurabile, tramite *ILogger*, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="bbecd-268">Adds a configurable logging experience (via *ILogger*) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="bbecd-269">Per altre informazioni, vedere [Creare richieste HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="bbecd-269">For more information, see [Make HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="content-root"></a><span data-ttu-id="bbecd-270">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="bbecd-270">Content root</span></span>

<span data-ttu-id="bbecd-271">La radice del contenuto è il percorso di base per i contenuti privati usati dall'app, ad esempio i relativi file Razor.</span><span class="sxs-lookup"><span data-stu-id="bbecd-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="bbecd-272">Per impostazione predefinita, la radice del contenuto è il percorso di base per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="bbecd-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="bbecd-273">È possibile specificare un percorso alternativo in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="bbecd-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="bbecd-274">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="bbecd-274">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="bbecd-275">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="bbecd-275">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="bbecd-276">Radice Web</span><span class="sxs-lookup"><span data-stu-id="bbecd-276">Web root</span></span>

<span data-ttu-id="bbecd-277">La radice Web, nota anche come *webroot*, è il percorso di base per le risorse statiche pubbliche, ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="bbecd-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="bbecd-278">Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della radice Web e dalle relative sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="bbecd-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="bbecd-279">Il percorso della radice Web è, per impostazione predefinita, *\<radice del contenuto >/wwwroot*, ma è possibile impostare un percorso diverso in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="bbecd-279">The web root path defaults to *\<content root>/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="bbecd-280">Per i file Razor (file con estensione  *CSHTML*), il carattere tilde-barra `~/` punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="bbecd-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="bbecd-281">I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.</span><span class="sxs-lookup"><span data-stu-id="bbecd-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="bbecd-282">Per altre informazioni, vedere [File statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="bbecd-282">For more information, see [Static files](xref:fundamentals/static-files).</span></span>
