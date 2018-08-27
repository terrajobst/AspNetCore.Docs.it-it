---
title: Inizializzare richieste HTTP
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/07/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 2a1bf78edb5068d8b10d66e5ef306b1ad4395da6
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751779"
---
# <a name="initiate-http-requests"></a>Inizializzare richieste HTTP

Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)

È possibile registrare e usare [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) per configurare e creare istanze di [HttpClient](/dotnet/api/system.net.http.httpclient) in un'app. I vantaggi offerti sono i seguenti:

* Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche. Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub. È possibile registrare un client predefinito per altri scopi.
* Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.
* Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.
* Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

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

Questo uso di `IHttpClientFactory` è un ottimo modo per effettuare il refactoring di un'app esistente. Non influisce in alcun modo sulle modalità d'uso di `HttpClient`. Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).

### <a name="named-clients"></a>Client denominati

Se un'app richiede molti usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**. La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome *github*. Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.

Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.

Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`. Specificare il nome del client da creare:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Nel codice precedente non è necessario che la richiesta specifichi un nome host. Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.

### <a name="typed-clients"></a>Client tipizzati

I client tipizzati offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi. L'approccio basato su client tipizzati offre l'aiuto di IntelliSense e del compilatore quando si usano i client. La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione. Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint. Un altro vantaggio è rappresentato dalla possibilità di usare l'inserimento di dipendenze e di inserirli dove necessario nell'app.

Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Nel codice precedente la configurazione viene spostata nel client tipizzato. L'oggetto `HttpClient` viene esposto come una proprietà pubblica. È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`. Il metodo `GetAspNetDocsIssues` incapsula il codice necessario per eseguire una query e analizzare gli ultimi problemi aperti in un repository GitHub.

Per registrare un client tipizzato è possibile usare il metodo di estensione `AddHttpClient` generico all'interno di `Startup.ConfigureServices`, specificando la classe del client tipizzato:

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

Per creare un gestore, definire una classe che deriva da `DelegatingHandler`. Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Il codice precedente definisce un gestore di base. Verifica se un'intestazione `X-API-KEY` è stata inclusa nella richiesta. Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.

Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`. Questa attività viene eseguita tramite metodi di estensione in [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze. Il gestore **deve** essere registrato come temporaneo nell'inserimento di dipendenze. Dopo la registrazione è possibile chiamare [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler), passando il tipo per il gestore.

È possibile registrare più gestori nell'ordine di esecuzione. Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Usare gestori basati su Polly

`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly). Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET. Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.

Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione. Le estensioni per Polly sono disponibili nel pacchetto NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Il pacchetto non è incluso nel metapacchetto [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Per usare le estensioni, nel progetto deve essere incluso un elemento `<PackageReference />` esplicito.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=9)]

Dopo il ripristino di questo pacchetto, i metodi di estensione che supportano l'aggiunta di gestori basati su Polly ai client risultano disponibili.

### <a name="handle-transient-faults"></a>Gestire gli errori temporanei

La maggior parte degli errori comuni si verifica quando le chiamate HTTP esterne sono temporanee. Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`. I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.

L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `Startup.ConfigureServices`. L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Nel codice precedente viene definito un criterio `WaitAndRetryAsync`. Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.

### <a name="dynamically-select-policies"></a>Selezionare i criteri in modo dinamico

Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione. Una di queste estensioni è `AddPolicyHandler`, che include più overload. Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Nel codice precedente se la richiesta in uscita è un'operazione GET, viene applicato un timeout di 10 secondi. Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.

### <a name="add-multiple-polly-handlers"></a>Aggiungere più gestori Polly

Una pratica comune per offrire una funzionalità avanzata è quella di annidare i criteri Polly:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Nell'esempio precedente vengono aggiunti due gestori. Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione. Le richieste non riuscite vengono ritentate fino a tre volte. La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito. Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi. I criteri dell'interruttore di circuito sono con stato. Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.

### <a name="add-policies-from-the-polly-registry"></a>Aggiungere criteri dal registro Polly

Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`. Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Nel codice precedente vengono registrati due criteri quando si aggiunge `PolicyRegistry` a `ServiceCollection`. Per usare un criterio dal registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.

Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Gestione di HttpClient e durata

Viene restituita una nuova istanza di `HttpClient` per ogni chiamata di `CreateClient` per `IHttpClientFactory`. Esiste un [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) per ogni client denominato. `IHttpClientFactory` esegue il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse. Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.

Il pooling dei gestori è consigliabile in quanto ogni gestore gestisce in genere le proprie connessioni HTTP sottostanti. La creazione di più gestori del necessario può causare ritardi di connessione. Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.

La durata del gestore predefinito è di due minuti. Il valore predefinito può essere sottoposto a override per ogni client denominato. Per eseguire l'override, chiamare [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) per l'elemento `IHttpClientBuilder` restituito al momento della creazione del client:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

L'eliminazione del client non è obbligatoria. L'eliminazione annulla le richieste in uscita e garantisce che l'istanza di `HttpClient` specificata non possa essere usata dopo la chiamata di [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose). `IHttpClientFactory` tiene traccia ed elimina le risorse usate dalle istanze di `HttpClient`. Le istanze di `HttpClient` possono essere considerate a livello generale come oggetti .NET che non richiedono l'eliminazione.

Mantenere una sola istanza di `HttpClient` attiva per un lungo periodo di tempo è un modello comune usato prima dell'avvento di `IHttpClientFactory`. Questo modello non è più necessario dopo la migrazione a `IHttpClientFactory`.

## <a name="logging"></a>Registrazione

I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste. Per visualizzare i messaggi di log predefiniti, abilitare il livello di informazioni appropriato nella configurazione di registrazione. La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.

La categoria di log usata per ogni client include il nome del client. Un client denominato *MyNamedClient*, ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. I messaggi con suffisso *LogicalHandler* sono esterni alla pipeline del gestore delle richieste. Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata. Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.

La registrazione avviene anche all'interno della pipeline del gestore delle richieste. Nell'esempio *MyNamedClient*, i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete. Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.

L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline. Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.

L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.

## <a name="configure-the-httpmessagehandler"></a>Configurare HttpMessageHandler

Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.

Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`. Il metodo di estensione [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) può essere usato per definire un delegato. Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]
