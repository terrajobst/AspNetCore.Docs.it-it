---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: fundamentals/index
ms.openlocfilehash: cff2afd62ed60648dc689d408dde56ecda18c261
ms.sourcegitcommit: 2d4c1732c4866ed26b83da35f7bc2ad021a9c701
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2019
ms.locfileid: "70815644"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="0f4f6-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f4f6-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="0f4f6-104">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="0f4f6-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="0f4f6-105">The Startup class</span></span>

<span data-ttu-id="0f4f6-106">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="0f4f6-107">Vengono configurati i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="0f4f6-108">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="0f4f6-109">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="0f4f6-110">Ad esempio, un componente di registrazione è un servizio.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-110">For example, a logging component is a service.</span></span> <span data-ttu-id="0f4f6-111">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="0f4f6-112">La pipeline di gestione delle richieste è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="0f4f6-113">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="0f4f6-114">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="0f4f6-115">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="0f4f6-116">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="0f4f6-117">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="0f4f6-118">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="0f4f6-118">Dependency injection (services)</span></span>

<span data-ttu-id="0f4f6-119">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="0f4f6-120">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="0f4f6-121">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="0f4f6-122">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="0f4f6-123">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="0f4f6-124">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="0f4f6-125">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="0f4f6-126">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="0f4f6-127">Middleware</span><span class="sxs-lookup"><span data-stu-id="0f4f6-127">Middleware</span></span>

<span data-ttu-id="0f4f6-128">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="0f4f6-129">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="0f4f6-130">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="0f4f6-131">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="0f4f6-132">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="0f4f6-133">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="0f4f6-134">Per altre informazioni, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="0f4f6-135">Host</span><span class="sxs-lookup"><span data-stu-id="0f4f6-135">Host</span></span>

<span data-ttu-id="0f4f6-136">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="0f4f6-137">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="0f4f6-138">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="0f4f6-138">An HTTP server implementation</span></span>
* <span data-ttu-id="0f4f6-139">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="0f4f6-139">Middleware components</span></span>
* <span data-ttu-id="0f4f6-140">Registrazione</span><span class="sxs-lookup"><span data-stu-id="0f4f6-140">Logging</span></span>
* <span data-ttu-id="0f4f6-141">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="0f4f6-141">DI</span></span>
* <span data-ttu-id="0f4f6-142">Configurazione</span><span class="sxs-lookup"><span data-stu-id="0f4f6-142">Configuration</span></span>

<span data-ttu-id="0f4f6-143">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f4f6-144">Sono disponibili due host: l'host generico e l'host Web.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="0f4f6-145">È consigliato l'uso dell'host generico, mentre l'host Web è disponibile solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="0f4f6-146">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="0f4f6-147">I metodi `CreateDefaultBuilder` e `ConfigureWebHostDefaults` configurano un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="0f4f6-148">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="0f4f6-149">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="0f4f6-150">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="0f4f6-151">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0f4f6-152">Sono disponibili due host: l'host Web e l'host generico.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="0f4f6-153">In ASP.NET Core 2.x, l'host generico è destinato solo agli scenari non Web.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="0f4f6-154">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="0f4f6-155">Il metodo `CreateDefaultBuilder` configura un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="0f4f6-156">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="0f4f6-157">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="0f4f6-158">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="0f4f6-159">Per altre informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="0f4f6-160">Scenari non Web</span><span class="sxs-lookup"><span data-stu-id="0f4f6-160">Non-web scenarios</span></span>

<span data-ttu-id="0f4f6-161">L'host generico consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="0f4f6-162">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="0f4f6-163">Server</span><span class="sxs-lookup"><span data-stu-id="0f4f6-163">Servers</span></span>

<span data-ttu-id="0f4f6-164">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="0f4f6-165">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="0f4f6-166">Windows</span><span class="sxs-lookup"><span data-stu-id="0f4f6-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="0f4f6-167">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="0f4f6-168">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="0f4f6-169">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="0f4f6-170">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="0f4f6-171">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="0f4f6-172">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="0f4f6-173">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0f4f6-174">macOS</span><span class="sxs-lookup"><span data-stu-id="0f4f6-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="0f4f6-175">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="0f4f6-176">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="0f4f6-177">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0f4f6-178">Linux</span><span class="sxs-lookup"><span data-stu-id="0f4f6-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="0f4f6-179">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="0f4f6-180">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="0f4f6-181">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="0f4f6-182">Windows</span><span class="sxs-lookup"><span data-stu-id="0f4f6-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="0f4f6-183">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="0f4f6-184">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="0f4f6-185">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="0f4f6-186">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="0f4f6-187">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0f4f6-188">macOS</span><span class="sxs-lookup"><span data-stu-id="0f4f6-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="0f4f6-189">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="0f4f6-190">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="0f4f6-191">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0f4f6-192">Linux</span><span class="sxs-lookup"><span data-stu-id="0f4f6-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="0f4f6-193">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="0f4f6-194">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="0f4f6-195">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="0f4f6-196">Per altre informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="0f4f6-197">Configurazione</span><span class="sxs-lookup"><span data-stu-id="0f4f6-197">Configuration</span></span>

<span data-ttu-id="0f4f6-198">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="0f4f6-199">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="0f4f6-200">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="0f4f6-201">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="0f4f6-202">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="0f4f6-203">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="0f4f6-204">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="0f4f6-205">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="0f4f6-206">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="0f4f6-207">Opzioni</span><span class="sxs-lookup"><span data-stu-id="0f4f6-207">Options</span></span>

<span data-ttu-id="0f4f6-208">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="0f4f6-209">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="0f4f6-210">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="0f4f6-211">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="0f4f6-212">Ambienti</span><span class="sxs-lookup"><span data-stu-id="0f4f6-212">Environments</span></span>

<span data-ttu-id="0f4f6-213">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="0f4f6-214">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0f4f6-215">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="0f4f6-216">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="0f4f6-217">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="0f4f6-218">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="0f4f6-219">Registrazione</span><span class="sxs-lookup"><span data-stu-id="0f4f6-219">Logging</span></span>

<span data-ttu-id="0f4f6-220">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="0f4f6-221">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-221">Available providers include the following:</span></span>

* <span data-ttu-id="0f4f6-222">Console</span><span class="sxs-lookup"><span data-stu-id="0f4f6-222">Console</span></span>
* <span data-ttu-id="0f4f6-223">Debug</span><span class="sxs-lookup"><span data-stu-id="0f4f6-223">Debug</span></span>
* <span data-ttu-id="0f4f6-224">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="0f4f6-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="0f4f6-225">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="0f4f6-225">Windows Event Log</span></span>
* <span data-ttu-id="0f4f6-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0f4f6-226">TraceSource</span></span>
* <span data-ttu-id="0f4f6-227">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="0f4f6-227">Azure App Service</span></span>
* <span data-ttu-id="0f4f6-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="0f4f6-228">Azure Application Insights</span></span>

<span data-ttu-id="0f4f6-229">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="0f4f6-230">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="0f4f6-231">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="0f4f6-232">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="0f4f6-233">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="0f4f6-234">Per altre informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="0f4f6-235">Routing</span><span class="sxs-lookup"><span data-stu-id="0f4f6-235">Routing</span></span>

<span data-ttu-id="0f4f6-236">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="0f4f6-237">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="0f4f6-238">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="0f4f6-239">Per altre informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="0f4f6-240">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="0f4f6-240">Error handling</span></span>

<span data-ttu-id="0f4f6-241">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="0f4f6-242">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="0f4f6-242">A developer exception page</span></span>
* <span data-ttu-id="0f4f6-243">Pagine degli errori personalizzate</span><span class="sxs-lookup"><span data-stu-id="0f4f6-243">Custom error pages</span></span>
* <span data-ttu-id="0f4f6-244">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="0f4f6-244">Static status code pages</span></span>
* <span data-ttu-id="0f4f6-245">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="0f4f6-245">Startup exception handling</span></span>

<span data-ttu-id="0f4f6-246">Per altre informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="0f4f6-247">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="0f4f6-247">Make HTTP requests</span></span>

<span data-ttu-id="0f4f6-248">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="0f4f6-249">Il factory:</span><span class="sxs-lookup"><span data-stu-id="0f4f6-249">The factory:</span></span>

* <span data-ttu-id="0f4f6-250">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="0f4f6-251">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="0f4f6-252">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="0f4f6-253">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="0f4f6-254">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="0f4f6-255">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="0f4f6-256">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="0f4f6-257">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="0f4f6-258">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="0f4f6-259">Per altre informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="0f4f6-260">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="0f4f6-260">Content root</span></span>

<span data-ttu-id="0f4f6-261">La radice del contenuto è il percorso di base per i contenuti privati usati dall'app, ad esempio i relativi file Razor.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-261">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="0f4f6-262">Per impostazione predefinita, la radice del contenuto è il percorso di base per il file eseguibile che ospita l'app.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-262">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="0f4f6-263">È possibile specificare un percorso alternativo in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-263">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f4f6-264">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-264">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0f4f6-265">Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-265">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="0f4f6-266">Radice Web</span><span class="sxs-lookup"><span data-stu-id="0f4f6-266">Web root</span></span>

<span data-ttu-id="0f4f6-267">La radice Web, nota anche come *webroot*, è il percorso di base per le risorse statiche pubbliche, ad esempio CSS, JavaScript e i file di immagine.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-267">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="0f4f6-268">Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della radice Web e dalle relative sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-268">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="0f4f6-269">Il percorso della radice Web è, per impostazione predefinita, *{Radice contenuto}/wwwroot*, ma è possibile impostare un percorso diverso in fase di [creazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-269">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0f4f6-270">Per ulteriori informazioni, vedere [Webroot](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#webroot)</span><span class="sxs-lookup"><span data-stu-id="0f4f6-270">For more information, see [WebRoot](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#webroot)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0f4f6-271">Per altre informazioni, vedere [Web root](/aspnet/core/fundamentals/host/web-host#webroot) (Radice Web).</span><span class="sxs-lookup"><span data-stu-id="0f4f6-271">For more information, see [Web root](/aspnet/core/fundamentals/host/web-host#webroot).</span></span>

::: moniker-end

<span data-ttu-id="0f4f6-272">Per i file Razor (file con estensione  *CSHTML*), il carattere tilde-barra `~/` punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-272">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="0f4f6-273">I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-273">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="0f4f6-274">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="0f4f6-274">For more information, see <xref:fundamentals/static-files>.</span></span>
