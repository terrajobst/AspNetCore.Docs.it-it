---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: a16a2fbb4ad2a79f96b6646ffdc359619d361a25
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434317"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7267f-103">Nozioni fondamentali su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7267f-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7267f-104">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7267f-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="7267f-105">The Startup class</span></span>

<span data-ttu-id="7267f-106">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7267f-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="7267f-107">Vengono configurati i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="7267f-108">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="7267f-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7267f-109">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7267f-110">Ad esempio, un componente di registrazione è un servizio.</span><span class="sxs-lookup"><span data-stu-id="7267f-110">For example, a logging component is a service.</span></span> <span data-ttu-id="7267f-111">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7267f-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7267f-112">La pipeline di gestione delle richieste è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="7267f-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7267f-113">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7267f-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7267f-114">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7267f-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7267f-115">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7267f-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7267f-116">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7267f-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7267f-117">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7267f-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7267f-118">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="7267f-118">Dependency injection (services)</span></span>

<span data-ttu-id="7267f-119">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7267f-120">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="7267f-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7267f-121">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7267f-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7267f-122">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="7267f-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7267f-123">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7267f-124">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="7267f-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7267f-125">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="7267f-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7267f-126">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7267f-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7267f-127">Middleware</span><span class="sxs-lookup"><span data-stu-id="7267f-127">Middleware</span></span>

<span data-ttu-id="7267f-128">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="7267f-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7267f-129">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7267f-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7267f-130">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7267f-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7267f-131">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7267f-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7267f-132">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="7267f-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7267f-133">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7267f-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7267f-134">Per altre informazioni, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7267f-135">Host</span><span class="sxs-lookup"><span data-stu-id="7267f-135">Host</span></span>

<span data-ttu-id="7267f-136">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="7267f-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7267f-137">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7267f-138">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="7267f-138">An HTTP server implementation</span></span>
* <span data-ttu-id="7267f-139">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="7267f-139">Middleware components</span></span>
* <span data-ttu-id="7267f-140">Registrazione</span><span class="sxs-lookup"><span data-stu-id="7267f-140">Logging</span></span>
* <span data-ttu-id="7267f-141">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="7267f-141">DI</span></span>
* <span data-ttu-id="7267f-142">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7267f-142">Configuration</span></span>

<span data-ttu-id="7267f-143">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="7267f-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7267f-144">Sono disponibili due host: l'host generico e l'host Web.</span><span class="sxs-lookup"><span data-stu-id="7267f-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="7267f-145">È consigliato l'uso dell'host generico, mentre l'host Web è disponibile solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="7267f-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="7267f-146">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7267f-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="7267f-147">I metodi `CreateDefaultBuilder` e `ConfigureWebHostDefaults` configurano un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7267f-148">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7267f-149">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7267f-150">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="7267f-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7267f-151">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7267f-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7267f-152">Scenari non Web</span><span class="sxs-lookup"><span data-stu-id="7267f-152">Non-web scenarios</span></span>

<span data-ttu-id="7267f-153">L'host generico consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="7267f-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7267f-154">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7267f-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7267f-155">Server</span><span class="sxs-lookup"><span data-stu-id="7267f-155">Servers</span></span>

<span data-ttu-id="7267f-156">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7267f-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7267f-157">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7267f-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="7267f-158">Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="7267f-159">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="7267f-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7267f-160">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="7267f-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7267f-161">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7267f-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7267f-162">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7267f-163">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-163">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7267f-164">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="7267f-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7267f-165">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7267f-166">macOS</span><span class="sxs-lookup"><span data-stu-id="7267f-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="7267f-167">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-168">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-169">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7267f-170">Linux</span><span class="sxs-lookup"><span data-stu-id="7267f-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="7267f-171">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-172">In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-173">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="7267f-174">Per altre informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7267f-175">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7267f-175">Configuration</span></span>

<span data-ttu-id="7267f-176">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7267f-177">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="7267f-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7267f-178">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7267f-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7267f-179">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7267f-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7267f-180">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7267f-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7267f-181">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="7267f-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7267f-182">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7267f-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7267f-183">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7267f-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7267f-184">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7267f-185">Opzioni</span><span class="sxs-lookup"><span data-stu-id="7267f-185">Options</span></span>

<span data-ttu-id="7267f-186">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7267f-187">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="7267f-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7267f-188">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="7267f-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7267f-189">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7267f-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7267f-190">Ambienti</span><span class="sxs-lookup"><span data-stu-id="7267f-190">Environments</span></span>

<span data-ttu-id="7267f-191">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7267f-192">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="7267f-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7267f-193">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="7267f-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7267f-194">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7267f-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7267f-195">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="7267f-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7267f-196">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7267f-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7267f-197">Registrazione</span><span class="sxs-lookup"><span data-stu-id="7267f-197">Logging</span></span>

<span data-ttu-id="7267f-198">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="7267f-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7267f-199">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="7267f-199">Available providers include the following:</span></span>

* <span data-ttu-id="7267f-200">Console</span><span class="sxs-lookup"><span data-stu-id="7267f-200">Console</span></span>
* <span data-ttu-id="7267f-201">Debug</span><span class="sxs-lookup"><span data-stu-id="7267f-201">Debug</span></span>
* <span data-ttu-id="7267f-202">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="7267f-203">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-203">Windows Event Log</span></span>
* <span data-ttu-id="7267f-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7267f-204">TraceSource</span></span>
* <span data-ttu-id="7267f-205">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="7267f-205">Azure App Service</span></span>
* <span data-ttu-id="7267f-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7267f-206">Azure Application Insights</span></span>

<span data-ttu-id="7267f-207">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7267f-208">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="7267f-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7267f-209">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7267f-210">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="7267f-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7267f-211">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7267f-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7267f-212">Per altre informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7267f-213">Routing</span><span class="sxs-lookup"><span data-stu-id="7267f-213">Routing</span></span>

<span data-ttu-id="7267f-214">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="7267f-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7267f-215">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="7267f-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7267f-216">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7267f-217">Per altre informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7267f-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7267f-218">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="7267f-218">Error handling</span></span>

<span data-ttu-id="7267f-219">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7267f-220">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="7267f-220">A developer exception page</span></span>
* <span data-ttu-id="7267f-221">Pagine degli errori personalizzate</span><span class="sxs-lookup"><span data-stu-id="7267f-221">Custom error pages</span></span>
* <span data-ttu-id="7267f-222">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="7267f-222">Static status code pages</span></span>
* <span data-ttu-id="7267f-223">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="7267f-223">Startup exception handling</span></span>

<span data-ttu-id="7267f-224">Per altre informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7267f-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7267f-225">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="7267f-225">Make HTTP requests</span></span>

<span data-ttu-id="7267f-226">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7267f-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7267f-227">Il factory:</span><span class="sxs-lookup"><span data-stu-id="7267f-227">The factory:</span></span>

* <span data-ttu-id="7267f-228">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="7267f-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7267f-229">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="7267f-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7267f-230">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="7267f-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7267f-231">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="7267f-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7267f-232">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7267f-233">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7267f-234">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="7267f-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7267f-235">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="7267f-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7267f-236">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="7267f-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7267f-237">Per altre informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7267f-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7267f-238">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="7267f-238">Content root</span></span>

<span data-ttu-id="7267f-239">La radice del contenuto è il percorso di base di:</span><span class="sxs-lookup"><span data-stu-id="7267f-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="7267f-240">Eseguibile che ospita l'app (*exe*).</span><span class="sxs-lookup"><span data-stu-id="7267f-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7267f-241">Assembly compilati che costituiscono l'app ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7267f-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7267f-242">File di contenuto non di codice usati dall'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7267f-243">File Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7267f-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7267f-244">File di configurazione (con*estensione JSON*, *XML*)</span><span class="sxs-lookup"><span data-stu-id="7267f-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7267f-245">File di dati ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="7267f-245">Data files (*.db*)</span></span>
* <span data-ttu-id="7267f-246">[Radice Web](#web-root), in genere la cartella *wwwroot* pubblicata.</span><span class="sxs-lookup"><span data-stu-id="7267f-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7267f-247">Durante lo sviluppo:</span><span class="sxs-lookup"><span data-stu-id="7267f-247">During development:</span></span>

* <span data-ttu-id="7267f-248">Per impostazione predefinita, la radice del contenuto è la directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7267f-249">La directory radice del progetto viene utilizzata per creare:</span><span class="sxs-lookup"><span data-stu-id="7267f-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7267f-250">Percorso dei file di contenuto non di codice dell'app nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7267f-251">[Radice Web](#web-root), in genere la cartella *wwwroot* nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7267f-252">Quando si [Compila l'host](#host), è possibile specificare un percorso radice del contenuto alternativo.</span><span class="sxs-lookup"><span data-stu-id="7267f-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7267f-253">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="7267f-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7267f-254">Radice Web</span><span class="sxs-lookup"><span data-stu-id="7267f-254">Web root</span></span>

<span data-ttu-id="7267f-255">La radice Web è il percorso di base per i file di risorse pubblici, non di codice, statici, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7267f-256">Fogli di stile ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="7267f-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7267f-257">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7267f-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7267f-258">Immagini ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7267f-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7267f-259">I file statici vengono serviti per impostazione predefinita solo dalla directory radice Web (e dalle sottodirectory).</span><span class="sxs-lookup"><span data-stu-id="7267f-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7267f-260">Per impostazione predefinita, il percorso radice Web è *{Content root}/wwwroot*, ma è possibile specificare una radice Web diversa durante [la compilazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="7267f-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7267f-261">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="7267f-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="7267f-262">Impedire la pubblicazione di file in *wwwroot* con il [contenuto\<> elemento del progetto](/visualstudio/msbuild/common-msbuild-project-items#content) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7267f-263">Nell'esempio seguente viene impedita la pubblicazione di contenuto nella directory *wwwroot/locale* e nelle sottodirectory:</span><span class="sxs-lookup"><span data-stu-id="7267f-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7267f-264">Per evitare la pubblicazione di asset di identità statici nella radice Web, vedere <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="7267f-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="7267f-265">Nei file Razor (con*estensione cshtml*) la barra tilde (`~/`) punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="7267f-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7267f-266">Un percorso che inizia con `~/` viene definito *percorso virtuale*.</span><span class="sxs-lookup"><span data-stu-id="7267f-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7267f-267">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7267f-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7267f-268">Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7267f-269">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="7267f-269">The Startup class</span></span>

<span data-ttu-id="7267f-270">All'interno della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7267f-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="7267f-271">Vengono configurati i servizi necessari per l'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="7267f-272">Viene definita la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="7267f-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7267f-273">I *servizi* sono componenti usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7267f-274">Ad esempio, un componente di registrazione è un servizio.</span><span class="sxs-lookup"><span data-stu-id="7267f-274">For example, a logging component is a service.</span></span> <span data-ttu-id="7267f-275">Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7267f-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7267f-276">La pipeline di gestione delle richieste è strutturata come una serie di componenti *middleware*.</span><span class="sxs-lookup"><span data-stu-id="7267f-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7267f-277">Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7267f-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7267f-278">Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7267f-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7267f-279">Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7267f-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7267f-280">Ecco un esempio di classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7267f-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7267f-281">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7267f-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7267f-282">Iniezione di dipendenze (servizi)</span><span class="sxs-lookup"><span data-stu-id="7267f-282">Dependency injection (services)</span></span>

<span data-ttu-id="7267f-283">ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7267f-284">Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto.</span><span class="sxs-lookup"><span data-stu-id="7267f-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7267f-285">Il parametro può essere il tipo di servizio o un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7267f-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7267f-286">Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.</span><span class="sxs-lookup"><span data-stu-id="7267f-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7267f-287">Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7267f-288">La riga evidenziata è un esempio di inserimento costruttore:</span><span class="sxs-lookup"><span data-stu-id="7267f-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7267f-289">Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.</span><span class="sxs-lookup"><span data-stu-id="7267f-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7267f-290">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7267f-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7267f-291">Middleware</span><span class="sxs-lookup"><span data-stu-id="7267f-291">Middleware</span></span>

<span data-ttu-id="7267f-292">La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware.</span><span class="sxs-lookup"><span data-stu-id="7267f-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7267f-293">Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.</span><span class="sxs-lookup"><span data-stu-id="7267f-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7267f-294">Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7267f-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7267f-295">Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7267f-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7267f-296">Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:</span><span class="sxs-lookup"><span data-stu-id="7267f-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7267f-297">ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7267f-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7267f-298">Per altre informazioni, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7267f-299">Host</span><span class="sxs-lookup"><span data-stu-id="7267f-299">Host</span></span>

<span data-ttu-id="7267f-300">Un'app ASP.NET Core crea un *host* all'avvio.</span><span class="sxs-lookup"><span data-stu-id="7267f-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7267f-301">L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7267f-302">Un'implementazione del server HTTP</span><span class="sxs-lookup"><span data-stu-id="7267f-302">An HTTP server implementation</span></span>
* <span data-ttu-id="7267f-303">I componenti middleware</span><span class="sxs-lookup"><span data-stu-id="7267f-303">Middleware components</span></span>
* <span data-ttu-id="7267f-304">Registrazione</span><span class="sxs-lookup"><span data-stu-id="7267f-304">Logging</span></span>
* <span data-ttu-id="7267f-305">Inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="7267f-305">DI</span></span>
* <span data-ttu-id="7267f-306">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7267f-306">Configuration</span></span>

<span data-ttu-id="7267f-307">Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.</span><span class="sxs-lookup"><span data-stu-id="7267f-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7267f-308">Sono disponibili due host: l'host Web e l'host generico.</span><span class="sxs-lookup"><span data-stu-id="7267f-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="7267f-309">In ASP.NET Core 2.x, l'host generico è destinato solo agli scenari non Web.</span><span class="sxs-lookup"><span data-stu-id="7267f-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="7267f-310">Il codice per creare un host si trova in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7267f-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="7267f-311">Il metodo `CreateDefaultBuilder` configura un host con le opzioni usate comunemente, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7267f-312">Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7267f-313">Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente, argomenti della riga di comando e altri origini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7267f-314">Inviare l'output di registrazione alla console e ai provider di debug.</span><span class="sxs-lookup"><span data-stu-id="7267f-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7267f-315">Per altre informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="7267f-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7267f-316">Scenari non Web</span><span class="sxs-lookup"><span data-stu-id="7267f-316">Non-web scenarios</span></span>

<span data-ttu-id="7267f-317">L'host generico consente ad altri tipi di app di usare estensioni del framework trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app.</span><span class="sxs-lookup"><span data-stu-id="7267f-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7267f-318">Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7267f-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7267f-319">Server</span><span class="sxs-lookup"><span data-stu-id="7267f-319">Servers</span></span>

<span data-ttu-id="7267f-320">Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7267f-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7267f-321">Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7267f-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7267f-322">Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="7267f-323">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="7267f-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7267f-324">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="7267f-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7267f-325">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7267f-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7267f-326">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7267f-327">*Server HTTP IIS* è un server per Windows che usa IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7267f-328">Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="7267f-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7267f-329">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7267f-330">macOS</span><span class="sxs-lookup"><span data-stu-id="7267f-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="7267f-331">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-332">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-333">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7267f-334">Linux</span><span class="sxs-lookup"><span data-stu-id="7267f-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="7267f-335">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-336">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-337">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7267f-338">Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="7267f-339">ASP.NET Core include le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="7267f-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7267f-340">*Kestrel* è un server Web multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="7267f-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7267f-341">Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7267f-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7267f-342">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7267f-343">*HTTP.sys* è un server per Windows che non viene usato con IIS.</span><span class="sxs-lookup"><span data-stu-id="7267f-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7267f-344">macOS</span><span class="sxs-lookup"><span data-stu-id="7267f-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="7267f-345">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-346">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-347">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7267f-348">Linux</span><span class="sxs-lookup"><span data-stu-id="7267f-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="7267f-349">ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7267f-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7267f-350">Il gheppio può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="7267f-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7267f-351">Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7267f-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7267f-352">Per altre informazioni, vedere <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7267f-353">Configurazione</span><span class="sxs-lookup"><span data-stu-id="7267f-353">Configuration</span></span>

<span data-ttu-id="7267f-354">ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7267f-355">Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="7267f-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7267f-356">È anche possibile scrivere provider di configurazione personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7267f-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7267f-357">Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7267f-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7267f-358">Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7267f-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7267f-359">Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.</span><span class="sxs-lookup"><span data-stu-id="7267f-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7267f-360">Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7267f-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7267f-361">Per i segreti di produzione, si consiglia di usare [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7267f-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7267f-362">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7267f-363">Opzioni</span><span class="sxs-lookup"><span data-stu-id="7267f-363">Options</span></span>

<span data-ttu-id="7267f-364">Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7267f-365">Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="7267f-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7267f-366">Il codice seguente, ad esempio, imposta le opzioni WebSockets:</span><span class="sxs-lookup"><span data-stu-id="7267f-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7267f-367">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7267f-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7267f-368">Ambienti</span><span class="sxs-lookup"><span data-stu-id="7267f-368">Environments</span></span>

<span data-ttu-id="7267f-369">Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7267f-370">È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="7267f-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7267f-371">ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="7267f-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7267f-372">L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7267f-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7267f-373">Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="7267f-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7267f-374">Per altre informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7267f-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7267f-375">Registrazione</span><span class="sxs-lookup"><span data-stu-id="7267f-375">Logging</span></span>

<span data-ttu-id="7267f-376">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="7267f-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7267f-377">Tra i provider disponibili sono inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="7267f-377">Available providers include the following:</span></span>

* <span data-ttu-id="7267f-378">Console</span><span class="sxs-lookup"><span data-stu-id="7267f-378">Console</span></span>
* <span data-ttu-id="7267f-379">Debug</span><span class="sxs-lookup"><span data-stu-id="7267f-379">Debug</span></span>
* <span data-ttu-id="7267f-380">Event Tracing for Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="7267f-381">Registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="7267f-381">Windows Event Log</span></span>
* <span data-ttu-id="7267f-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7267f-382">TraceSource</span></span>
* <span data-ttu-id="7267f-383">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="7267f-383">Azure App Service</span></span>
* <span data-ttu-id="7267f-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7267f-384">Azure Application Insights</span></span>

<span data-ttu-id="7267f-385">Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7267f-386">Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.</span><span class="sxs-lookup"><span data-stu-id="7267f-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7267f-387">L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7267f-388">I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="7267f-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7267f-389">Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7267f-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7267f-390">Per altre informazioni, vedere <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7267f-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7267f-391">Routing</span><span class="sxs-lookup"><span data-stu-id="7267f-391">Routing</span></span>

<span data-ttu-id="7267f-392">Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore.</span><span class="sxs-lookup"><span data-stu-id="7267f-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7267f-393">Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware.</span><span class="sxs-lookup"><span data-stu-id="7267f-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7267f-394">Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.</span><span class="sxs-lookup"><span data-stu-id="7267f-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7267f-395">Per altre informazioni, vedere <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7267f-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7267f-396">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="7267f-396">Error handling</span></span>

<span data-ttu-id="7267f-397">ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7267f-398">Una pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="7267f-398">A developer exception page</span></span>
* <span data-ttu-id="7267f-399">Pagine degli errori personalizzate</span><span class="sxs-lookup"><span data-stu-id="7267f-399">Custom error pages</span></span>
* <span data-ttu-id="7267f-400">Pagine dei codici di stato statiche</span><span class="sxs-lookup"><span data-stu-id="7267f-400">Static status code pages</span></span>
* <span data-ttu-id="7267f-401">Gestione delle eccezioni durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="7267f-401">Startup exception handling</span></span>

<span data-ttu-id="7267f-402">Per altre informazioni, vedere <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7267f-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7267f-403">Creare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="7267f-403">Make HTTP requests</span></span>

<span data-ttu-id="7267f-404">Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7267f-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7267f-405">Il factory:</span><span class="sxs-lookup"><span data-stu-id="7267f-405">The factory:</span></span>

* <span data-ttu-id="7267f-406">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="7267f-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7267f-407">Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="7267f-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7267f-408">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="7267f-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7267f-409">Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="7267f-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7267f-410">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7267f-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7267f-411">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="7267f-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7267f-412">Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="7267f-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7267f-413">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="7267f-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7267f-414">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="7267f-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7267f-415">Per altre informazioni, vedere <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7267f-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7267f-416">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="7267f-416">Content root</span></span>

<span data-ttu-id="7267f-417">La radice del contenuto è il percorso di base di:</span><span class="sxs-lookup"><span data-stu-id="7267f-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="7267f-418">Eseguibile che ospita l'app (*exe*).</span><span class="sxs-lookup"><span data-stu-id="7267f-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7267f-419">Assembly compilati che costituiscono l'app ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7267f-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7267f-420">File di contenuto non di codice usati dall'app, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7267f-421">File Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7267f-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7267f-422">File di configurazione (con*estensione JSON*, *XML*)</span><span class="sxs-lookup"><span data-stu-id="7267f-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7267f-423">File di dati ( *. DB*)</span><span class="sxs-lookup"><span data-stu-id="7267f-423">Data files (*.db*)</span></span>
* <span data-ttu-id="7267f-424">[Radice Web](#web-root), in genere la cartella *wwwroot* pubblicata.</span><span class="sxs-lookup"><span data-stu-id="7267f-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7267f-425">Durante lo sviluppo:</span><span class="sxs-lookup"><span data-stu-id="7267f-425">During development:</span></span>

* <span data-ttu-id="7267f-426">Per impostazione predefinita, la radice del contenuto è la directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7267f-427">La directory radice del progetto viene utilizzata per creare:</span><span class="sxs-lookup"><span data-stu-id="7267f-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7267f-428">Percorso dei file di contenuto non di codice dell'app nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7267f-429">[Radice Web](#web-root), in genere la cartella *wwwroot* nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7267f-430">Quando si [Compila l'host](#host), è possibile specificare un percorso radice del contenuto alternativo.</span><span class="sxs-lookup"><span data-stu-id="7267f-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7267f-431">Per altre informazioni, vedere <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="7267f-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7267f-432">Radice Web</span><span class="sxs-lookup"><span data-stu-id="7267f-432">Web root</span></span>

<span data-ttu-id="7267f-433">La radice Web è il percorso di base per i file di risorse pubblici, non di codice, statici, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7267f-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7267f-434">Fogli di stile ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="7267f-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7267f-435">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7267f-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7267f-436">Immagini ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7267f-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7267f-437">I file statici vengono serviti per impostazione predefinita solo dalla directory radice Web (e dalle sottodirectory).</span><span class="sxs-lookup"><span data-stu-id="7267f-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7267f-438">Per impostazione predefinita, il percorso radice Web è *{Content root}/wwwroot*, ma è possibile specificare una radice Web diversa durante [la compilazione dell'host](#host).</span><span class="sxs-lookup"><span data-stu-id="7267f-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7267f-439">Per altre informazioni, vedere [Web root](xref:fundamentals/host/web-host#web-root) (Radice Web).</span><span class="sxs-lookup"><span data-stu-id="7267f-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="7267f-440">Impedire la pubblicazione di file in *wwwroot* con il [contenuto\<> elemento del progetto](/visualstudio/msbuild/common-msbuild-project-items#content) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="7267f-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7267f-441">Nell'esempio seguente viene impedita la pubblicazione di contenuto nella directory *wwwroot/locale* e nelle sottodirectory:</span><span class="sxs-lookup"><span data-stu-id="7267f-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7267f-442">Nei file Razor (con*estensione cshtml*) la barra tilde (`~/`) punta alla radice Web.</span><span class="sxs-lookup"><span data-stu-id="7267f-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7267f-443">Un percorso che inizia con `~/` viene definito *percorso virtuale*.</span><span class="sxs-lookup"><span data-stu-id="7267f-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7267f-444">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7267f-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end