---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610324"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="88255-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88255-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="88255-104">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88255-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="88255-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="88255-105">The Startup class</span></span>

<span data-ttu-id="88255-106">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="88255-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="88255-107">Viene eseguita la configurazione di tutti i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="88255-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="88255-108">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="88255-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="88255-109">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="88255-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="88255-110">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="88255-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="88255-111">Un oggetto di contesto Entity Framework Core, ad esempio, è un servizio.</span><span class="sxs-lookup"><span data-stu-id="88255-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="88255-112">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="88255-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="88255-113">La pipeline è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="88255-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="88255-114">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88255-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="88255-115">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="88255-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="88255-116">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="88255-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="88255-117">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="88255-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="88255-118">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="88255-118">Dependency injection (services)</span></span>

<span data-ttu-id="88255-119">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="88255-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="88255-120">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="88255-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="88255-121">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="88255-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="88255-122">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="88255-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="88255-123">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="88255-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="88255-124">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="88255-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="88255-125">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="88255-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="88255-126">Per ulteriori informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="88255-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="88255-127">Middleware</span><span class="sxs-lookup"><span data-stu-id="88255-127">Middleware</span></span>

<span data-ttu-id="88255-128">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="88255-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="88255-129">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="88255-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="88255-130">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="88255-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="88255-131">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="88255-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="88255-132">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="88255-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="88255-133">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="88255-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="88255-134">Per ulteriori informazioni, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="88255-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="88255-135">L'host</span><span class="sxs-lookup"><span data-stu-id="88255-135">The host</span></span>

<span data-ttu-id="88255-136">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="88255-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="88255-137">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="88255-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="88255-138">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="88255-138">An HTTP server implementation</span></span>
* <span data-ttu-id="88255-139">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="88255-139">Middleware components</span></span>
* <span data-ttu-id="88255-140">Registrazione</span><span class="sxs-lookup"><span data-stu-id="88255-140">Logging</span></span>
* <span data-ttu-id="88255-141">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="88255-141">DI</span></span>
* <span data-ttu-id="88255-142">Configurazione</span><span class="sxs-lookup"><span data-stu-id="88255-142">Configuration</span></span>

<span data-ttu-id="88255-143">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="88255-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="88255-144">Il codice usato per creare un host si trova in `Program.Main` e segue il pattern [Builder](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="88255-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="88255-145">Per configurare ogni risorsa che fa parte dell'host vengono chiamati dei metodi.</span><span class="sxs-lookup"><span data-stu-id="88255-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="88255-146">Un metodo builder viene chiamato per combinare il tutto e creare un'istanza dell'oggetto host.</span><span class="sxs-lookup"><span data-stu-id="88255-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88255-147">`CreateHostBuilder` è un nome speciale che identifica il metodo del generatore per i componenti esterni, ad esempio [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="88255-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="88255-148">In ASP.NET Core 3.0 o versioni successive è possibile usare un host generico (classe `Host`) o un host Web (classe `WebHost`) in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="88255-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="88255-149">È consigliato l'uso di un host generico, mentre l'host Web è disponibile per garantire la compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="88255-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="88255-150">Il framework offre i metodi `CreateDefaultBuilder` e `ConfigureWebHostDefaults` per configurare un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="88255-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="88255-151">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="88255-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="88255-152">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88255-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="88255-153">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="88255-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="88255-154">Ecco un esempio di codice per creare un host.</span><span class="sxs-lookup"><span data-stu-id="88255-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="88255-155">I metodi che configurano l'host con le opzioni usate comunemente sono evidenziati:</span><span class="sxs-lookup"><span data-stu-id="88255-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="88255-156">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="88255-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88255-157">`CreateWebHostBuilder` è un nome speciale che identifica il metodo del generatore per i componenti esterni, ad esempio [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="88255-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="88255-158">ASP.NET Core 2.x usa un host Web (classe `WebHost`) per le app Web.</span><span class="sxs-lookup"><span data-stu-id="88255-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="88255-159">Il framework offre `CreateDefaultBuilder` per configurare un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="88255-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="88255-160">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="88255-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="88255-161">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88255-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="88255-162">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="88255-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="88255-163">Ecco un esempio di codice per creare un host:</span><span class="sxs-lookup"><span data-stu-id="88255-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="88255-164">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="88255-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="88255-165">Scenari host avanzati</span><span class="sxs-lookup"><span data-stu-id="88255-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88255-166">L'host generico è disponibile per qualsiasi app .NET Core, non solo per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88255-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="88255-167">L'host generico (classe `Host`) consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="88255-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="88255-168">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="88255-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88255-169">L'host Web è progettato per includere un'implementazione del server HTTP che non è richiesta per altri tipi di app .NET.</span><span class="sxs-lookup"><span data-stu-id="88255-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="88255-170">A partire da ASP.NET Core 2.1, l'host generico (classe `Host`) è disponibile per qualsiasi app .NET Core e non solo per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88255-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="88255-171">L'host generico consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="88255-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="88255-172">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="88255-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="88255-173">È anche possibile usare l'host per eseguire attività in background.</span><span class="sxs-lookup"><span data-stu-id="88255-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="88255-174">Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="88255-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="88255-175">Server</span><span class="sxs-lookup"><span data-stu-id="88255-175">Servers</span></span>

<span data-ttu-id="88255-176">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="88255-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="88255-177">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="88255-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="88255-178">Windows</span><span class="sxs-lookup"><span data-stu-id="88255-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="88255-179">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="88255-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="88255-180">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="88255-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="88255-181">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="88255-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="88255-182">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="88255-183">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="88255-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="88255-184">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="88255-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="88255-185">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="88255-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="88255-186">macOS</span><span class="sxs-lookup"><span data-stu-id="88255-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="88255-187">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="88255-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="88255-188">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="88255-189">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="88255-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="88255-190">Linux</span><span class="sxs-lookup"><span data-stu-id="88255-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="88255-191">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="88255-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="88255-192">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="88255-193">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="88255-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="88255-194">Windows</span><span class="sxs-lookup"><span data-stu-id="88255-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="88255-195">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="88255-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="88255-196">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="88255-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="88255-197">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="88255-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="88255-198">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="88255-199">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="88255-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="88255-200">macOS</span><span class="sxs-lookup"><span data-stu-id="88255-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="88255-201">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="88255-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="88255-202">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="88255-203">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="88255-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="88255-204">Linux</span><span class="sxs-lookup"><span data-stu-id="88255-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="88255-205">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="88255-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="88255-206">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="88255-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="88255-207">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="88255-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="88255-208">Per ulteriori informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="88255-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="88255-209">Configurazione</span><span class="sxs-lookup"><span data-stu-id="88255-209">Configuration</span></span>

<span data-ttu-id="88255-210">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88255-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="88255-211">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="88255-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="88255-212">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="88255-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="88255-213">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="88255-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="88255-214">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="88255-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="88255-215">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="88255-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="88255-216">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="88255-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="88255-217">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="88255-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="88255-218">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="88255-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="88255-219">Opzioni</span><span class="sxs-lookup"><span data-stu-id="88255-219">Options</span></span>

<span data-ttu-id="88255-220">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88255-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="88255-221">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="88255-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="88255-222">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="88255-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="88255-223">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="88255-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="88255-224">Ambienti</span><span class="sxs-lookup"><span data-stu-id="88255-224">Environments</span></span>

<span data-ttu-id="88255-225">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88255-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="88255-226">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="88255-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="88255-227">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="88255-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="88255-228">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="88255-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="88255-229">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="88255-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="88255-230">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="88255-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="88255-231">Registrazione</span><span class="sxs-lookup"><span data-stu-id="88255-231">Logging</span></span>

<span data-ttu-id="88255-232">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="88255-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="88255-233">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="88255-233">Available providers include the following:</span></span>

* <span data-ttu-id="88255-234">Console</span><span class="sxs-lookup"><span data-stu-id="88255-234">Console</span></span>
* <span data-ttu-id="88255-235">Debug</span><span class="sxs-lookup"><span data-stu-id="88255-235">Debug</span></span>
* <span data-ttu-id="88255-236">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="88255-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="88255-237">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="88255-237">Windows Event Log</span></span>
* <span data-ttu-id="88255-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="88255-238">TraceSource</span></span>
* <span data-ttu-id="88255-239">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="88255-239">Azure App Service</span></span>
* <span data-ttu-id="88255-240">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="88255-240">Azure Application Insights</span></span>

<span data-ttu-id="88255-241">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="88255-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="88255-242">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="88255-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="88255-243">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="88255-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="88255-244">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="88255-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="88255-245">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="88255-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="88255-246">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="88255-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="88255-247">Routing</span><span class="sxs-lookup"><span data-stu-id="88255-247">Routing</span></span>

<span data-ttu-id="88255-248">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="88255-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="88255-249">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="88255-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="88255-250">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="88255-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="88255-251">Per ulteriori informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="88255-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="88255-252">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="88255-252">Error handling</span></span>

<span data-ttu-id="88255-253">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="88255-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="88255-254">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="88255-254">A developer exception page</span></span>
* <span data-ttu-id="88255-255">Pagine degli errori personalizzate</span><span class="sxs-lookup"><span data-stu-id="88255-255">Custom error pages</span></span>
* <span data-ttu-id="88255-256">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="88255-256">Static status code pages</span></span>
* <span data-ttu-id="88255-257">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="88255-257">Startup exception handling</span></span>

<span data-ttu-id="88255-258">Per ulteriori informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="88255-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="88255-259">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="88255-259">Make HTTP requests</span></span>

<span data-ttu-id="88255-260">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="88255-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="88255-261">Il factory:</span><span class="sxs-lookup"><span data-stu-id="88255-261">The factory:</span></span>

* <span data-ttu-id="88255-262">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="88255-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="88255-263">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="88255-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="88255-264">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="88255-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="88255-265">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="88255-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="88255-266">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88255-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="88255-267">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="88255-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="88255-268">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="88255-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="88255-269">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="88255-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="88255-270">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="88255-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="88255-271">Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="88255-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="88255-272">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="88255-272">Content root</span></span>

<span data-ttu-id="88255-273">La radice del contenuto è il percorso di base per i contenuti privati usati dall'app, ad esempio i relativi file Razor.</span><span class="sxs-lookup"><span data-stu-id="88255-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="88255-274">Per impostazione predefinita, la radice del contenuto è il percorso di base per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="88255-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="88255-275">È possibile specificare un percorso alternativo in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="88255-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88255-276">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="88255-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88255-277">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="88255-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="88255-278">Radice Web</span><span class="sxs-lookup"><span data-stu-id="88255-278">Web root</span></span>

<span data-ttu-id="88255-279">La radice Web, nota anche come *webroot*, è il percorso di base per le risorse statiche pubbliche, ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="88255-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="88255-280">Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della radice Web e dalle relative sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="88255-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="88255-281">Il percorso della radice Web è, per impostazione predefinita, *{Radice contenuto}/wwwroot*, ma è possibile impostare un percorso diverso in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="88255-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="88255-282">Per i file Razor (file con estensione  *CSHTML*), il carattere tilde-barra `~/` punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="88255-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="88255-283">I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.</span><span class="sxs-lookup"><span data-stu-id="88255-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="88255-284">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="88255-284">For more information, see <xref:fundamentals/static-files>.</span></span>
