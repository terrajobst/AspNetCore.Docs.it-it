---
title: Configurazione di ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come configurare le app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 8c9fcaecb04555718f5da6a42a8e56c258e795af
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813444"
---
# <a name="aspnet-core-signalr-configuration"></a>Configurazione di ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Opzioni di serializzazione JSON/MessagePack

ASP.NET Core SignalR supporta due protocolli per la codifica messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html). Ogni protocollo presenta le opzioni di configurazione della serializzazione.

Serializzazione JSON può essere configurata nel server usando il [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` (metodo). Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto. Il [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti. Vedere le [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.

Ad esempio, per configurare il serializzatore per usare i nomi delle proprietà "PascalCase", anziché i nomi predefiniti "camelCase", usare il codice seguente:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

Nel client .NET, lo stesso `AddJsonProtocol` metodo di estensione esiste sul [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> Non è possibile configurare la serializzazione JSON nel client JavaScript in questo momento.

### <a name="messagepack-serialization-options"></a>Opzioni di serializzazione MessagePack

MessagePack serializzazione può essere configurata specificando un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare. Visualizzare [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.

> [!NOTE]
> Non è possibile configurare la serializzazione MessagePack nel client JavaScript in questo momento.

## <a name="configure-server-options"></a>Configurare le opzioni server

La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:

::: moniker range=">= aspnetcore-3.0"

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 secondi | Il server considererà il client disconnesso se non è stato ricevuto un messaggio (incluso keep-alive) in questo intervallo. Potrebbero richiedere più tempo rispetto a questo intervallo di timeout per il client deve essere contrassegnato effettivamente disconnessi, a causa di questa modalità di implementazione. Il valore consigliato è double il `KeepAliveInterval` valore.|
| `HandshakeTimeout` | 15 secondi | Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa. Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake. Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 secondi | Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione. Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client. L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.  |
| `SupportedProtocols` | Tutti i protocolli installati | Protocolli supportati da questo hub. Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub. |
| `EnableDetailedErrors` | `false` | Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub. Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili. |
| `StreamBufferCapacity` | `10` | Il numero massimo di elementi che possono essere memorizzati nel buffer per il client di carica i flussi. Se viene raggiunto questo limite, l'elaborazione di chiamate viene bloccata fino a quando il server elabora gli elementi di flusso.|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 secondi | Il server considererà il client disconnesso se non è stato ricevuto un messaggio (incluso keep-alive) in questo intervallo. Potrebbero richiedere più tempo rispetto a questo intervallo di timeout per il client deve essere contrassegnato effettivamente disconnessi, a causa di questa modalità di implementazione. Il valore consigliato è double il `KeepAliveInterval` valore.|
| `HandshakeTimeout` | 15 secondi | Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa. Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake. Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 secondi | Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione. Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client. L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.  |
| `SupportedProtocols` | Tutti i protocolli installati | Protocolli supportati da questo hub. Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub. |
| `EnableDetailedErrors` | `false` | Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub. Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Opzione | Default Value | DESCRIZIONE |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 secondi | Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa. Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake. Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 secondi | Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione. Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client. L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.  |
| `SupportedProtocols` | Tutti i protocolli installati | Protocolli supportati da questo hub. Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub. |
| `EnableDetailedErrors` | `false` | Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub. Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili. |

::: moniker-end

Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare in `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Le opzioni per un singolo hub ignorare le opzioni globali disponibili in `AddSignalR` e possono essere configurati usando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Opzioni di configurazione HTTP avanzate

Usare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate per i trasporti e gestione della memoria buffer. Queste opzioni vengono configurate passando un delegato [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

La tabella seguente descrive le opzioni per la configurazione delle opzioni di HTTP avanzate del ASP.NET Core SignalR:

| Opzione | Default Value | DESCRIZIONE |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Il numero massimo di byte ricevuti dal client che il buffer del server. Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. |
| `AuthorizationData` | Dati raccolti automaticamente dal `Authorize` gli attributi applicati alla classe dell'Hub. | Un elenco delle [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) oggetti utilizzati per determinare se un client è autorizzato a connettersi all'hub. |
| `TransportMaxBufferSize` | 32 KB | Il numero massimo di byte inviati dall'app che il buffer del server. Se si aumenta questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. |
| `Transports` | Tutti i trasporti sono abilitati. | Maschera di bit del `HttpTransportType` valori che è possono limitare i trasporti un client può usare per la connessione. |
| `LongPolling` | come illustrato più avanti. | Opzioni aggiuntive specifiche per il trasporto di Polling lungo. |
| `WebSockets` | come illustrato più avanti. | Opzioni aggiuntive specifiche per il trasporto WebSocket. |

Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate usando il `LongPolling` proprietà:

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 secondi | La quantità massima di tempo il server attende un messaggio da inviare al client prima di terminare una richiesta di poll singolo. Riduzione di questo valore fa sì che il client inviare nuove richieste di polling più frequente. |

Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate usando il `WebSockets` proprietà:

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 secondi | Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata. |
| `SubProtocolSelector` | `null` | Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato. Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato. |

## <a name="configure-client-options"></a>Configurare le opzioni client

Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo (disponibile nel client .NET e JavaScript). È anche disponibile nel client di Java, ma il `HttpHubConnectionBuilder` sottoclasse corrisponde a ciò che contiene le opzioni di configurazione generatore, anche nel `HubConnection` stesso.

### <a name="configure-logging"></a>Configurare la registrazione

La registrazione è configurata nel Client .NET usando il `ConfigureLogging` (metodo). Registrazione provider e i filtri possono essere registrata nello stesso modo come sono nel server. Vedere le [registrazione in ASP.NET Core](xref:fundamentals/logging/index) per altre informazioni.

> [!NOTE]
> Per registrare i provider di registrazione, è necessario installare i pacchetti necessari. Vedere le [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.

Ad esempio, per abilitare la registrazione della Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet. Chiamare il `AddConsole` metodo di estensione:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Nel client JavaScript, un simile `configureLogging` esiste alcun metodo. Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre. I log vengono scritti nella finestra della console del browser.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

Invece di un `LogLevel` valore, è anche possibile fornire un `string` valore che rappresenta un nome a livello di log. Ciò è utile quando si configura la registrazione in ambienti di SignalR in cui non si ha accesso al `LogLevel` costanti.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Nella tabella seguente sono elencati i livelli di log disponibili. Il valore specificato per `configureLogging` imposta la **minimo** livello che verrà registrato di registrazione. I messaggi registrati a questo livello **o i livelli elencati dopo di esso nella tabella**, verranno registrati.

| string | LogLevel |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| `"info"` **or** `"information"` | `LogLevel.Information` |
| `"warn"` **or** `"warning"` | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).

Per altre informazioni sulla registrazione, vedere la [documentazione di diagnostica di SignalR](xref:signalr/diagnostics).

Il client SignalR Java Usa il [SLF4J](https://www.slf4j.org/) libreria per la registrazione. È un'API di registrazione di alto livello che consente agli utenti della libreria scegliere la propria implementazione di registrazione specifici, grazie alla disponibilità di una dipendenza di registrazione specifici. Il frammento di codice seguente viene illustrato come utilizzare `java.util.logging` con il client Java di SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger di alcuna operazione predefinita con il messaggio di avviso seguente:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Ciò può essere ignorato.

### <a name="configure-allowed-transports"></a>Configurare i trasporti consentiti

È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript). Un OR bit per bit dei valori di `HttpTransportType` può essere utilizzato per limitare il client per usare solo i tipi di trasporto specificati. Tutti i trasporti sono abilitati per impostazione predefinita.

Ad esempio, per disabilitare il trasporto di eventi Server-Sent, ma consentire le connessioni WebSocket e di Polling lungo:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

Nel client JavaScript, i trasporti vengono configurati impostando il `transport` campo per l'oggetto di opzioni fornita a `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

In questa versione di Java client WebSocket è il trasporto è disponibile solo.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

Nel client Java, il trasporto è selezionato con il `withTransport` metodo di `HttpHubConnectionBuilder`. Impostazioni predefinite del client Java per il trasporto WebSocket.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Il client SignalR Java non supporta ancora trasporto fallback.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Configurare l'autenticazione della connessione

Per fornire i dati di autenticazione con le richieste di SignalR, usare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato. Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (tramite il `Authorization` intestazione con un tipo di `Bearer`). Nel client JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste Server-Sent eventi e WebSockets). In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.

Nel client .NET, il `AccessTokenProvider` opzione può essere specificata tramite il delegato di opzioni in `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

Nel client JavaScript, il token di accesso viene configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

Nel client Java di SignalR, è possibile configurare un token di connessione da utilizzare per l'autenticazione, fornendo una factory di token di accesso per il [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Uso [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire un' [RxJava](https://github.com/ReactiveX/RxJava) [singolo\<String >](https://reactivex.io/documentation/single.html). Con una chiamata a [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Configurare i timeout e opzioni keep-alive

Sono disponibili in opzioni aggiuntive per la configurazione di timeout e il comportamento keep-alive di `HubConnection` oggetto stesso:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opzione | Valore predefinito | Descrizione |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 secondi (30.000 millisecondi) | Timeout per l'attività del server. Se il server non ha inviato un messaggio in questo intervallo, il client considera il trigger server disconnessa la `Closed` evento (`onclose` in JavaScript). Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout. Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo. |
| `HandshakeTimeout` | 15 secondi | Timeout per l'handshake iniziale del server. Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger la `Closed` evento (`onclose` in JavaScript). Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake. Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

Nel Client .NET, i valori di timeout vengono specificati come `TimeSpan` valori.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opzione | Valore predefinito | DESCRIZIONE |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 secondi (30.000 millisecondi) | Timeout per l'attività del server. Se il server non ha inviato un messaggio in questo intervallo, il client considera i server disconnesso e i trigger di `onclose` evento. Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout. Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opzione | Valore predefinito | DESCRIZIONE |
| ----------- | ------------- | ----------- |
|`getServerTimeout` `setServerTimeout` | 30 secondi (30.000 millisecondi) | Timeout per l'attività del server. Se il server non ha inviato un messaggio in questo intervallo, il client considera i server disconnesso e i trigger di `onClose` evento. Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout. Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo. |
| `withHandshakeResponseTimeout` | 15 secondi | Timeout per l'handshake iniziale del server. Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger di `onClose` evento. Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake. Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

---

### <a name="configure-additional-options"></a>Configurare le opzioni aggiuntive

Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo sul `HubConnectionBuilder` o le API di configurazione diversi sul `HttpHubConnectionBuilder` nel client Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opzione di .NET |  Valore predefinito | Descrizione |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP. |
| `SkipNegotiation` | `false` | Impostare questa proprietà su `true` per ignorare la fase di negoziazione. **Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**. Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR. |
| `ClientCertificates` | Empty | Raccolta di certificati TLS da inviare autenticare le richieste. |
| `Cookies` | Empty | Una raccolta di cookie HTTP da inviare con ogni richiesta HTTP. |
| `Credentials` | Empty | Credenziali da inviare con ogni richiesta HTTP. |
| `CloseTimeout` | 5 secondi | Solo WebSocket. La quantità massima di tempo è in attesa dopo la chiusura per il server confermare la richiesta di chiusura al client. Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette. |
| `Headers` | Empty | Mappa delle intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP. |
| `HttpMessageHandlerFactory` | `null` | Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` usato per inviare richieste HTTP. Non è utilizzato per le connessioni di WebSocket. Questo delegato deve restituire un valore diverso da null, e riceve il valore predefinito come parametro. Modificare le impostazioni del valore predefinito e restituirlo o restituire un nuovo `HttpMessageHandler` istanza. **Quando si sostituisce il gestore assicurarsi di copiare le impostazioni desiderate per impedire che il gestore fornito, in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non si applicano al nuovo gestore.** |
| `Proxy` | `null` | Un proxy HTTP da usare quando si inviano richieste HTTP. |
| `UseDefaultCredentials` | `false` | Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket. In questo modo l'uso dell'autenticazione di Windows. |
| `WebSocketConfiguration` | `null` | Delegato che può essere utilizzato per configurare le opzioni aggiuntive di WebSocket. Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opzione di JavaScript | Default Value | DESCRIZIONE |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP. |
| `skipNegotiation` | `false` | Impostare questa proprietà su `true` per ignorare la fase di negoziazione. **Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**. Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opzione Java | Default Value | Descrizione |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP. |
| `shouldSkipNegotiate` | `false` | Impostare questa proprietà su `true` per ignorare la fase di negoziazione. **Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**. Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR. |
| `withHeader` `withHeaders` | Empty | Mappa delle intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP. |

---

Nel Client .NET, queste opzioni possono essere modificate dal delegato opzioni fornito a `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

Nel JavaScript Client, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

Nel client Java, queste opzioni possono essere configurate con i metodi su di `HttpHubConnectionBuilder` restituito dal `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
