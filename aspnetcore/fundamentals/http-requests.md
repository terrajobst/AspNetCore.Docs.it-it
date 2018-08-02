---
title: Inizializzare richieste HTTP
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 87424eaea499ba7ece1e5ef88649fcbb2e297635
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320655"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="6cd71-103">Inizializzare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="6cd71-103">Initiate HTTP requests</span></span>

<span data-ttu-id="6cd71-104">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="6cd71-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="6cd71-105">È possibile registrare e usare [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) per configurare e creare istanze di [HttpClient](/dotnet/api/system.net.http.httpclient) in un'app.</span><span class="sxs-lookup"><span data-stu-id="6cd71-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="6cd71-106">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="6cd71-106">It offers the following benefits:</span></span>

* <span data-ttu-id="6cd71-107">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="6cd71-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="6cd71-108">Ad esempio, è possibile registrare e configurare un client "github" per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="6cd71-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="6cd71-109">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="6cd71-110">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="6cd71-111">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="6cd71-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="6cd71-112">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="6cd71-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="6cd71-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6cd71-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cd71-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="6cd71-114">Prerequisites</span></span>

<span data-ttu-id="6cd71-115">I progetti destinati a .NET Framework richiedono l'installazione del pacchetto NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="6cd71-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="6cd71-116">I progetti destinati a .NET Core che fanno riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage-app) sono già inclusi nel pacchetto `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="6cd71-117">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="6cd71-117">Consumption patterns</span></span>

<span data-ttu-id="6cd71-118">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="6cd71-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="6cd71-119">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="6cd71-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="6cd71-120">Client denominati</span><span class="sxs-lookup"><span data-stu-id="6cd71-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="6cd71-121">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="6cd71-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="6cd71-122">Client generati</span><span class="sxs-lookup"><span data-stu-id="6cd71-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="6cd71-123">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="6cd71-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="6cd71-124">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="6cd71-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="6cd71-125">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="6cd71-125">Basic usage</span></span>

<span data-ttu-id="6cd71-126">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="6cd71-127">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6cd71-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="6cd71-128">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="6cd71-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="6cd71-129">Questo uso di `IHttpClientFactory` è un ottimo modo per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="6cd71-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="6cd71-130">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="6cd71-131">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="6cd71-132">Client denominati</span><span class="sxs-lookup"><span data-stu-id="6cd71-132">Named clients</span></span>

<span data-ttu-id="6cd71-133">Se un'app richiede più usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="6cd71-133">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="6cd71-134">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="6cd71-135">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome "github".</span><span class="sxs-lookup"><span data-stu-id="6cd71-135">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="6cd71-136">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="6cd71-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="6cd71-137">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="6cd71-138">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="6cd71-139">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="6cd71-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="6cd71-140">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="6cd71-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="6cd71-141">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="6cd71-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="6cd71-142">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="6cd71-142">Typed clients</span></span>

<span data-ttu-id="6cd71-143">I client tipizzati offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="6cd71-144">L'approccio basato su client tipizzati offre l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="6cd71-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="6cd71-145">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="6cd71-146">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="6cd71-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="6cd71-147">Un altro vantaggio è rappresentato dalla possibilità di usare l'inserimento di dipendenze e di inserirli dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="6cd71-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="6cd71-148">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="6cd71-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6cd71-149">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="6cd71-150">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="6cd71-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="6cd71-151">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="6cd71-152">Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="6cd71-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="6cd71-153">Per registrare un client tipizzato è possibile usare il metodo di estensione `AddHttpClient` generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="6cd71-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="6cd71-154">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6cd71-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="6cd71-155">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="6cd71-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="6cd71-156">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="6cd71-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="6cd71-157">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="6cd71-158">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="6cd71-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="6cd71-159">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="6cd71-160">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="6cd71-161">Client generati</span><span class="sxs-lookup"><span data-stu-id="6cd71-161">Generated clients</span></span>

<span data-ttu-id="6cd71-162">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="6cd71-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="6cd71-163">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="6cd71-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="6cd71-164">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="6cd71-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="6cd71-165">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="6cd71-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="6cd71-166">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="6cd71-166">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="6cd71-167">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="6cd71-167">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="6cd71-168">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="6cd71-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="6cd71-169">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="6cd71-169">Outgoing request middleware</span></span>

<span data-ttu-id="6cd71-170">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="6cd71-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="6cd71-171">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="6cd71-172">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="6cd71-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="6cd71-173">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="6cd71-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="6cd71-174">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6cd71-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="6cd71-175">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="6cd71-176">Per creare un gestore, definire una classe che deriva da `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="6cd71-177">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="6cd71-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="6cd71-178">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="6cd71-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="6cd71-179">Verifica se un'intestazione X-API-KEY è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="6cd71-179">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="6cd71-180">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="6cd71-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="6cd71-181">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="6cd71-182">Questa attività viene eseguita tramite metodi di estensione in `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-182">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="6cd71-183">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6cd71-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="6cd71-184">Il gestore **deve** essere registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6cd71-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="6cd71-185">Dopo la registrazione è possibile chiamare `AddHttpMessageHandler`, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="6cd71-185">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="6cd71-186">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="6cd71-187">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="6cd71-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="6cd71-188">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="6cd71-188">Use Polly-based handlers</span></span>

<span data-ttu-id="6cd71-189">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="6cd71-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="6cd71-190">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="6cd71-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="6cd71-191">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="6cd71-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="6cd71-192">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="6cd71-193">Le estensioni per Polly sono disponibili nel pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="6cd71-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="6cd71-194">Il pacchetto non è incluso nel metapacchetto [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6cd71-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6cd71-195">Per usare le estensioni, nel progetto deve essere incluso un elemento `<PackageReference />` esplicito.</span><span class="sxs-lookup"><span data-stu-id="6cd71-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="6cd71-196">Dopo il ripristino di questo pacchetto, i metodi di estensione che supportano l'aggiunta di gestori basati su Polly ai client risultano disponibili.</span><span class="sxs-lookup"><span data-stu-id="6cd71-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="6cd71-197">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="6cd71-197">Handle transient faults</span></span>

<span data-ttu-id="6cd71-198">Gli errori più comuni che si prevede possano verificarsi quando si effettuano chiamate HTTP esterne saranno temporanei.</span><span class="sxs-lookup"><span data-stu-id="6cd71-198">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="6cd71-199">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="6cd71-200">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="6cd71-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="6cd71-201">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6cd71-202">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="6cd71-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="6cd71-203">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="6cd71-204">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="6cd71-205">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="6cd71-205">Dynamically select policies</span></span>

<span data-ttu-id="6cd71-206">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="6cd71-207">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="6cd71-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="6cd71-208">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="6cd71-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="6cd71-209">Nel codice precedente se la richiesta in uscita è un'operazione GET, viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="6cd71-210">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="6cd71-211">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="6cd71-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="6cd71-212">Una pratica comune per offrire una funzionalità avanzata è quella di annidare i criteri Polly:</span><span class="sxs-lookup"><span data-stu-id="6cd71-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="6cd71-213">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="6cd71-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="6cd71-214">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="6cd71-215">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="6cd71-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="6cd71-216">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="6cd71-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="6cd71-217">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="6cd71-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="6cd71-218">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="6cd71-219">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="6cd71-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="6cd71-220">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="6cd71-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="6cd71-221">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="6cd71-222">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6cd71-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="6cd71-223">Nel codice precedente viene aggiunto un elemento PolicyRegistry a `ServiceCollection` e vengono registrati due criteri.</span><span class="sxs-lookup"><span data-stu-id="6cd71-223">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="6cd71-224">Per usare un criterio del registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="6cd71-224">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="6cd71-225">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="6cd71-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="6cd71-226">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="6cd71-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="6cd71-227">Ogni volta che `CreateClient` viene chiamato in `IHttpClientFactory` viene restituita una nuova istanza di un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-227">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="6cd71-228">Per ogni client denominato sarà presente un elemento `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-228">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="6cd71-229">`IHttpClientFactory` eseguirà il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="6cd71-229">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="6cd71-230">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="6cd71-230">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="6cd71-231">Il pooling dei gestori è opportuno poiché ogni gestore si occupa in genere di gestire le proprie connessioni HTTP sottostanti. La creazione di un numero superiore di gestori rispetto a quelli necessari può determinare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="6cd71-232">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="6cd71-232">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="6cd71-233">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="6cd71-233">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="6cd71-234">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-234">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="6cd71-235">Per eseguire l'override, chiamare `SetHandlerLifetime` nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="6cd71-235">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="6cd71-236">Registrazione</span><span class="sxs-lookup"><span data-stu-id="6cd71-236">Logging</span></span>

<span data-ttu-id="6cd71-237">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="6cd71-237">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="6cd71-238">Per visualizzare i messaggi di log predefiniti è necessario abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="6cd71-238">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="6cd71-239">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="6cd71-239">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="6cd71-240">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="6cd71-240">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="6cd71-241">Un client denominato "MyNamedClient", ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-241">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="6cd71-242">I messaggi con il suffisso "LogicalHandler" sono all'esterno della pipeline del gestore richieste.</span><span class="sxs-lookup"><span data-stu-id="6cd71-242">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="6cd71-243">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="6cd71-243">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="6cd71-244">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="6cd71-244">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="6cd71-245">La registrazione avviene anche all'interno della pipeline del gestore richieste.</span><span class="sxs-lookup"><span data-stu-id="6cd71-245">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="6cd71-246">Nel caso dell'esempio "MyNamedClient", i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-246">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="6cd71-247">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="6cd71-247">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="6cd71-248">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="6cd71-248">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="6cd71-249">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="6cd71-249">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="6cd71-250">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="6cd71-250">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="6cd71-251">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="6cd71-251">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="6cd71-252">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="6cd71-252">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="6cd71-253">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="6cd71-253">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="6cd71-254">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6cd71-254">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="6cd71-255">È possibile usare il metodo di estensione `ConfigurePrimaryHttpMessageHandler` per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="6cd71-255">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="6cd71-256">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="6cd71-256">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
