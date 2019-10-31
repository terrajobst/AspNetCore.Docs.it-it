---
title: Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: a963833acfa12889c8ae3dac443962682e1cb931
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190589"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="063b1-103">Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="063b1-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="063b1-104">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)e [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="063b1-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="063b1-105">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="063b1-106">`IHttpClientFactory` offre i seguenti vantaggi:</span><span class="sxs-lookup"><span data-stu-id="063b1-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="063b1-107">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="063b1-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="063b1-108">Ad esempio, un client denominato *GitHub* potrebbe essere registrato e configurato per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="063b1-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="063b1-109">Un client predefinito può essere registrato per l'accesso generale.</span><span class="sxs-lookup"><span data-stu-id="063b1-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="063b1-110">Codifica il concetto di middleware in uscita tramite la delega dei gestori in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="063b1-111">Fornisce le estensioni per il middleware basato su Polly per sfruttare i vantaggi delegare i gestori in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="063b1-112">Gestisce il pool e la durata delle istanze di `HttpClientMessageHandler` sottostanti.</span><span class="sxs-lookup"><span data-stu-id="063b1-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="063b1-113">La gestione automatica evita i problemi comuni di DNS (Domain Name System) che si verificano quando si gestiscono manualmente `HttpClient` durate.</span><span class="sxs-lookup"><span data-stu-id="063b1-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="063b1-114">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="063b1-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="063b1-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="063b1-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="063b1-116">Il codice di esempio in questa versione dell'argomento USA <xref:System.Text.Json> per deserializzare il contenuto JSON restituito nelle risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="063b1-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="063b1-117">Per esempi che usano `Json.NET` e `ReadAsAsync<T>`, usare il selettore di versione per selezionare una versione 2. x di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="063b1-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="063b1-118">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="063b1-118">Consumption patterns</span></span>

<span data-ttu-id="063b1-119">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="063b1-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="063b1-120">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="063b1-121">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="063b1-122">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="063b1-123">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="063b1-124">L'approccio migliore dipende dai requisiti dell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="063b1-125">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-125">Basic usage</span></span>

<span data-ttu-id="063b1-126">`IHttpClientFactory` può essere registrato chiamando `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="063b1-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="063b1-127">È possibile richiedere un `IHttpClientFactory` usando l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="063b1-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="063b1-128">Il codice seguente usa `IHttpClientFactory` per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="063b1-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="063b1-129">L'uso di `IHttpClientFactory` come nell'esempio precedente è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="063b1-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="063b1-130">Non ha alcun effetto sul modo in cui viene usato `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="063b1-131">Nelle posizioni in cui vengono create `HttpClient` istanze in un'app esistente, sostituire tali occorrenze con le chiamate a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="063b1-132">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-132">Named clients</span></span>

<span data-ttu-id="063b1-133">I client denominati sono una scelta ottimale nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="063b1-134">L'app richiede molti usi distinti di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="063b1-135">Molti `HttpClient`s hanno una configurazione diversa.</span><span class="sxs-lookup"><span data-stu-id="063b1-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="063b1-136">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="063b1-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="063b1-137">Nel codice precedente il client viene configurato con:</span><span class="sxs-lookup"><span data-stu-id="063b1-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="063b1-138">Indirizzo di base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="063b1-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="063b1-139">Due intestazioni necessarie per lavorare con l'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="063b1-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="063b1-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="063b1-140">CreateClient</span></span>

<span data-ttu-id="063b1-141">Ogni volta che viene chiamato <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="063b1-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="063b1-142">Viene creata una nuova istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="063b1-143">Viene chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-143">The configuration action is called.</span></span>

<span data-ttu-id="063b1-144">Per creare un client denominato, passare il nome in `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="063b1-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="063b1-145">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="063b1-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="063b1-146">Il codice può passare solo il percorso, perché viene usato l'indirizzo di base configurato per il client.</span><span class="sxs-lookup"><span data-stu-id="063b1-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="063b1-147">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-147">Typed clients</span></span>

<span data-ttu-id="063b1-148">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="063b1-148">Typed clients:</span></span>

* <span data-ttu-id="063b1-149">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="063b1-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="063b1-150">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="063b1-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="063b1-151">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="063b1-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="063b1-152">Ad esempio, è possibile usare un singolo client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="063b1-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="063b1-153">Per un singolo endpoint back-end.</span><span class="sxs-lookup"><span data-stu-id="063b1-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="063b1-154">Per incapsulare tutta la logica che riguarda l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="063b1-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="063b1-155">Usare DI e può essere inserito laddove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="063b1-156">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="063b1-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="063b1-157">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="063b1-157">In the preceding code:</span></span>

* <span data-ttu-id="063b1-158">La configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="063b1-159">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="063b1-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="063b1-160">È possibile creare metodi specifici dell'API che espongono `HttpClient` funzionalità.</span><span class="sxs-lookup"><span data-stu-id="063b1-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="063b1-161">Ad esempio, il metodo `GetAspNetDocsIssues` incapsula il codice per recuperare i problemi aperti.</span><span class="sxs-lookup"><span data-stu-id="063b1-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="063b1-162">Il codice seguente chiama <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` per registrare una classe client tipizzata:</span><span class="sxs-lookup"><span data-stu-id="063b1-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="063b1-163">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="063b1-164">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="063b1-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="063b1-165">La configurazione di un client tipizzato può essere specificata durante la registrazione in `Startup.ConfigureServices`, anziché nel costruttore del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="063b1-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="063b1-166">Il `HttpClient` può essere incapsulato all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="063b1-167">Anziché esporla come proprietà, definire un metodo che chiama l'istanza di `HttpClient` internamente:</span><span class="sxs-lookup"><span data-stu-id="063b1-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="063b1-168">Nel codice precedente, il `HttpClient` viene archiviato in un campo privato.</span><span class="sxs-lookup"><span data-stu-id="063b1-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="063b1-169">L'accesso alla `HttpClient` è dal metodo public `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="063b1-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="063b1-170">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-170">Generated clients</span></span>

<span data-ttu-id="063b1-171">`IHttpClientFactory` può essere usato in combinazione con librerie di terze parti, ad esempio il [refitting](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="063b1-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="063b1-172">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="063b1-173">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="063b1-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="063b1-174">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="063b1-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="063b1-175">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="063b1-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="063b1-176">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="063b1-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="063b1-177">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="063b1-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="063b1-178">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="063b1-178">Outgoing request middleware</span></span>

<span data-ttu-id="063b1-179">`HttpClient` ha il concetto di delegare i gestori che possono essere collegati insieme per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="063b1-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="063b1-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="063b1-181">Semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="063b1-182">Supporta la registrazione e il concatenamento di più gestori per compilare una pipeline middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="063b1-183">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="063b1-184">Questo modello:</span><span class="sxs-lookup"><span data-stu-id="063b1-184">This pattern:</span></span>

  * <span data-ttu-id="063b1-185">È simile alla pipeline del middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="063b1-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="063b1-186">Fornisce un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="063b1-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="063b1-187">memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="063b1-187">caching</span></span>
    * <span data-ttu-id="063b1-188">gestione errori</span><span class="sxs-lookup"><span data-stu-id="063b1-188">error handling</span></span>
    * <span data-ttu-id="063b1-189">serializzazione</span><span class="sxs-lookup"><span data-stu-id="063b1-189">serialization</span></span>
    * <span data-ttu-id="063b1-190">registrazione</span><span class="sxs-lookup"><span data-stu-id="063b1-190">logging</span></span>

<span data-ttu-id="063b1-191">Per creare un gestore di delega:</span><span class="sxs-lookup"><span data-stu-id="063b1-191">To create a delegating handler:</span></span>

* <span data-ttu-id="063b1-192">Derivare da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="063b1-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="063b1-193">Eseguire l'override di <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="063b1-194">Eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="063b1-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="063b1-195">Il codice precedente controlla se l'intestazione `X-API-KEY` è presente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="063b1-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="063b1-196">Se `X-API-KEY` è mancante, viene restituito <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="063b1-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="063b1-197">È possibile aggiungere più di un gestore alla configurazione per un `HttpClient` con <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="063b1-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="063b1-198">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="063b1-199">L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="063b1-200">I gestori possono dipendere da servizi di qualsiasi ambito.</span><span class="sxs-lookup"><span data-stu-id="063b1-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="063b1-201">I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="063b1-202">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="063b1-203">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="063b1-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="063b1-204">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="063b1-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="063b1-205">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="063b1-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="063b1-206">Passare i dati nel gestore usando [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="063b1-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="063b1-207">Usare <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="063b1-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="063b1-208">Creare un oggetto di archiviazione <xref:System.Threading.AsyncLocal`1> personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="063b1-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="063b1-209">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-209">Use Polly-based handlers</span></span>

<span data-ttu-id="063b1-210">`IHttpClientFactory` si integra con la libreria di terze parti [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="063b1-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="063b1-211">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="063b1-212">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="063b1-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="063b1-213">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="063b1-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="063b1-214">Le estensioni di Polly supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="063b1-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="063b1-215">Polly richiede il pacchetto NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="063b1-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="063b1-216">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="063b1-216">Handle transient faults</span></span>

<span data-ttu-id="063b1-217">Gli errori si verificano in genere quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="063b1-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="063b1-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> consente di definire un criterio per gestire gli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="063b1-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="063b1-219">I criteri configurati con `AddTransientHttpErrorPolicy` gestiscono le risposte seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="063b1-220">5xx HTTP</span><span class="sxs-lookup"><span data-stu-id="063b1-220">HTTP 5xx</span></span>
* <span data-ttu-id="063b1-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="063b1-221">HTTP 408</span></span>

<span data-ttu-id="063b1-222">`AddTransientHttpErrorPolicy` fornisce l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="063b1-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="063b1-223">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="063b1-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="063b1-224">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="063b1-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="063b1-225">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="063b1-225">Dynamically select policies</span></span>

<span data-ttu-id="063b1-226">I metodi di estensione vengono forniti per aggiungere gestori basati su Polly, ad esempio <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="063b1-227">L'overload di `AddPolicyHandler` seguente controlla la richiesta per decidere quali criteri applicare:</span><span class="sxs-lookup"><span data-stu-id="063b1-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="063b1-228">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="063b1-229">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="063b1-230">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="063b1-231">È comune annidare i criteri Polly:</span><span class="sxs-lookup"><span data-stu-id="063b1-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="063b1-232">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="063b1-232">In the preceding example:</span></span>

* <span data-ttu-id="063b1-233">Vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="063b1-233">Two handlers are added.</span></span>
* <span data-ttu-id="063b1-234">Il primo gestore USA <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> per aggiungere un criterio di ripetizione dei tentativi.</span><span class="sxs-lookup"><span data-stu-id="063b1-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="063b1-235">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="063b1-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="063b1-236">La seconda chiamata di `AddTransientHttpErrorPolicy` aggiunge un criterio di interruttore.</span><span class="sxs-lookup"><span data-stu-id="063b1-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="063b1-237">Altre richieste esterne vengono bloccate per 30 secondi se 5 tentativi non riusciti si verificano in sequenza.</span><span class="sxs-lookup"><span data-stu-id="063b1-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="063b1-238">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="063b1-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="063b1-239">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="063b1-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="063b1-240">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="063b1-241">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="063b1-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="063b1-242">Nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="063b1-242">In the following code:</span></span>

* <span data-ttu-id="063b1-243">Vengono aggiunti i criteri "regular" e "Long".</span><span class="sxs-lookup"><span data-stu-id="063b1-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="063b1-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> aggiunge i criteri "regular" e "Long" dal registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="063b1-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="063b1-245">Per ulteriori informazioni sulle integrazioni `IHttpClientFactory` e Polly, vedere il [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="063b1-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="063b1-246">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="063b1-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="063b1-247">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-248">Viene creato un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="063b1-249">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="063b1-250">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="063b1-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="063b1-251">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="063b1-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="063b1-252">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="063b1-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="063b1-253">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="063b1-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="063b1-254">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito, che possono impedire al gestore di reagire alle modifiche DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="063b1-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="063b1-255">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="063b1-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="063b1-256">È possibile eseguire l'override del valore predefinito in base al client denominato:</span><span class="sxs-lookup"><span data-stu-id="063b1-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="063b1-257">le istanze di `HttpClient` possono in genere essere considerate come oggetti .NET che **non** richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="063b1-258">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="063b1-259">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="063b1-260">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-261">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="063b1-262">Registrazione</span><span class="sxs-lookup"><span data-stu-id="063b1-262">Logging</span></span>

<span data-ttu-id="063b1-263">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-263">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="063b1-264">Abilitare il livello di informazioni appropriato nella configurazione di registrazione per visualizzare i messaggi di log predefiniti.</span><span class="sxs-lookup"><span data-stu-id="063b1-264">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="063b1-265">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="063b1-265">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="063b1-266">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="063b1-266">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="063b1-267">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="063b1-267">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="063b1-268">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-268">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="063b1-269">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="063b1-269">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="063b1-270">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-270">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="063b1-271">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-271">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="063b1-272">Nell'esempio *MyNamedClient* i messaggi vengono registrati con la categoria di log "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="063b1-272">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="063b1-273">Per la richiesta, questo errore si verifica dopo l'esecuzione di tutti gli altri gestori e immediatamente prima dell'invio della richiesta.</span><span class="sxs-lookup"><span data-stu-id="063b1-273">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="063b1-274">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-274">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="063b1-275">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="063b1-275">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="063b1-276">Questo può includere modifiche alle intestazioni delle richieste o al codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-276">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="063b1-277">L'inclusione del nome del client nella categoria log consente il filtraggio dei log per specifici client denominati.</span><span class="sxs-lookup"><span data-stu-id="063b1-277">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="063b1-278">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="063b1-278">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="063b1-279">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="063b1-279">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="063b1-280">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="063b1-280">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="063b1-281">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="063b1-281">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="063b1-282">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="063b1-282">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="063b1-283">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="063b1-283">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="063b1-284">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-284">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="063b1-285">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="063b1-285">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="063b1-286">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="063b1-286">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="063b1-287">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="063b1-287">In the following example:</span></span>

* <span data-ttu-id="063b1-288"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="063b1-288"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="063b1-289">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-289">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="063b1-290">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="063b1-290">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="063b1-291">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="063b1-291">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="063b1-292">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="063b1-292">Additional resources</span></span>

* [<span data-ttu-id="063b1-293">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="063b1-293">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="063b1-294">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="063b1-294">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="063b1-295">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="063b1-295">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="063b1-296">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="063b1-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="063b1-297">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-297">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="063b1-298">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-298">It offers the following benefits:</span></span>

* <span data-ttu-id="063b1-299">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="063b1-299">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="063b1-300">Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="063b1-300">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="063b1-301">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="063b1-301">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="063b1-302">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="063b1-302">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="063b1-303">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="063b1-303">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="063b1-304">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="063b1-304">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="063b1-305">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="063b1-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="063b1-306">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="063b1-306">Consumption patterns</span></span>

<span data-ttu-id="063b1-307">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="063b1-307">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="063b1-308">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-308">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="063b1-309">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-309">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="063b1-310">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-310">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="063b1-311">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-311">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="063b1-312">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="063b1-312">None of them are strictly superior to another.</span></span> <span data-ttu-id="063b1-313">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-313">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="063b1-314">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-314">Basic usage</span></span>

<span data-ttu-id="063b1-315">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-315">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="063b1-316">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="063b1-316">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="063b1-317">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="063b1-317">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="063b1-318">Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="063b1-318">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="063b1-319">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-319">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="063b1-320">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-320">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="063b1-321">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-321">Named clients</span></span>

<span data-ttu-id="063b1-322">Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="063b1-322">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="063b1-323">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-323">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="063b1-324">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*.</span><span class="sxs-lookup"><span data-stu-id="063b1-324">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="063b1-325">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="063b1-325">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="063b1-326">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-326">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="063b1-327">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-327">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="063b1-328">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="063b1-328">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="063b1-329">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="063b1-329">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="063b1-330">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="063b1-330">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="063b1-331">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-331">Typed clients</span></span>

<span data-ttu-id="063b1-332">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="063b1-332">Typed clients:</span></span>

* <span data-ttu-id="063b1-333">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="063b1-333">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="063b1-334">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="063b1-334">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="063b1-335">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="063b1-335">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="063b1-336">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="063b1-336">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="063b1-337">Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-337">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="063b1-338">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="063b1-338">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="063b1-339">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-339">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="063b1-340">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="063b1-340">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="063b1-341">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-341">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="063b1-342">Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="063b1-342">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="063b1-343">Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="063b1-343">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="063b1-344">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-344">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="063b1-345">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="063b1-345">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="063b1-346">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="063b1-346">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="063b1-347">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-347">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="063b1-348">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="063b1-348">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="063b1-349">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="063b1-349">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="063b1-350">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="063b1-350">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="063b1-351">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-351">Generated clients</span></span>

<span data-ttu-id="063b1-352">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="063b1-352">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="063b1-353">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-353">Refit is a REST library for .NET.</span></span> <span data-ttu-id="063b1-354">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="063b1-354">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="063b1-355">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="063b1-355">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="063b1-356">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="063b1-356">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="063b1-357">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="063b1-357">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="063b1-358">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="063b1-358">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="063b1-359">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="063b1-359">Outgoing request middleware</span></span>

<span data-ttu-id="063b1-360">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-360">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="063b1-361">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-361">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="063b1-362">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-362">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="063b1-363">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-363">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="063b1-364">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="063b1-364">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="063b1-365">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-365">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="063b1-366">Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="063b1-366">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="063b1-367">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="063b1-367">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="063b1-368">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="063b1-368">The preceding code defines a basic handler.</span></span> <span data-ttu-id="063b1-369">Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="063b1-369">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="063b1-370">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="063b1-370">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="063b1-371">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-371">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="063b1-372">Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="063b1-372">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="063b1-373">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-373">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="063b1-374">L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-374">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="063b1-375">I gestori possono dipendere da servizi di qualsiasi ambito.</span><span class="sxs-lookup"><span data-stu-id="063b1-375">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="063b1-376">I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-376">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="063b1-377">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-377">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="063b1-378">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="063b1-378">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="063b1-379">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="063b1-379">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="063b1-380">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="063b1-380">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="063b1-381">Passare i dati al gestore usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="063b1-381">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="063b1-382">Usare `IHttpContextAccessor` per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="063b1-382">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="063b1-383">Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="063b1-383">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="063b1-384">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-384">Use Polly-based handlers</span></span>

<span data-ttu-id="063b1-385">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="063b1-385">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="063b1-386">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-386">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="063b1-387">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="063b1-387">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="063b1-388">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="063b1-388">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="063b1-389">Le estensioni Polly:</span><span class="sxs-lookup"><span data-stu-id="063b1-389">The Polly extensions:</span></span>

* <span data-ttu-id="063b1-390">Supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="063b1-390">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="063b1-391">Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="063b1-391">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="063b1-392">Il pacchetto non è incluso nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="063b1-392">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="063b1-393">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="063b1-393">Handle transient faults</span></span>

<span data-ttu-id="063b1-394">La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="063b1-394">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="063b1-395">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="063b1-395">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="063b1-396">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="063b1-396">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="063b1-397">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-397">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="063b1-398">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="063b1-398">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="063b1-399">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="063b1-399">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="063b1-400">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="063b1-400">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="063b1-401">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="063b1-401">Dynamically select policies</span></span>

<span data-ttu-id="063b1-402">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="063b1-402">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="063b1-403">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="063b1-403">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="063b1-404">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="063b1-404">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="063b1-405">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-405">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="063b1-406">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-406">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="063b1-407">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-407">Add multiple Polly handlers</span></span>

<span data-ttu-id="063b1-408">In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:</span><span class="sxs-lookup"><span data-stu-id="063b1-408">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="063b1-409">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="063b1-409">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="063b1-410">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="063b1-410">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="063b1-411">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="063b1-411">Failed requests are retried up to three times.</span></span> <span data-ttu-id="063b1-412">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="063b1-412">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="063b1-413">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="063b1-413">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="063b1-414">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="063b1-414">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="063b1-415">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="063b1-415">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="063b1-416">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-416">Add policies from the Polly registry</span></span>

<span data-ttu-id="063b1-417">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="063b1-417">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="063b1-418">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="063b1-418">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="063b1-419">Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="063b1-419">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="063b1-420">Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="063b1-420">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="063b1-421">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="063b1-421">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="063b1-422">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="063b1-422">HttpClient and lifetime management</span></span>

<span data-ttu-id="063b1-423">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-423">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-424">È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-424">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="063b1-425">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-425">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="063b1-426">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="063b1-426">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="063b1-427">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="063b1-427">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="063b1-428">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="063b1-428">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="063b1-429">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="063b1-429">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="063b1-430">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="063b1-430">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="063b1-431">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="063b1-431">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="063b1-432">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-432">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="063b1-433">Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="063b1-433">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="063b1-434">L'eliminazione del client non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="063b1-434">Disposal of the client isn't required.</span></span> <span data-ttu-id="063b1-435">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-435">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="063b1-436">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-436">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="063b1-437">Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-437">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="063b1-438">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-438">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-439">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-439">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="063b1-440">Registrazione</span><span class="sxs-lookup"><span data-stu-id="063b1-440">Logging</span></span>

<span data-ttu-id="063b1-441">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-441">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="063b1-442">Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-442">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="063b1-443">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="063b1-443">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="063b1-444">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="063b1-444">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="063b1-445">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-445">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="063b1-446">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-446">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="063b1-447">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="063b1-447">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="063b1-448">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-448">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="063b1-449">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-449">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="063b1-450">Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-450">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="063b1-451">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="063b1-451">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="063b1-452">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-452">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="063b1-453">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="063b1-453">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="063b1-454">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-454">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="063b1-455">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="063b1-455">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="063b1-456">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="063b1-456">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="063b1-457">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="063b1-457">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="063b1-458">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="063b1-458">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="063b1-459">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="063b1-459">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="063b1-460">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="063b1-460">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="063b1-461">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="063b1-461">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="063b1-462">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-462">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="063b1-463">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="063b1-463">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="063b1-464">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="063b1-464">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="063b1-465">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="063b1-465">In the following example:</span></span>

* <span data-ttu-id="063b1-466"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="063b1-466"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="063b1-467">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-467">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="063b1-468">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="063b1-468">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="063b1-469">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="063b1-469">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="063b1-470">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="063b1-470">Additional resources</span></span>

* [<span data-ttu-id="063b1-471">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="063b1-471">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="063b1-472">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="063b1-472">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="063b1-473">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="063b1-473">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="063b1-474">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="063b1-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="063b1-475">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-475">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="063b1-476">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-476">It offers the following benefits:</span></span>

* <span data-ttu-id="063b1-477">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="063b1-477">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="063b1-478">Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="063b1-478">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="063b1-479">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="063b1-479">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="063b1-480">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="063b1-480">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="063b1-481">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="063b1-481">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="063b1-482">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="063b1-482">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="063b1-483">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="063b1-483">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="063b1-484">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="063b1-484">Prerequisites</span></span>

<span data-ttu-id="063b1-485">I progetti destinati a .NET Framework richiedono l'installazione del pacchetto NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="063b1-485">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="063b1-486">I progetti destinati a .NET Core che fanno riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage-app) sono già inclusi nel pacchetto `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="063b1-486">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="063b1-487">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="063b1-487">Consumption patterns</span></span>

<span data-ttu-id="063b1-488">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="063b1-488">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="063b1-489">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-489">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="063b1-490">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-490">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="063b1-491">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-491">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="063b1-492">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-492">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="063b1-493">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="063b1-493">None of them are strictly superior to another.</span></span> <span data-ttu-id="063b1-494">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-494">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="063b1-495">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="063b1-495">Basic usage</span></span>

<span data-ttu-id="063b1-496">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-496">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="063b1-497">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="063b1-497">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="063b1-498">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="063b1-498">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="063b1-499">Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="063b1-499">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="063b1-500">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-500">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="063b1-501">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-501">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="063b1-502">Client denominati</span><span class="sxs-lookup"><span data-stu-id="063b1-502">Named clients</span></span>

<span data-ttu-id="063b1-503">Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="063b1-503">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="063b1-504">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-504">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="063b1-505">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*.</span><span class="sxs-lookup"><span data-stu-id="063b1-505">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="063b1-506">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="063b1-506">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="063b1-507">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-507">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="063b1-508">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-508">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="063b1-509">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="063b1-509">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="063b1-510">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="063b1-510">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="063b1-511">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="063b1-511">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="063b1-512">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="063b1-512">Typed clients</span></span>

<span data-ttu-id="063b1-513">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="063b1-513">Typed clients:</span></span>

* <span data-ttu-id="063b1-514">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="063b1-514">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="063b1-515">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="063b1-515">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="063b1-516">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="063b1-516">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="063b1-517">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="063b1-517">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="063b1-518">Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="063b1-518">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="063b1-519">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="063b1-519">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="063b1-520">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-520">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="063b1-521">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="063b1-521">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="063b1-522">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-522">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="063b1-523">Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="063b1-523">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="063b1-524">Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="063b1-524">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="063b1-525">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-525">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="063b1-526">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="063b1-526">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="063b1-527">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="063b1-527">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="063b1-528">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="063b1-528">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="063b1-529">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="063b1-529">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="063b1-530">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="063b1-530">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="063b1-531">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="063b1-531">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="063b1-532">Client generati</span><span class="sxs-lookup"><span data-stu-id="063b1-532">Generated clients</span></span>

<span data-ttu-id="063b1-533">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="063b1-533">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="063b1-534">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-534">Refit is a REST library for .NET.</span></span> <span data-ttu-id="063b1-535">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="063b1-535">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="063b1-536">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="063b1-536">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="063b1-537">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="063b1-537">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="063b1-538">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="063b1-538">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="063b1-539">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="063b1-539">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="063b1-540">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="063b1-540">Outgoing request middleware</span></span>

<span data-ttu-id="063b1-541">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-541">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="063b1-542">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-542">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="063b1-543">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-543">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="063b1-544">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="063b1-544">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="063b1-545">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="063b1-545">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="063b1-546">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-546">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="063b1-547">Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="063b1-547">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="063b1-548">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="063b1-548">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="063b1-549">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="063b1-549">The preceding code defines a basic handler.</span></span> <span data-ttu-id="063b1-550">Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="063b1-550">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="063b1-551">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="063b1-551">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="063b1-552">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-552">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="063b1-553">Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="063b1-553">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="063b1-554">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="063b1-554">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="063b1-555">Il gestore **deve** essere registrato nell'inserimento di dipendenze come servizio temporaneo, senza definizione di ambito.</span><span class="sxs-lookup"><span data-stu-id="063b1-555">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="063b1-556">Se il gestore viene registrato come un servizio con ambito e i servizi da cui dipende il gestore sono eliminabili:</span><span class="sxs-lookup"><span data-stu-id="063b1-556">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="063b1-557">I servizi del gestore potrebbero essere eliminati prima che il gestore esca dall'ambito.</span><span class="sxs-lookup"><span data-stu-id="063b1-557">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="063b1-558">I servizi del gestore eliminati causano un errore del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-558">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="063b1-559">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-559">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="063b1-560">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="063b1-560">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="063b1-561">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="063b1-561">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="063b1-562">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="063b1-562">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="063b1-563">Passare i dati al gestore usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="063b1-563">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="063b1-564">Usare `IHttpContextAccessor` per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="063b1-564">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="063b1-565">Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="063b1-565">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="063b1-566">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-566">Use Polly-based handlers</span></span>

<span data-ttu-id="063b1-567">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="063b1-567">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="063b1-568">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="063b1-568">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="063b1-569">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="063b1-569">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="063b1-570">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="063b1-570">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="063b1-571">Le estensioni Polly:</span><span class="sxs-lookup"><span data-stu-id="063b1-571">The Polly extensions:</span></span>

* <span data-ttu-id="063b1-572">Supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="063b1-572">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="063b1-573">Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="063b1-573">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="063b1-574">Il pacchetto non è incluso nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="063b1-574">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="063b1-575">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="063b1-575">Handle transient faults</span></span>

<span data-ttu-id="063b1-576">La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="063b1-576">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="063b1-577">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="063b1-577">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="063b1-578">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="063b1-578">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="063b1-579">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="063b1-579">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="063b1-580">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="063b1-580">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="063b1-581">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="063b1-581">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="063b1-582">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="063b1-582">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="063b1-583">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="063b1-583">Dynamically select policies</span></span>

<span data-ttu-id="063b1-584">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="063b1-584">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="063b1-585">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="063b1-585">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="063b1-586">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="063b1-586">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="063b1-587">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-587">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="063b1-588">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="063b1-588">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="063b1-589">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-589">Add multiple Polly handlers</span></span>

<span data-ttu-id="063b1-590">In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:</span><span class="sxs-lookup"><span data-stu-id="063b1-590">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="063b1-591">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="063b1-591">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="063b1-592">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="063b1-592">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="063b1-593">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="063b1-593">Failed requests are retried up to three times.</span></span> <span data-ttu-id="063b1-594">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="063b1-594">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="063b1-595">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="063b1-595">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="063b1-596">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="063b1-596">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="063b1-597">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="063b1-597">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="063b1-598">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="063b1-598">Add policies from the Polly registry</span></span>

<span data-ttu-id="063b1-599">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="063b1-599">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="063b1-600">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="063b1-600">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="063b1-601">Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="063b1-601">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="063b1-602">Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="063b1-602">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="063b1-603">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="063b1-603">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="063b1-604">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="063b1-604">HttpClient and lifetime management</span></span>

<span data-ttu-id="063b1-605">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-605">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-606">È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-606">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="063b1-607">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-607">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="063b1-608">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="063b1-608">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="063b1-609">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="063b1-609">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="063b1-610">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="063b1-610">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="063b1-611">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="063b1-611">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="063b1-612">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="063b1-612">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="063b1-613">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="063b1-613">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="063b1-614">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="063b1-614">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="063b1-615">Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="063b1-615">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="063b1-616">L'eliminazione del client non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="063b1-616">Disposal of the client isn't required.</span></span> <span data-ttu-id="063b1-617">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="063b1-617">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="063b1-618">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-618">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="063b1-619">Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-619">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="063b1-620">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-620">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="063b1-621">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="063b1-621">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="063b1-622">Registrazione</span><span class="sxs-lookup"><span data-stu-id="063b1-622">Logging</span></span>

<span data-ttu-id="063b1-623">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-623">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="063b1-624">Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="063b1-624">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="063b1-625">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="063b1-625">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="063b1-626">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="063b1-626">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="063b1-627">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-627">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="063b1-628">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-628">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="063b1-629">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="063b1-629">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="063b1-630">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-630">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="063b1-631">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="063b1-631">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="063b1-632">Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="063b1-632">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="063b1-633">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="063b1-633">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="063b1-634">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="063b1-634">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="063b1-635">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="063b1-635">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="063b1-636">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="063b1-636">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="063b1-637">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="063b1-637">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="063b1-638">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="063b1-638">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="063b1-639">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="063b1-639">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="063b1-640">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="063b1-640">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="063b1-641">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="063b1-641">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="063b1-642">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="063b1-642">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="063b1-643">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="063b1-643">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="063b1-644">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="063b1-644">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="063b1-645">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="063b1-645">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="063b1-646">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="063b1-646">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="063b1-647">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="063b1-647">In the following example:</span></span>

* <span data-ttu-id="063b1-648"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="063b1-648"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="063b1-649">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="063b1-649">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="063b1-650">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="063b1-650">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="063b1-651">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="063b1-651">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="063b1-652">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="063b1-652">Additional resources</span></span>

* [<span data-ttu-id="063b1-653">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="063b1-653">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="063b1-654">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="063b1-654">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="063b1-655">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="063b1-655">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
