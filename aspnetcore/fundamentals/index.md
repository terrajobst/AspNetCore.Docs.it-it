---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 3fbfc7c4c0d5e568339bc00a7cbe84a3932acf1f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664234"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="c069d-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c069d-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="c069d-104">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c069d-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="c069d-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="c069d-105">The Startup class</span></span>

<span data-ttu-id="c069d-106">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c069d-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="c069d-107">Vengono configurati i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="c069d-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="c069d-108">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c069d-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="c069d-109">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="c069d-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="c069d-110">Ad esempio, un componente di registrazione è un servizio.</span><span class="sxs-lookup"><span data-stu-id="c069d-110">For example, a logging component is a service.</span></span> <span data-ttu-id="c069d-111">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c069d-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="c069d-112">La pipeline di gestione delle richieste è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="c069d-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="c069d-113">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c069d-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="c069d-114">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="c069d-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="c069d-115">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c069d-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="c069d-116">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c069d-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="c069d-117">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c069d-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="c069d-118">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="c069d-118">Dependency injection (services)</span></span>

<span data-ttu-id="c069d-119">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="c069d-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="c069d-120">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="c069d-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="c069d-121">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c069d-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="c069d-122">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="c069d-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="c069d-123">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c069d-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="c069d-124">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="c069d-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="c069d-125">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="c069d-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="c069d-126">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c069d-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="c069d-127">Middleware</span><span class="sxs-lookup"><span data-stu-id="c069d-127">Middleware</span></span>

<span data-ttu-id="c069d-128">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="c069d-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="c069d-129">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="c069d-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="c069d-130">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c069d-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="c069d-131">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="c069d-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="c069d-132">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="c069d-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="c069d-133">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c069d-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="c069d-134">Per altre informazioni, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="c069d-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="c069d-135">Host</span><span class="sxs-lookup"><span data-stu-id="c069d-135">Host</span></span>

<span data-ttu-id="c069d-136">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="c069d-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="c069d-137">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="c069d-138">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="c069d-138">An HTTP server implementation</span></span>
* <span data-ttu-id="c069d-139">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="c069d-139">Middleware components</span></span>
* <span data-ttu-id="c069d-140">Registrazione</span><span class="sxs-lookup"><span data-stu-id="c069d-140">Logging</span></span>
* <span data-ttu-id="c069d-141">DI</span><span class="sxs-lookup"><span data-stu-id="c069d-141">DI</span></span>
* <span data-ttu-id="c069d-142">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c069d-142">Configuration</span></span>

<span data-ttu-id="c069d-143">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c069d-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c069d-144">Sono disponibili due host: l'host generico e l'host Web.</span><span class="sxs-lookup"><span data-stu-id="c069d-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="c069d-145">È consigliato l'uso dell'host generico, mentre l'host Web è disponibile solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="c069d-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="c069d-146">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c069d-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="c069d-147">I metodi `CreateDefaultBuilder` e `ConfigureWebHostDefaults` configurano un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="c069d-148">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="c069d-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="c069d-149">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="c069d-150">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="c069d-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="c069d-151">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="c069d-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c069d-152">Sono disponibili due host: l'host Web e l'host generico.</span><span class="sxs-lookup"><span data-stu-id="c069d-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="c069d-153">In ASP.NET Core 2.x, l'host generico è destinato solo agli scenari non Web.</span><span class="sxs-lookup"><span data-stu-id="c069d-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="c069d-154">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c069d-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="c069d-155">Il metodo `CreateDefaultBuilder` configura un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="c069d-156">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="c069d-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="c069d-157">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="c069d-158">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="c069d-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="c069d-159">Per altre informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="c069d-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="c069d-160">Scenari non Web</span><span class="sxs-lookup"><span data-stu-id="c069d-160">Non-web scenarios</span></span>

<span data-ttu-id="c069d-161">L'host generico consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="c069d-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="c069d-162">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="c069d-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="c069d-163">Server</span><span class="sxs-lookup"><span data-stu-id="c069d-163">Servers</span></span>

<span data-ttu-id="c069d-164">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="c069d-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="c069d-165">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c069d-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="c069d-166">Windows</span><span class="sxs-lookup"><span data-stu-id="c069d-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="c069d-167">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="c069d-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="c069d-168">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c069d-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="c069d-169">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="c069d-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="c069d-170">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="c069d-171">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="c069d-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="c069d-172">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="c069d-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="c069d-173">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="c069d-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="c069d-174">macOS</span><span class="sxs-lookup"><span data-stu-id="c069d-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="c069d-175">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="c069d-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="c069d-176">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="c069d-177">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c069d-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="c069d-178">Linux</span><span class="sxs-lookup"><span data-stu-id="c069d-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="c069d-179">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="c069d-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="c069d-180">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="c069d-181">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c069d-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="c069d-182">Windows</span><span class="sxs-lookup"><span data-stu-id="c069d-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="c069d-183">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="c069d-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="c069d-184">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c069d-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="c069d-185">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="c069d-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="c069d-186">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="c069d-187">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="c069d-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="c069d-188">macOS</span><span class="sxs-lookup"><span data-stu-id="c069d-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="c069d-189">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="c069d-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="c069d-190">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="c069d-191">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c069d-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="c069d-192">Linux</span><span class="sxs-lookup"><span data-stu-id="c069d-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="c069d-193">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="c069d-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="c069d-194">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="c069d-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="c069d-195">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="c069d-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="c069d-196">Per altre informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="c069d-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="c069d-197">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c069d-197">Configuration</span></span>

<span data-ttu-id="c069d-198">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="c069d-199">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="c069d-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="c069d-200">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c069d-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="c069d-201">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c069d-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="c069d-202">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c069d-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="c069d-203">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="c069d-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="c069d-204">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c069d-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c069d-205">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="c069d-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="c069d-206">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c069d-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="c069d-207">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c069d-207">Options</span></span>

<span data-ttu-id="c069d-208">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="c069d-209">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="c069d-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="c069d-210">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="c069d-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="c069d-211">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="c069d-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="c069d-212">Ambienti</span><span class="sxs-lookup"><span data-stu-id="c069d-212">Environments</span></span>

<span data-ttu-id="c069d-213">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c069d-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="c069d-214">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c069d-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="c069d-215">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="c069d-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="c069d-216">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c069d-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="c069d-217">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="c069d-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="c069d-218">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c069d-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="c069d-219">Registrazione</span><span class="sxs-lookup"><span data-stu-id="c069d-219">Logging</span></span>

<span data-ttu-id="c069d-220">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="c069d-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="c069d-221">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="c069d-221">Available providers include the following:</span></span>

* <span data-ttu-id="c069d-222">Console</span><span class="sxs-lookup"><span data-stu-id="c069d-222">Console</span></span>
* <span data-ttu-id="c069d-223">Debug</span><span class="sxs-lookup"><span data-stu-id="c069d-223">Debug</span></span>
* <span data-ttu-id="c069d-224">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="c069d-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="c069d-225">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="c069d-225">Windows Event Log</span></span>
* <span data-ttu-id="c069d-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="c069d-226">TraceSource</span></span>
* <span data-ttu-id="c069d-227">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="c069d-227">Azure App Service</span></span>
* <span data-ttu-id="c069d-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="c069d-228">Azure Application Insights</span></span>

<span data-ttu-id="c069d-229">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="c069d-230">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="c069d-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="c069d-231">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="c069d-232">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="c069d-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="c069d-233">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="c069d-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="c069d-234">Per altre informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="c069d-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="c069d-235">Routing.</span><span class="sxs-lookup"><span data-stu-id="c069d-235">Routing</span></span>

<span data-ttu-id="c069d-236">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="c069d-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="c069d-237">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="c069d-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="c069d-238">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="c069d-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="c069d-239">Per altre informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="c069d-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="c069d-240">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="c069d-240">Error handling</span></span>

<span data-ttu-id="c069d-241">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="c069d-242">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="c069d-242">A developer exception page</span></span>
* <span data-ttu-id="c069d-243">Pagine di errore personalizzate</span><span class="sxs-lookup"><span data-stu-id="c069d-243">Custom error pages</span></span>
* <span data-ttu-id="c069d-244">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="c069d-244">Static status code pages</span></span>
* <span data-ttu-id="c069d-245">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="c069d-245">Startup exception handling</span></span>

<span data-ttu-id="c069d-246">Per altre informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="c069d-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="c069d-247">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="c069d-247">Make HTTP requests</span></span>

<span data-ttu-id="c069d-248">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="c069d-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="c069d-249">Il factory:</span><span class="sxs-lookup"><span data-stu-id="c069d-249">The factory:</span></span>

* <span data-ttu-id="c069d-250">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="c069d-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="c069d-251">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="c069d-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="c069d-252">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="c069d-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="c069d-253">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="c069d-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="c069d-254">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c069d-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="c069d-255">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="c069d-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="c069d-256">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="c069d-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="c069d-257">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="c069d-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="c069d-258">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="c069d-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="c069d-259">Per altre informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="c069d-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="c069d-260">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="c069d-260">Content root</span></span>

<span data-ttu-id="c069d-261">La radice del contenuto è il percorso di base di:</span><span class="sxs-lookup"><span data-stu-id="c069d-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="c069d-262">Eseguibile che ospita l'app (*exe*).</span><span class="sxs-lookup"><span data-stu-id="c069d-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="c069d-263">Assembly compilati che costituiscono l'app ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="c069d-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="c069d-264">File di contenuto non di codice usati dall'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="c069d-265">File Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="c069d-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="c069d-266">File di configurazione (con*estensione JSON*, *XML*)</span><span class="sxs-lookup"><span data-stu-id="c069d-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="c069d-267">File di dati ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="c069d-267">Data files (*.db*)</span></span>
* <span data-ttu-id="c069d-268">[Radice Web](#web-root), in genere la cartella *wwwroot* pubblicata.</span><span class="sxs-lookup"><span data-stu-id="c069d-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="c069d-269">Durante lo sviluppo:</span><span class="sxs-lookup"><span data-stu-id="c069d-269">During development:</span></span>

* <span data-ttu-id="c069d-270">Per impostazione predefinita, la radice del contenuto è la directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="c069d-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="c069d-271">La directory radice del progetto viene utilizzata per creare:</span><span class="sxs-lookup"><span data-stu-id="c069d-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="c069d-272">Percorso dei file di contenuto non di codice dell'app nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="c069d-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="c069d-273">[Radice Web](#web-root), in genere la cartella *wwwroot* nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="c069d-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c069d-274">Quando si [Compila l'host](#host), è possibile specificare un percorso radice del contenuto alternativo.</span><span class="sxs-lookup"><span data-stu-id="c069d-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="c069d-275">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="c069d-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c069d-276">Quando si [Compila l'host](#host), è possibile specificare un percorso radice del contenuto alternativo.</span><span class="sxs-lookup"><span data-stu-id="c069d-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="c069d-277">Per altre informazioni, vedere <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="c069d-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="c069d-278">Radice Web</span><span class="sxs-lookup"><span data-stu-id="c069d-278">Web root</span></span>

<span data-ttu-id="c069d-279">La radice Web è il percorso di base per i file di risorse pubblici, non di codice, statici, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c069d-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="c069d-280">Fogli di stile ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="c069d-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="c069d-281">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="c069d-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="c069d-282">Immagini ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="c069d-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="c069d-283">I file statici vengono serviti per impostazione predefinita solo dalla directory radice Web (e dalle sottodirectory).</span><span class="sxs-lookup"><span data-stu-id="c069d-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c069d-284">Per impostazione predefinita, il percorso radice Web è *{Content root}/wwwroot*, ma è possibile specificare una radice Web diversa durante [la compilazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="c069d-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="c069d-285">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="c069d-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c069d-286">Per impostazione predefinita, il percorso radice Web è *{Content root}/wwwroot*, ma è possibile specificare una radice Web diversa durante [la compilazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="c069d-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="c069d-287">Per altre informazioni, vedere [Web root](xref:fundamentals/host/web-host#web-root) (Radice Web).</span><span class="sxs-lookup"><span data-stu-id="c069d-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="c069d-288">Impedire la pubblicazione di file in *wwwroot* con il [contenuto\<> elemento del progetto](/visualstudio/msbuild/common-msbuild-project-items#content) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="c069d-288">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="c069d-289">Nell'esempio seguente viene impedita la pubblicazione di contenuto nella directory *wwwroot/locale* e nelle sottodirectory:</span><span class="sxs-lookup"><span data-stu-id="c069d-289">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c069d-290">Per evitare la pubblicazione di asset di identità statici nella radice Web, vedere <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="c069d-290">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

::: moniker-end

<span data-ttu-id="c069d-291">Nei file Razor (con*estensione cshtml*) la barra tilde (`~/`) punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="c069d-291">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="c069d-292">Un percorso che inizia con `~/` viene definito *percorso virtuale*.</span><span class="sxs-lookup"><span data-stu-id="c069d-292">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="c069d-293">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="c069d-293">For more information, see <xref:fundamentals/static-files>.</span></span>
