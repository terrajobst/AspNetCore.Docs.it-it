---
title: Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 746604bc92775a6fac124ee8bfcf37635786fe41
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717009"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="5cad9-103">Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cad9-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5cad9-104">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)e [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="5cad9-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="5cad9-105">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5cad9-106">`IHttpClientFactory` offre i seguenti vantaggi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="5cad9-107">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="5cad9-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-108">Ad esempio, un client denominato *GitHub* potrebbe essere registrato e configurato per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5cad9-109">Un client predefinito può essere registrato per l'accesso generale.</span><span class="sxs-lookup"><span data-stu-id="5cad9-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="5cad9-110">Codifica il concetto di middleware in uscita tramite la delega dei gestori in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="5cad9-111">Fornisce le estensioni per il middleware basato su Polly per sfruttare i vantaggi delegare i gestori in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="5cad9-112">Gestisce il pool e la durata delle istanze di `HttpClientMessageHandler` sottostanti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="5cad9-113">La gestione automatica evita i problemi comuni di DNS (Domain Name System) che si verificano quando si gestiscono manualmente `HttpClient` durate.</span><span class="sxs-lookup"><span data-stu-id="5cad9-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5cad9-114">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="5cad9-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5cad9-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5cad9-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="5cad9-116">Il codice di esempio in questa versione dell'argomento USA <xref:System.Text.Json> per deserializzare il contenuto JSON restituito nelle risposte HTTP.</span><span class="sxs-lookup"><span data-stu-id="5cad9-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="5cad9-117">Per esempi che usano `Json.NET` e `ReadAsAsync<T>`, usare il selettore di versione per selezionare una versione 2. x di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="5cad9-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5cad9-118">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="5cad9-118">Consumption patterns</span></span>

<span data-ttu-id="5cad9-119">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5cad9-120">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5cad9-121">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5cad9-122">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5cad9-123">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5cad9-124">L'approccio migliore dipende dai requisiti dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5cad9-125">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-125">Basic usage</span></span>

<span data-ttu-id="5cad9-126">`IHttpClientFactory` può essere registrato chiamando `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5cad9-127">È possibile richiedere un `IHttpClientFactory` usando l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5cad9-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5cad9-128">Il codice seguente usa `IHttpClientFactory` per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5cad9-129">L'uso di `IHttpClientFactory` come nell'esempio precedente è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="5cad9-130">Non ha alcun effetto sul modo in cui viene usato `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="5cad9-131">Nelle posizioni in cui vengono create `HttpClient` istanze in un'app esistente, sostituire tali occorrenze con le chiamate a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5cad9-132">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-132">Named clients</span></span>

<span data-ttu-id="5cad9-133">I client denominati sono una scelta ottimale nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="5cad9-134">L'app richiede molti usi distinti di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="5cad9-135">Molti `HttpClient`s hanno una configurazione diversa.</span><span class="sxs-lookup"><span data-stu-id="5cad9-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="5cad9-136">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5cad9-137">Nel codice precedente il client viene configurato con:</span><span class="sxs-lookup"><span data-stu-id="5cad9-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="5cad9-138">Indirizzo di base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="5cad9-139">Due intestazioni necessarie per lavorare con l'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5cad9-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="5cad9-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="5cad9-140">CreateClient</span></span>

<span data-ttu-id="5cad9-141">Ogni volta che viene chiamato <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="5cad9-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="5cad9-142">Viene creata una nuova istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="5cad9-143">Viene chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-143">The configuration action is called.</span></span>

<span data-ttu-id="5cad9-144">Per creare un client denominato, passare il nome in `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5cad9-145">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="5cad9-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5cad9-146">Il codice può passare solo il percorso, perché viene usato l'indirizzo di base configurato per il client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5cad9-147">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-147">Typed clients</span></span>

<span data-ttu-id="5cad9-148">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="5cad9-148">Typed clients:</span></span>

* <span data-ttu-id="5cad9-149">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5cad9-150">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5cad9-151">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5cad9-152">Ad esempio, è possibile usare un singolo client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="5cad9-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="5cad9-153">Per un singolo endpoint back-end.</span><span class="sxs-lookup"><span data-stu-id="5cad9-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="5cad9-154">Per incapsulare tutta la logica che riguarda l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="5cad9-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="5cad9-155">Usare DI e può essere inserito laddove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="5cad9-156">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="5cad9-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5cad9-157">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-157">In the preceding code:</span></span>

* <span data-ttu-id="5cad9-158">La configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="5cad9-159">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="5cad9-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="5cad9-160">È possibile creare metodi specifici dell'API che espongono `HttpClient` funzionalità.</span><span class="sxs-lookup"><span data-stu-id="5cad9-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5cad9-161">Ad esempio, il metodo `GetAspNetDocsIssues` incapsula il codice per recuperare i problemi aperti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="5cad9-162">Il codice seguente chiama <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` per registrare una classe client tipizzata:</span><span class="sxs-lookup"><span data-stu-id="5cad9-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5cad9-163">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5cad9-164">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5cad9-165">La configurazione di un client tipizzato può essere specificata durante la registrazione in `Startup.ConfigureServices`, anziché nel costruttore del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="5cad9-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5cad9-166">Il `HttpClient` può essere incapsulato all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="5cad9-167">Anziché esporla come proprietà, definire un metodo che chiama l'istanza di `HttpClient` internamente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5cad9-168">Nel codice precedente, il `HttpClient` viene archiviato in un campo privato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="5cad9-169">L'accesso alla `HttpClient` è dal metodo public `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5cad9-170">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-170">Generated clients</span></span>

<span data-ttu-id="5cad9-171">`IHttpClientFactory` può essere usato in combinazione con librerie di terze parti, ad esempio il [refitting](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="5cad9-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5cad9-172">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5cad9-173">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="5cad9-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5cad9-174">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="5cad9-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5cad9-175">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5cad9-176">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="5cad9-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5cad9-177">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="5cad9-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5cad9-178">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="5cad9-178">Outgoing request middleware</span></span>

<span data-ttu-id="5cad9-179">`HttpClient` ha il concetto di delegare i gestori che possono essere collegati insieme per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5cad9-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="5cad9-181">Semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="5cad9-182">Supporta la registrazione e il concatenamento di più gestori per compilare una pipeline middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5cad9-183">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5cad9-184">Questo modello:</span><span class="sxs-lookup"><span data-stu-id="5cad9-184">This pattern:</span></span>

  * <span data-ttu-id="5cad9-185">È simile alla pipeline del middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cad9-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="5cad9-186">Fornisce un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5cad9-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="5cad9-187">memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="5cad9-187">caching</span></span>
    * <span data-ttu-id="5cad9-188">gestione errori</span><span class="sxs-lookup"><span data-stu-id="5cad9-188">error handling</span></span>
    * <span data-ttu-id="5cad9-189">serialization</span><span class="sxs-lookup"><span data-stu-id="5cad9-189">serialization</span></span>
    * <span data-ttu-id="5cad9-190">registrazione</span><span class="sxs-lookup"><span data-stu-id="5cad9-190">logging</span></span>

<span data-ttu-id="5cad9-191">Per creare un gestore di delega:</span><span class="sxs-lookup"><span data-stu-id="5cad9-191">To create a delegating handler:</span></span>

* <span data-ttu-id="5cad9-192">Derivare da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="5cad9-193">Eseguire l'override di <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="5cad9-194">Eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="5cad9-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5cad9-195">Il codice precedente controlla se l'intestazione `X-API-KEY` è presente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="5cad9-196">Se `X-API-KEY` è mancante, viene restituito <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="5cad9-197">È possibile aggiungere più di un gestore alla configurazione per un `HttpClient` con <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="5cad9-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="5cad9-198">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5cad9-199">L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="5cad9-200">I gestori possono dipendere da servizi di qualsiasi ambito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="5cad9-201">I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="5cad9-202">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="5cad9-203">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5cad9-204">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5cad9-205">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5cad9-206">Passare i dati nel gestore usando [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="5cad9-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="5cad9-207">Usare <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="5cad9-208">Creare un oggetto di archiviazione <xref:System.Threading.AsyncLocal`1> personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5cad9-209">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-209">Use Polly-based handlers</span></span>

<span data-ttu-id="5cad9-210">`IHttpClientFactory` si integra con la libreria di terze parti [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5cad9-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5cad9-211">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5cad9-212">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="5cad9-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5cad9-213">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-214">Le estensioni di Polly supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="5cad9-215">Polly richiede il pacchetto NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="5cad9-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5cad9-216">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="5cad9-216">Handle transient faults</span></span>

<span data-ttu-id="5cad9-217">Gli errori si verificano in genere quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="5cad9-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5cad9-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> consente di definire un criterio per gestire gli errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="5cad9-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5cad9-219">I criteri configurati con `AddTransientHttpErrorPolicy` gestiscono le risposte seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="5cad9-220">5xx HTTP</span><span class="sxs-lookup"><span data-stu-id="5cad9-220">HTTP 5xx</span></span>
* <span data-ttu-id="5cad9-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="5cad9-221">HTTP 408</span></span>

<span data-ttu-id="5cad9-222">`AddTransientHttpErrorPolicy` fornisce l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="5cad9-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="5cad9-223">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5cad9-224">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5cad9-225">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="5cad9-225">Dynamically select policies</span></span>

<span data-ttu-id="5cad9-226">I metodi di estensione vengono forniti per aggiungere gestori basati su Polly, ad esempio <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="5cad9-227">L'overload di `AddPolicyHandler` seguente controlla la richiesta per decidere quali criteri applicare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5cad9-228">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5cad9-229">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5cad9-230">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="5cad9-231">È comune annidare i criteri Polly:</span><span class="sxs-lookup"><span data-stu-id="5cad9-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5cad9-232">Nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-232">In the preceding example:</span></span>

* <span data-ttu-id="5cad9-233">Vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="5cad9-233">Two handlers are added.</span></span>
* <span data-ttu-id="5cad9-234">Il primo gestore USA <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> per aggiungere un criterio di ripetizione dei tentativi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="5cad9-235">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="5cad9-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="5cad9-236">La seconda chiamata di `AddTransientHttpErrorPolicy` aggiunge un criterio di interruttore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="5cad9-237">Altre richieste esterne vengono bloccate per 30 secondi se 5 tentativi non riusciti si verificano in sequenza.</span><span class="sxs-lookup"><span data-stu-id="5cad9-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="5cad9-238">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5cad9-239">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5cad9-240">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="5cad9-241">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="5cad9-242">Nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-242">In the following code:</span></span>

* <span data-ttu-id="5cad9-243">Vengono aggiunti i criteri "regular" e "Long".</span><span class="sxs-lookup"><span data-stu-id="5cad9-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="5cad9-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> aggiunge i criteri "regular" e "Long" dal registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="5cad9-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="5cad9-245">Per ulteriori informazioni sulle integrazioni `IHttpClientFactory` e Polly, vedere il [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5cad9-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5cad9-246">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="5cad9-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="5cad9-247">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-248">Viene creato un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="5cad9-249">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5cad9-250">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="5cad9-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5cad9-251">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5cad9-252">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5cad9-253">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5cad9-254">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito, che possono impedire al gestore di reagire alle modifiche DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="5cad9-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="5cad9-255">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5cad9-256">È possibile eseguire l'override del valore predefinito in base al client denominato:</span><span class="sxs-lookup"><span data-stu-id="5cad9-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="5cad9-257">le istanze di `HttpClient` possono in genere essere considerate come oggetti .NET che **non** richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="5cad9-258">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5cad9-259">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="5cad9-260">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-261">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="5cad9-262">Alternative a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="5cad9-263">L'uso di `IHttpClientFactory` in un'app abilitata per la funzionalità consente DI evitare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="5cad9-264">Problemi di esaurimento delle risorse raggruppando le istanze `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="5cad9-265">Problemi relativi a DNS obsoleti ciclando `HttpMessageHandler` istanze a intervalli regolari.</span><span class="sxs-lookup"><span data-stu-id="5cad9-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="5cad9-266">Esistono modi alternativi per risolvere i problemi precedenti utilizzando un'istanza di <xref:System.Net.Http.SocketsHttpHandler> di lunga durata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="5cad9-267">Creare un'istanza di `SocketsHttpHandler` quando l'app viene avviata e usata per il ciclo di vita dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="5cad9-268">Configurare <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> su un valore appropriato in base alle ore di aggiornamento DNS.</span><span class="sxs-lookup"><span data-stu-id="5cad9-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="5cad9-269">Creare istanze di `HttpClient` usando `new HttpClient(handler, dispostHandler: false)` in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="5cad9-270">Gli approcci precedenti risolvono i problemi di gestione delle risorse che `IHttpClientFactory` risolve in modo analogo.</span><span class="sxs-lookup"><span data-stu-id="5cad9-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="5cad9-271">Il `SocketsHttpHandler` condivide le connessioni tra le istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-272">Questa condivisione impedisce l'esaurimento del socket.</span><span class="sxs-lookup"><span data-stu-id="5cad9-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="5cad9-273">Il `SocketsHttpHandler` cicla le connessioni in base a `PooledConnectionLifetime` per evitare problemi DNS non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="5cad9-274">Cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-274">Cookies</span></span>

<span data-ttu-id="5cad9-275">Le istanze di `HttpMessageHandler` in pool generano `CookieContainer` oggetti condivisi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="5cad9-276">La condivisione di oggetti `CookieContainer` non prevista spesso genera codice errato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="5cad9-277">Per le app che richiedono cookie, prendere in considerazione una delle seguenti operazioni:</span><span class="sxs-lookup"><span data-stu-id="5cad9-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="5cad9-278">Disabilitazione della gestione automatica dei cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="5cad9-279">Evitare `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="5cad9-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="5cad9-280">Chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per disabilitare la gestione automatica dei cookie:</span><span class="sxs-lookup"><span data-stu-id="5cad9-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="5cad9-281">Registrazione</span><span class="sxs-lookup"><span data-stu-id="5cad9-281">Logging</span></span>

<span data-ttu-id="5cad9-282">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5cad9-283">Abilitare il livello di informazioni appropriato nella configurazione di registrazione per visualizzare i messaggi di log predefiniti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="5cad9-284">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="5cad9-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5cad9-285">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5cad9-286">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="5cad9-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="5cad9-287">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-288">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5cad9-289">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5cad9-290">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-291">Nell'esempio *MyNamedClient* i messaggi vengono registrati con la categoria di log "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="5cad9-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="5cad9-292">Per la richiesta, questo errore si verifica dopo l'esecuzione di tutti gli altri gestori e immediatamente prima dell'invio della richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="5cad9-293">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5cad9-294">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="5cad9-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5cad9-295">Questo può includere modifiche alle intestazioni delle richieste o al codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="5cad9-296">L'inclusione del nome del client nella categoria log consente il filtraggio dei log per specifici client denominati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5cad9-297">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5cad9-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5cad9-298">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5cad9-299">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5cad9-300">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5cad9-301">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="5cad9-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5cad9-302">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="5cad9-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5cad9-303">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5cad9-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5cad9-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5cad9-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5cad9-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5cad9-306">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-306">In the following example:</span></span>

* <span data-ttu-id="5cad9-307"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5cad9-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5cad9-308">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5cad9-309">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="5cad9-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5cad9-310">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="5cad9-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5cad9-311">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5cad9-311">Additional resources</span></span>

* [<span data-ttu-id="5cad9-312">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="5cad9-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5cad9-313">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5cad9-314">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="5cad9-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="5cad9-315">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5cad9-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5cad9-316">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5cad9-317">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-317">It offers the following benefits:</span></span>

* <span data-ttu-id="5cad9-318">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="5cad9-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-319">Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5cad9-320">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5cad9-321">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5cad9-322">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5cad9-323">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="5cad9-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5cad9-324">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cad9-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5cad9-325">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="5cad9-325">Consumption patterns</span></span>

<span data-ttu-id="5cad9-326">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5cad9-327">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5cad9-328">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5cad9-329">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5cad9-330">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5cad9-331">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="5cad9-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="5cad9-332">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5cad9-333">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-333">Basic usage</span></span>

<span data-ttu-id="5cad9-334">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5cad9-335">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5cad9-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5cad9-336">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5cad9-337">Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="5cad9-338">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5cad9-339">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5cad9-340">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-340">Named clients</span></span>

<span data-ttu-id="5cad9-341">Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="5cad9-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5cad9-342">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5cad9-343">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*.</span><span class="sxs-lookup"><span data-stu-id="5cad9-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5cad9-344">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5cad9-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5cad9-345">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5cad9-346">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5cad9-347">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5cad9-348">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="5cad9-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5cad9-349">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="5cad9-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5cad9-350">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-350">Typed clients</span></span>

<span data-ttu-id="5cad9-351">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="5cad9-351">Typed clients:</span></span>

* <span data-ttu-id="5cad9-352">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5cad9-353">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5cad9-354">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5cad9-355">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="5cad9-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="5cad9-356">Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5cad9-357">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="5cad9-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5cad9-358">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5cad9-359">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="5cad9-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5cad9-360">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5cad9-361">Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="5cad9-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5cad9-362">Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="5cad9-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5cad9-363">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5cad9-364">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5cad9-365">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="5cad9-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5cad9-366">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5cad9-367">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5cad9-368">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5cad9-369">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5cad9-370">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-370">Generated clients</span></span>

<span data-ttu-id="5cad9-371">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="5cad9-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5cad9-372">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5cad9-373">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="5cad9-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5cad9-374">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="5cad9-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5cad9-375">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5cad9-376">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="5cad9-376">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5cad9-377">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="5cad9-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5cad9-378">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="5cad9-378">Outgoing request middleware</span></span>

<span data-ttu-id="5cad9-379">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5cad9-380">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5cad9-381">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5cad9-382">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5cad9-383">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cad9-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5cad9-384">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5cad9-385">Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="5cad9-386">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="5cad9-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5cad9-387">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="5cad9-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5cad9-388">Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5cad9-389">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5cad9-390">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5cad9-391">Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="5cad9-392">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5cad9-393">L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="5cad9-394">I gestori possono dipendere da servizi di qualsiasi ambito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="5cad9-395">I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="5cad9-396">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="5cad9-397">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5cad9-398">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5cad9-399">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5cad9-400">Passare i dati al gestore usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="5cad9-401">Usare `IHttpContextAccessor` per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="5cad9-402">Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5cad9-403">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-403">Use Polly-based handlers</span></span>

<span data-ttu-id="5cad9-404">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5cad9-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5cad9-405">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5cad9-406">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="5cad9-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5cad9-407">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-408">Le estensioni Polly:</span><span class="sxs-lookup"><span data-stu-id="5cad9-408">The Polly extensions:</span></span>

* <span data-ttu-id="5cad9-409">Supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="5cad9-410">Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5cad9-411">Il pacchetto non è incluso nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cad9-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5cad9-412">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="5cad9-412">Handle transient faults</span></span>

<span data-ttu-id="5cad9-413">La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="5cad9-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5cad9-414">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5cad9-415">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="5cad9-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5cad9-416">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5cad9-417">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="5cad9-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5cad9-418">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5cad9-419">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5cad9-420">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="5cad9-420">Dynamically select policies</span></span>

<span data-ttu-id="5cad9-421">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5cad9-422">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="5cad9-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5cad9-423">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5cad9-424">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5cad9-425">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5cad9-426">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="5cad9-427">In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:</span><span class="sxs-lookup"><span data-stu-id="5cad9-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5cad9-428">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="5cad9-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5cad9-429">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5cad9-430">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="5cad9-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5cad9-431">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5cad9-432">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5cad9-433">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5cad9-434">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5cad9-435">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="5cad9-436">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5cad9-437">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="5cad9-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5cad9-438">Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5cad9-439">Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="5cad9-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5cad9-440">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5cad9-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5cad9-441">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="5cad9-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="5cad9-442">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-443">È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="5cad9-444">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5cad9-445">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="5cad9-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5cad9-446">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5cad9-447">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5cad9-448">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5cad9-449">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="5cad9-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5cad9-450">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5cad9-451">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5cad9-452">Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="5cad9-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5cad9-453">L'eliminazione del client non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="5cad9-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="5cad9-454">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5cad9-455">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-456">Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5cad9-457">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-458">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="5cad9-459">Alternative a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="5cad9-460">L'uso di `IHttpClientFactory` in un'app abilitata per la funzionalità consente DI evitare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="5cad9-461">Problemi di esaurimento delle risorse raggruppando le istanze `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="5cad9-462">Problemi relativi a DNS obsoleti ciclando `HttpMessageHandler` istanze a intervalli regolari.</span><span class="sxs-lookup"><span data-stu-id="5cad9-462">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="5cad9-463">Esistono modi alternativi per risolvere i problemi precedenti utilizzando un'istanza di <xref:System.Net.Http.SocketsHttpHandler> di lunga durata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="5cad9-464">Creare un'istanza di `SocketsHttpHandler` quando l'app viene avviata e usata per il ciclo di vita dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="5cad9-465">Configurare <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> su un valore appropriato in base alle ore di aggiornamento DNS.</span><span class="sxs-lookup"><span data-stu-id="5cad9-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="5cad9-466">Creare istanze di `HttpClient` usando `new HttpClient(handler, dispostHandler: false)` in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="5cad9-467">Gli approcci precedenti risolvono i problemi di gestione delle risorse che `IHttpClientFactory` risolve in modo analogo.</span><span class="sxs-lookup"><span data-stu-id="5cad9-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="5cad9-468">Il `SocketsHttpHandler` condivide le connessioni tra le istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-469">Questa condivisione impedisce l'esaurimento del socket.</span><span class="sxs-lookup"><span data-stu-id="5cad9-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="5cad9-470">Il `SocketsHttpHandler` cicla le connessioni in base a `PooledConnectionLifetime` per evitare problemi DNS non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-470">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="5cad9-471">Cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-471">Cookies</span></span>

<span data-ttu-id="5cad9-472">Le istanze di `HttpMessageHandler` in pool generano `CookieContainer` oggetti condivisi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="5cad9-473">La condivisione di oggetti `CookieContainer` non prevista spesso genera codice errato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="5cad9-474">Per le app che richiedono cookie, prendere in considerazione una delle seguenti operazioni:</span><span class="sxs-lookup"><span data-stu-id="5cad9-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="5cad9-475">Disabilitazione della gestione automatica dei cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="5cad9-476">Evitare `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="5cad9-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="5cad9-477">Chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per disabilitare la gestione automatica dei cookie:</span><span class="sxs-lookup"><span data-stu-id="5cad9-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="5cad9-478">Registrazione</span><span class="sxs-lookup"><span data-stu-id="5cad9-478">Logging</span></span>

<span data-ttu-id="5cad9-479">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5cad9-480">Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5cad9-481">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="5cad9-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5cad9-482">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5cad9-483">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5cad9-484">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-485">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5cad9-486">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5cad9-487">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-488">Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5cad9-489">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="5cad9-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5cad9-490">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5cad9-491">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="5cad9-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5cad9-492">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5cad9-493">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="5cad9-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5cad9-494">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5cad9-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5cad9-495">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5cad9-496">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5cad9-497">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5cad9-498">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="5cad9-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5cad9-499">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="5cad9-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5cad9-500">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5cad9-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5cad9-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5cad9-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5cad9-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5cad9-503">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-503">In the following example:</span></span>

* <span data-ttu-id="5cad9-504"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5cad9-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5cad9-505">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5cad9-506">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="5cad9-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5cad9-507">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="5cad9-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5cad9-508">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5cad9-508">Additional resources</span></span>

* [<span data-ttu-id="5cad9-509">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="5cad9-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5cad9-510">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5cad9-511">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="5cad9-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="5cad9-512">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5cad9-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5cad9-513">È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5cad9-514">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-514">It offers the following benefits:</span></span>

* <span data-ttu-id="5cad9-515">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="5cad9-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-516">Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5cad9-517">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5cad9-518">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5cad9-519">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5cad9-520">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="5cad9-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5cad9-521">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cad9-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cad9-522">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="5cad9-522">Prerequisites</span></span>

<span data-ttu-id="5cad9-523">I progetti destinati a .NET Framework richiedono l'installazione del pacchetto NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="5cad9-524">I progetti destinati a .NET Core che fanno riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage-app) sono già inclusi nel pacchetto `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5cad9-525">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="5cad9-525">Consumption patterns</span></span>

<span data-ttu-id="5cad9-526">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5cad9-527">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5cad9-528">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5cad9-529">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5cad9-530">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5cad9-531">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="5cad9-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="5cad9-532">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5cad9-533">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="5cad9-533">Basic usage</span></span>

<span data-ttu-id="5cad9-534">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5cad9-535">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5cad9-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5cad9-536">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="5cad9-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5cad9-537">Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="5cad9-538">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5cad9-539">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5cad9-540">Client denominati</span><span class="sxs-lookup"><span data-stu-id="5cad9-540">Named clients</span></span>

<span data-ttu-id="5cad9-541">Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="5cad9-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5cad9-542">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5cad9-543">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*.</span><span class="sxs-lookup"><span data-stu-id="5cad9-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5cad9-544">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5cad9-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5cad9-545">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5cad9-546">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5cad9-547">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5cad9-548">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="5cad9-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5cad9-549">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="5cad9-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5cad9-550">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="5cad9-550">Typed clients</span></span>

<span data-ttu-id="5cad9-551">Client tipizzati:</span><span class="sxs-lookup"><span data-stu-id="5cad9-551">Typed clients:</span></span>

* <span data-ttu-id="5cad9-552">Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5cad9-553">Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5cad9-554">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5cad9-555">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="5cad9-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="5cad9-556">Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5cad9-557">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="5cad9-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5cad9-558">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5cad9-559">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="5cad9-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5cad9-560">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5cad9-561">Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="5cad9-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5cad9-562">Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="5cad9-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5cad9-563">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5cad9-564">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5cad9-565">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="5cad9-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5cad9-566">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5cad9-567">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5cad9-568">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5cad9-569">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5cad9-570">Client generati</span><span class="sxs-lookup"><span data-stu-id="5cad9-570">Generated clients</span></span>

<span data-ttu-id="5cad9-571">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="5cad9-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5cad9-572">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5cad9-573">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="5cad9-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5cad9-574">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="5cad9-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5cad9-575">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5cad9-576">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="5cad9-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5cad9-577">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="5cad9-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5cad9-578">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="5cad9-578">Outgoing request middleware</span></span>

<span data-ttu-id="5cad9-579">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5cad9-580">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5cad9-581">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5cad9-582">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="5cad9-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5cad9-583">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cad9-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5cad9-584">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5cad9-585">Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="5cad9-586">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="5cad9-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5cad9-587">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="5cad9-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5cad9-588">Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5cad9-589">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5cad9-590">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5cad9-591">Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="5cad9-592">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5cad9-593">Il gestore **deve** essere registrato nell'inserimento di dipendenze come servizio temporaneo, senza definizione di ambito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="5cad9-594">Se il gestore viene registrato come un servizio con ambito e i servizi da cui dipende il gestore sono eliminabili:</span><span class="sxs-lookup"><span data-stu-id="5cad9-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="5cad9-595">I servizi del gestore potrebbero essere eliminati prima che il gestore esca dall'ambito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="5cad9-596">I servizi del gestore eliminati causano un errore del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="5cad9-597">Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="5cad9-598">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5cad9-599">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="5cad9-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5cad9-600">Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:</span><span class="sxs-lookup"><span data-stu-id="5cad9-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5cad9-601">Passare i dati al gestore usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="5cad9-602">Usare `IHttpContextAccessor` per accedere alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="5cad9-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="5cad9-603">Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5cad9-604">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-604">Use Polly-based handlers</span></span>

<span data-ttu-id="5cad9-605">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5cad9-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5cad9-606">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="5cad9-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5cad9-607">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="5cad9-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5cad9-608">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-609">Le estensioni Polly:</span><span class="sxs-lookup"><span data-stu-id="5cad9-609">The Polly extensions:</span></span>

* <span data-ttu-id="5cad9-610">Supportano l'aggiunta di gestori basati su Polly ai client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="5cad9-611">Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="5cad9-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5cad9-612">Il pacchetto non è incluso nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cad9-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5cad9-613">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="5cad9-613">Handle transient faults</span></span>

<span data-ttu-id="5cad9-614">La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee.</span><span class="sxs-lookup"><span data-stu-id="5cad9-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5cad9-615">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5cad9-616">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="5cad9-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5cad9-617">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5cad9-618">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="5cad9-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5cad9-619">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5cad9-620">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5cad9-621">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="5cad9-621">Dynamically select policies</span></span>

<span data-ttu-id="5cad9-622">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5cad9-623">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="5cad9-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5cad9-624">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5cad9-625">Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5cad9-626">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5cad9-627">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="5cad9-628">In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:</span><span class="sxs-lookup"><span data-stu-id="5cad9-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5cad9-629">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="5cad9-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5cad9-630">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5cad9-631">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="5cad9-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5cad9-632">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5cad9-633">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5cad9-634">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5cad9-635">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="5cad9-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5cad9-636">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="5cad9-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="5cad9-637">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5cad9-638">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="5cad9-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5cad9-639">Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5cad9-640">Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="5cad9-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5cad9-641">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5cad9-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5cad9-642">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="5cad9-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="5cad9-643">Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-644">È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="5cad9-645">La factory gestisce le durate delle istanze di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5cad9-646">`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="5cad9-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5cad9-647">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5cad9-648">Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5cad9-649">La creazione di più gestori del necessario può causare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5cad9-650">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="5cad9-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5cad9-651">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="5cad9-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5cad9-652">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5cad9-653">Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="5cad9-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5cad9-654">L'eliminazione del client non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="5cad9-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="5cad9-655">L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5cad9-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5cad9-656">`IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-657">Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5cad9-658">Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5cad9-659">Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="5cad9-660">Alternative a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="5cad9-661">L'uso di `IHttpClientFactory` in un'app abilitata per la funzionalità consente DI evitare:</span><span class="sxs-lookup"><span data-stu-id="5cad9-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="5cad9-662">Problemi di esaurimento delle risorse raggruppando le istanze `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="5cad9-663">Problemi relativi a DNS obsoleti ciclando `HttpMessageHandler` istanze a intervalli regolari.</span><span class="sxs-lookup"><span data-stu-id="5cad9-663">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="5cad9-664">Esistono modi alternativi per risolvere i problemi precedenti utilizzando un'istanza di <xref:System.Net.Http.SocketsHttpHandler> di lunga durata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="5cad9-665">Creare un'istanza di `SocketsHttpHandler` quando l'app viene avviata e usata per il ciclo di vita dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cad9-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="5cad9-666">Configurare <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> su un valore appropriato in base alle ore di aggiornamento DNS.</span><span class="sxs-lookup"><span data-stu-id="5cad9-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="5cad9-667">Creare istanze di `HttpClient` usando `new HttpClient(handler, dispostHandler: false)` in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="5cad9-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="5cad9-668">Gli approcci precedenti risolvono i problemi di gestione delle risorse che `IHttpClientFactory` risolve in modo analogo.</span><span class="sxs-lookup"><span data-stu-id="5cad9-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="5cad9-669">Il `SocketsHttpHandler` condivide le connessioni tra le istanze di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="5cad9-670">Questa condivisione impedisce l'esaurimento del socket.</span><span class="sxs-lookup"><span data-stu-id="5cad9-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="5cad9-671">Il `SocketsHttpHandler` cicla le connessioni in base a `PooledConnectionLifetime` per evitare problemi DNS non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="5cad9-671">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="5cad9-672">Cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-672">Cookies</span></span>

<span data-ttu-id="5cad9-673">Le istanze di `HttpMessageHandler` in pool generano `CookieContainer` oggetti condivisi.</span><span class="sxs-lookup"><span data-stu-id="5cad9-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="5cad9-674">La condivisione di oggetti `CookieContainer` non prevista spesso genera codice errato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="5cad9-675">Per le app che richiedono cookie, prendere in considerazione una delle seguenti operazioni:</span><span class="sxs-lookup"><span data-stu-id="5cad9-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="5cad9-676">Disabilitazione della gestione automatica dei cookie</span><span class="sxs-lookup"><span data-stu-id="5cad9-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="5cad9-677">Evitare `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="5cad9-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="5cad9-678">Chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per disabilitare la gestione automatica dei cookie:</span><span class="sxs-lookup"><span data-stu-id="5cad9-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="5cad9-679">Registrazione</span><span class="sxs-lookup"><span data-stu-id="5cad9-679">Logging</span></span>

<span data-ttu-id="5cad9-680">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5cad9-681">Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="5cad9-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5cad9-682">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="5cad9-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5cad9-683">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5cad9-684">Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5cad9-685">I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-686">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="5cad9-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5cad9-687">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5cad9-688">La registrazione avviene anche all'interno della pipeline del gestore delle richieste.</span><span class="sxs-lookup"><span data-stu-id="5cad9-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5cad9-689">Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5cad9-690">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="5cad9-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5cad9-691">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="5cad9-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5cad9-692">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="5cad9-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5cad9-693">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="5cad9-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5cad9-694">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="5cad9-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5cad9-695">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5cad9-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5cad9-696">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="5cad9-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5cad9-697">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5cad9-698">È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="5cad9-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5cad9-699">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="5cad9-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5cad9-700">Usare IHttpClientFactory in un'app console</span><span class="sxs-lookup"><span data-stu-id="5cad9-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5cad9-701">In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cad9-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5cad9-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5cad9-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5cad9-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5cad9-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5cad9-704">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="5cad9-704">In the following example:</span></span>

* <span data-ttu-id="5cad9-705"><xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5cad9-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5cad9-706">`MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5cad9-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5cad9-707">`HttpClient` viene usato per recuperare una pagina Web.</span><span class="sxs-lookup"><span data-stu-id="5cad9-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5cad9-708">`Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.</span><span class="sxs-lookup"><span data-stu-id="5cad9-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5cad9-709">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5cad9-709">Additional resources</span></span>

* [<span data-ttu-id="5cad9-710">Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti</span><span class="sxs-lookup"><span data-stu-id="5cad9-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5cad9-711">Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5cad9-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5cad9-712">Implementazione dello schema Circuit Breaker</span><span class="sxs-lookup"><span data-stu-id="5cad9-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
