---
title: Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 7a5b5c84775ea2482034ef9f3e8a2376036e66cb
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478740"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>Effettuare richieste HTTP usando IHttpClientFactory in ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)

È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app. `IHttpClientFactory` offers the following benefits:

* Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche. For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/). A default client can be registered for general access.
* Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`. Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.
* Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances. Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.
* Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).

The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses. For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.

## <a name="consumption-patterns"></a>Modelli di consumo

`IHttpClientFactory` può essere usato in un'app in diversi modi:

* [Utilizzo di base](#basic-usage)
* [Client denominati](#named-clients)
* [Client tipizzati](#typed-clients)
* [Client generati](#generated-clients)

The best approach depends upon the app's requirements.

### <a name="basic-usage"></a>Utilizzo di base

`IHttpClientFactory` can be registered by calling `AddHttpClient`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection). The following code uses `IHttpClientFactory` to create an `HttpClient` instance:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app. It has no impact on how `HttpClient` is used. In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Client denominati

Named clients are a good choice when:

* The app requires many distinct uses of `HttpClient`.
* Many `HttpClient`s have different configuration.

Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

In the preceding code the client is configured with:

* The base address `https://api.github.com/`.
* Two headers required to work with the GitHub API.

#### <a name="createclient"></a>CreateClient

Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:

* A new instance of `HttpClient` is created.
* The configuration action is called.

To create a named client, pass its name into `CreateClient`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Nel codice precedente non è necessario che la richiesta specifichi un nome host. The code can pass just the path, since the base address configured for the client is used.

### <a name="typed-clients"></a>Client tipizzati

Client tipizzati:

* Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.
* Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.
* La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione. For example, a single typed client might be used:
  * For a single backend endpoint.
  * To encapsulate all logic dealing with the endpoint.
* Work with DI and can be injected where required in the app.

Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Nel codice precedente:

* The configuration is moved into the typed client.
* L'oggetto `HttpClient` viene esposto come una proprietà pubblica.

API-specific methods can be created that expose `HttpClient` functionality. For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.

The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze. Il client tipizzato può essere inserito e usato direttamente:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

The `HttpClient` can be encapsulated within a typed client. Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

In the preceding code, the `HttpClient` is stored in a private field. Access to the `HttpClient` is by the public `GetRepos` method.

### <a name="generated-clients"></a>Client generati

`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit). Refit è una libreria REST per .NET. Converte le API REST in interfacce live. Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.

Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:

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

È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:

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

L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:

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

## <a name="outgoing-request-middleware"></a>Middleware per richieste in uscita

`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests. `IHttpClientFactory`:

* Simplifies defining the handlers to apply for each named client.
* Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline. Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita. This pattern:

  * Is similar to the inbound middleware pipeline in ASP.NET Core.
  * Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:

    * memorizzazione nella cache
    * gestione errori
    * serializzazione
    * registrazione

To create a delegating handler:

* Derive from <xref:System.Net.Http.DelegatingHandler>.
* Eseguire l'override di <xref:System.Net.Http.DelegatingHandler.SendAsync*>. Execute code before passing the request to the next handler in the pipeline:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

The preceding code checks if the `X-API-KEY` header is in the request. If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.

More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze. L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore. Handlers can depend upon services of any scope. I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.

Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.

È possibile registrare più gestori nell'ordine di esecuzione. Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:

* Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).
* Usare <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> per accedere alla richiesta corrente.
* Creare un oggetto di archiviazione <xref:System.Threading.AsyncLocal`1> personalizzato per passare i dati.

## <a name="use-polly-based-handlers"></a>Usare gestori basati su Polly

`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly). Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET. Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.

Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione. The Polly extensions support adding Polly-based handlers to clients. Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.

### <a name="handle-transient-faults"></a>Gestire gli errori temporanei

Faults typically occur when external HTTP calls are transient. <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors. Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:

* <xref:System.Net.Http.HttpRequestException>
* HTTP 5xx
* HTTP 408

`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

Nel codice precedente viene definito un criterio `WaitAndRetryAsync`. Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.

### <a name="dynamically-select-policies"></a>Selezionare i criteri in modo dinamico

Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>. The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi. Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.

### <a name="add-multiple-polly-handlers"></a>Aggiungere più gestori Polly

It's common to nest Polly policies:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Nell'esempio precedente:

* Two handlers are added.
* The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy. Le richieste non riuscite vengono ritentate fino a tre volte.
* The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy. Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially. I criteri dell'interruttore di circuito sono con stato. Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.

### <a name="add-policies-from-the-polly-registry"></a>Aggiungere criteri dal registro Polly

Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.

Nel codice seguente:

* The "regular" and "long" polices are added.
* <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Gestione di HttpClient e durata

Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`. An <xref:System.Net.Http.HttpMessageHandler> is created per named client. La factory gestisce le durate delle istanze di `HttpMessageHandler`.

`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse. Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.

Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti. La creazione di più gestori del necessario può causare ritardi di connessione. Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.

La durata del gestore predefinito è di due minuti. The default value can be overridden on a per named client basis:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal. L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`.

Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`. Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatives to IHttpClientFactory

Using `IHttpClientFactory` in a DI-enabled app avoids:

* Resource exhaustion problems by pooling `HttpMessageHandler` instances.
* Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.

There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.

- Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.
- Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.
- Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.

The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.

- The `SocketsHttpHandler` shares connections across `HttpClient` instances. This sharing prevents socket exhaustion.
- The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.

### <a name="cookies"></a>Cookie

The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared. Unanticipated `CookieContainer` object sharing often results in incorrect code. For apps that require cookies, consider either:

 - Disabling automatic cookie handling
 - Avoiding `IHttpClientFactory`

Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Registrazione

I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste. Enable the appropriate information level in the logging configuration to see the default log messages. La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.

La categoria di log usata per ogni client include il nome del client. A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler". I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste. Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata. Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.

La registrazione avviene anche all'interno della pipeline del gestore delle richieste. In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler". For the request, this occurs after all other handlers have run and immediately before the request is sent. Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.

L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline. This may include changes to request headers or to the response status code.

Including the name of the client in the log category enables log filtering for specific named clients.

## <a name="configure-the-httpmessagehandler"></a>Configurare HttpMessageHandler

Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.

Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`. È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato. Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Usare IHttpClientFactory in un'app console

In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Nell'esempio seguente:

* <xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).
* `MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`. `HttpClient` viene usato per recuperare una pagina Web.
* `Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementazione dello schema Circuit Breaker](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)

È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app. I vantaggi offerti sono i seguenti:

* Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche. Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/). È possibile registrare un client predefinito per altri scopi.
* Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.
* Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.
* Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="consumption-patterns"></a>Modelli di consumo

`IHttpClientFactory` può essere usato in un'app in diversi modi:

* [Utilizzo di base](#basic-usage)
* [Client denominati](#named-clients)
* [Client tipizzati](#typed-clients)
* [Client generati](#generated-clients)

Nessuno di questi modi può essere considerato superiore a un altro. L'approccio migliore dipende dai vincoli dell'app.

### <a name="basic-usage"></a>Utilizzo di base

È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection). `IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente. Non influisce in alcun modo sulle modalità d'uso di `HttpClient`. Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Client denominati

Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**. La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*. Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.

Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.

Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`. Specificare il nome del client da creare:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Nel codice precedente non è necessario che la richiesta specifichi un nome host. Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.

### <a name="typed-clients"></a>Client tipizzati

Client tipizzati:

* Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.
* Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.
* La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione. Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.
* Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.

Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Nel codice precedente la configurazione viene spostata nel client tipizzato. L'oggetto `HttpClient` viene esposto come una proprietà pubblica. È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`. Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.

Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze. Il client tipizzato può essere inserito e usato direttamente:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato. Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Nel codice precedente `HttpClient` viene archiviato come un campo privato. L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.

### <a name="generated-clients"></a>Client generati

È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit). Refit è una libreria REST per .NET. Converte le API REST in interfacce live. Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.

Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:

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

È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:

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

L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:

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

## <a name="outgoing-request-middleware"></a>Middleware per richieste in uscita

`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita. `IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato. Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita. Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita. Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core. Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.

Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>. Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Il codice precedente definisce un gestore di base. Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta. Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.

Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`. Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze. L'interfaccia `IHttpClientFactory` crea un ambito di inserimento delle dipendenze separato per ogni gestore. I gestori possono dipendere da servizi di qualsiasi ambito. I servizi da cui dipendono i gestori vengono eliminati al momento dell'eliminazione del gestore.

Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.

È possibile registrare più gestori nell'ordine di esecuzione. Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:

* Passare i dati al gestore usando `HttpRequestMessage.Properties`.
* Usare `IHttpContextAccessor` per accedere alla richiesta corrente.
* Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.

## <a name="use-polly-based-handlers"></a>Usare gestori basati su Polly

`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly). Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET. Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.

Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione. Le estensioni Polly:

* Supportano l'aggiunta di gestori basati su Polly ai client.
* Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Il pacchetto non è incluso nel framework condiviso ASP.NET Core.

### <a name="handle-transient-faults"></a>Gestire gli errori temporanei

La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee. Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`. I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.

L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`. L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Nel codice precedente viene definito un criterio `WaitAndRetryAsync`. Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.

### <a name="dynamically-select-policies"></a>Selezionare i criteri in modo dinamico

Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione. Una di queste estensioni è `AddPolicyHandler`, che include più overload. Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi. Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.

### <a name="add-multiple-polly-handlers"></a>Aggiungere più gestori Polly

In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Nell'esempio precedente vengono aggiunti due gestori. Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione. Le richieste non riuscite vengono ritentate fino a tre volte. La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito. Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi. I criteri dell'interruttore di circuito sono con stato. Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.

### <a name="add-policies-from-the-polly-registry"></a>Aggiungere criteri dal registro Polly

Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`. Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`. Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.

Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Gestione di HttpClient e durata

Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`. È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato. La factory gestisce le durate delle istanze di `HttpMessageHandler`.

`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse. Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.

Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti. La creazione di più gestori del necessario può causare ritardi di connessione. Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.

La durata del gestore predefinito è di due minuti. Il valore predefinito può essere sottoposto a override per ogni client denominato. Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

L'eliminazione del client non è obbligatoria. L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`. Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.

Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`. Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatives to IHttpClientFactory

Using `IHttpClientFactory` in a DI-enabled app avoids:

* Resource exhaustion problems by pooling `HttpMessageHandler` instances.
* Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.

There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.

- Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.
- Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.
- Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.

The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.

- The `SocketsHttpHandler` shares connections across `HttpClient` instances. This sharing prevents socket exhaustion.
- The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.

### <a name="cookies"></a>Cookie

The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared. Unanticipated `CookieContainer` object sharing often results in incorrect code. For apps that require cookies, consider either:

 - Disabling automatic cookie handling
 - Avoiding `IHttpClientFactory`

Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Registrazione

I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste. Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione. La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.

La categoria di log usata per ogni client include il nome del client. Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste. Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata. Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.

La registrazione avviene anche all'interno della pipeline del gestore delle richieste. Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete. Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.

L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline. Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.

L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.

## <a name="configure-the-httpmessagehandler"></a>Configurare HttpMessageHandler

Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.

Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`. È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato. Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Usare IHttpClientFactory in un'app console

In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Nell'esempio seguente:

* <xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).
* `MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`. `HttpClient` viene usato per recuperare una pagina Web.
* `Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementazione dello schema Circuit Breaker](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)

È possibile registrare e usare un'interfaccia <xref:System.Net.Http.IHttpClientFactory> per configurare e creare istanze di <xref:System.Net.Http.HttpClient> in un'app. I vantaggi offerti sono i seguenti:

* Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche. Ad esempio, è possibile registrare e configurare un client *github* per accedere a [GitHub](https://github.com/). È possibile registrare un client predefinito per altri scopi.
* Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.
* Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.
* Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

I progetti destinati a .NET Framework richiedono l'installazione del pacchetto NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/). I progetti destinati a .NET Core che fanno riferimento al [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage-app) sono già inclusi nel pacchetto `Microsoft.Extensions.Http`.

## <a name="consumption-patterns"></a>Modelli di consumo

`IHttpClientFactory` può essere usato in un'app in diversi modi:

* [Utilizzo di base](#basic-usage)
* [Client denominati](#named-clients)
* [Client tipizzati](#typed-clients)
* [Client generati](#generated-clients)

Nessuno di questi modi può essere considerato superiore a un altro. L'approccio migliore dipende dai vincoli dell'app.

### <a name="basic-usage"></a>Utilizzo di base

È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection). `IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Questo uso di `IHttpClientFactory` è un modo efficace per effettuare il refactoring di un'app esistente. Non influisce in alcun modo sulle modalità d'uso di `HttpClient`. Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Client denominati

Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**. La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*. Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.

Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.

Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`. Specificare il nome del client da creare:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Nel codice precedente non è necessario che la richiesta specifichi un nome host. Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.

### <a name="typed-clients"></a>Client tipizzati

Client tipizzati:

* Offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.
* Offrono l'aiuto di IntelliSense e del compilatore quando si usano i client.
* La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione. Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.
* Usano l'inserimento di dipendenze e possono essere inseriti dove necessario nell'app.

Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Nel codice precedente la configurazione viene spostata nel client tipizzato. L'oggetto `HttpClient` viene esposto come una proprietà pubblica. È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`. Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.

Per registrare un client tipizzato è possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze. Il client tipizzato può essere inserito e usato direttamente:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `Startup.ConfigureServices`, anziché nel relativo costruttore:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato. Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Nel codice precedente `HttpClient` viene archiviato come un campo privato. L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.

### <a name="generated-clients"></a>Client generati

È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit). Refit è una libreria REST per .NET. Converte le API REST in interfacce live. Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.

Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:

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

È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:

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

L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:

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

## <a name="outgoing-request-middleware"></a>Middleware per richieste in uscita

`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita. `IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato. Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita. Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita. Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core. Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.

Per creare un gestore, definire una classe che deriva da <xref:System.Net.Http.DelegatingHandler>. Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Il codice precedente definisce un gestore di base. Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta. Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.

Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`. Questa attività viene eseguita tramite metodi di estensione in <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze. Il gestore **deve** essere registrato nell'inserimento di dipendenze come servizio temporaneo, senza definizione di ambito. Se il gestore viene registrato come un servizio con ambito e i servizi da cui dipende il gestore sono eliminabili:

* I servizi del gestore potrebbero essere eliminati prima che il gestore esca dall'ambito.
* I servizi del gestore eliminati causano un errore del gestore.

Dopo la registrazione è possibile chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, passando il tipo di gestore.

È possibile registrare più gestori nell'ordine di esecuzione. Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Usare uno degli approcci seguenti per condividere lo stato in base alla richiesta con i gestori di messaggi:

* Passare i dati al gestore usando `HttpRequestMessage.Properties`.
* Usare `IHttpContextAccessor` per accedere alla richiesta corrente.
* Creare un oggetto di archiviazione `AsyncLocal` personalizzato per passare i dati.

## <a name="use-polly-based-handlers"></a>Usare gestori basati su Polly

`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly). Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET. Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.

Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione. Le estensioni Polly:

* Supportano l'aggiunta di gestori basati su Polly ai client.
* Possono essere usate dopo l'installazione del pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Il pacchetto non è incluso nel framework condiviso ASP.NET Core.

### <a name="handle-transient-faults"></a>Gestire gli errori temporanei

La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee. Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`. I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.

L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`. L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Nel codice precedente viene definito un criterio `WaitAndRetryAsync`. Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.

### <a name="dynamically-select-policies"></a>Selezionare i criteri in modo dinamico

Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione. Una di queste estensioni è `AddPolicyHandler`, che include più overload. Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Nel codice precedente, se la richiesta in uscita è una richiesta HTTP GET viene applicato un timeout di 10 secondi. Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.

### <a name="add-multiple-polly-handlers"></a>Aggiungere più gestori Polly

In molti casi i criteri Polly vengono annidati per offrire funzionalità avanzate:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Nell'esempio precedente vengono aggiunti due gestori. Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione. Le richieste non riuscite vengono ritentate fino a tre volte. La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito. Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi. I criteri dell'interruttore di circuito sono con stato. Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.

### <a name="add-policies-from-the-polly-registry"></a>Aggiungere criteri dal registro Polly

Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`. Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`. Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.

Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Gestione di HttpClient e durata

Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`. È presente un <xref:System.Net.Http.HttpMessageHandler> per ogni client denominato. La factory gestisce le durate delle istanze di `HttpMessageHandler`.

`IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse. Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.

Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti. La creazione di più gestori del necessario può causare ritardi di connessione. Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.

La durata del gestore predefinito è di due minuti. Il valore predefinito può essere sottoposto a override per ogni client denominato. Per eseguire l'override, chiamare <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

L'eliminazione del client non è obbligatoria. L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata a <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`. Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.

Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`. Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatives to IHttpClientFactory

Using `IHttpClientFactory` in a DI-enabled app avoids:

* Resource exhaustion problems by pooling `HttpMessageHandler` instances.
* Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.

There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.

- Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.
- Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.
- Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.

The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.

- The `SocketsHttpHandler` shares connections across `HttpClient` instances. This sharing prevents socket exhaustion.
- The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.

### <a name="cookies"></a>Cookie

The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared. Unanticipated `CookieContainer` object sharing often results in incorrect code. For apps that require cookies, consider either:

 - Disabling automatic cookie handling
 - Avoiding `IHttpClientFactory`

Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Registrazione

I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste. Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione. La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.

La categoria di log usata per ogni client include il nome del client. Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste. Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata. Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.

La registrazione avviene anche all'interno della pipeline del gestore delle richieste. Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete. Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.

L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline. Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.

L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.

## <a name="configure-the-httpmessagehandler"></a>Configurare HttpMessageHandler

Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.

Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`. È possibile usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> per definire un delegato. Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Usare IHttpClientFactory in un'app console

In un'app console aggiungere al progetto i riferimenti ai pacchetti seguenti:

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

Nell'esempio seguente:

* <xref:System.Net.Http.IHttpClientFactory> è registrato nel contenitore di servizi dell'[host generico](xref:fundamentals/host/generic-host).
* `MyService` crea un'istanza della factory client dal servizio, che viene usata per creare una classe `HttpClient`. `HttpClient` viene usato per recuperare una pagina Web.
* `Main` crea un ambito per eseguire il metodo `GetPage` del servizio e scrivere i primi 500 caratteri del contenuto della pagina Web nella console.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare HttpClientFactory per l'implementazione di richieste HTTP resilienti](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Implementazione dei tentativi di chiamate HTTP con backoff esponenziale con i criteri di Polly e HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementazione dello schema Circuit Breaker](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
