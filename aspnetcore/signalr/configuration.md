---
title: Configurazione di ASP.NET SignalR Core
author: rachelappel
description: Informazioni su come configurare le app di ASP.NET SignalR Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961983"
---
# <a name="aspnet-core-signalr-configuration"></a>Configurazione di ASP.NET SignalR Core

## <a name="jsonmessagepack-serialization-options"></a>Opzioni di serializzazione JSON/MessagePack

ASP.NET SignalR Core supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html). Ogni protocollo presenta opzioni di configurazione di serializzazione.

Serializzazione JSON può essere configurata sul server utilizzando il [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunti dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` metodo. Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto. Il [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere usato per configurare la serializzazione di argomenti e valori restituiti. Vedere la [JSON.NET documentazione](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.

Ad esempio, per configurare il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi di "camelCase" predefiniti, usare il codice seguente:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

Nel client di .NET, lo stesso `AddJsonHubProtocol` metodo di estensione esiste nel [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Non è possibile configurare la serializzazione JSON nel client di JavaScript in questo momento.

### <a name="messagepack-serialization-options"></a>Opzioni di serializzazione MessagePack

Serializzazione MessagePack può essere configurata, fornendo un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare. Vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.

> [!NOTE]
> Non è possibile configurare la serializzazione MessagePack nel client di JavaScript in questo momento.

## <a name="configure-server-options"></a>Configurare le opzioni server

Nella tabella seguente vengono descritte le opzioni per la configurazione degli hub SignalR:

| Opzione | Descrizione |
| ------ | ----------- |
| `HandshakeTimeout` | Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa. |
| `KeepAliveInterval` | Se il server non è stato inviato un messaggio entro questo intervallo, per tenere aperta la connessione viene automaticamente inviato un messaggio di ping. |
| `SupportedProtocols` | Protocolli supportati da questo hub. Per impostazione predefinita, sono consentiti tutti i protocolli registrati sul server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per gli hub singoli. |
| `EnableDetailedErrors` | Se `true`dettagliate i messaggi di eccezione vengono restituiti al client quando viene generata un'eccezione in un metodo dell'Hub. Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili. |

Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Opzioni per un singolo hub sostituiranno globale fornita `AddSignalR` e possono essere configurate tramite [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Utilizzare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate relative alla gestione buffer di memoria e trasporti. Queste opzioni vengono configurate passando un delegato da [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Opzione | Descrizione |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Il numero massimo di byte ricevuti dal client che il buffer del server. Aumentando questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. Il valore predefinito è 32KB. |
| `AuthorizationData` | Un elenco di [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) gli oggetti usati per determinare se un client è autorizzato a connettersi all'hub. Per impostazione predefinita, viene popolato con i valori di `Authorize` gli attributi applicati alla classe dell'Hub. |
| `TransportMaxBufferSize` | Il numero massimo di byte inviati dall'app che il buffer del server. Aumentando questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. Il valore predefinito è 32KB. |
| `Transports` | Maschera di bit di `HttpTransportType` valori che limitano i trasporti un client può utilizzare per la connessione. Tutti i trasporti sono abilitati per impostazione predefinita. |
| `LongPolling` | Opzioni aggiuntive specifiche per il trasporto di Polling lungo. |
| `WebSockets` | Opzioni aggiuntive specifiche per il trasporto WebSocket. |

Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate tramite la `LongPolling` proprietà:

| Opzione | Descrizione |
| ------ | ----------- |
| `PollTimeout` | La quantità massima di tempo il server attende un messaggio da inviare al client prima di interrompere una richiesta di polling singolo. Diminuendo il valore, il client per l'emissione di nuove richieste di polling più frequente. Il valore predefinito è 90 secondi. |

Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate tramite la `WebSockets` proprietà:

| Opzione | Descrizione |
| ------ | ----------- |
| `CloseTimeout` | Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata. |
| `SubProtocolSelector` | Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato. Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato. |

## <a name="configure-client-options"></a>Configurare le opzioni client

Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo (disponibile nel client sia .NET e JavaScript), nonché dal `HubConnection` se stesso.

### <a name="configure-logging"></a>Configurare la registrazione

La registrazione è configurata nel Client di .NET tramite il `ConfigureLogging` metodo. Registrazione provider e i filtri possono essere registrata nello stesso modo come se fossero nel server. Vedere la [la registrazione in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentazione per ulteriori informazioni.

> [!NOTE]
> Per registrare i provider di registrazione, è necessario installare i pacchetti necessari. Vedere la [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.

Ad esempio, per abilitare la registrazione nella Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet. Chiamare il `AddConsole` metodo di estensione:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Nel client di JavaScript, un simile `configureLogging` metodo esiste. Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre. I log vengono scritti nella finestra della console del browser.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` metodo.

I livelli di log disponibili per il client JavaScript sono elencati di seguito. Impostando il livello di registrazione su uno di questi valori consente la registrazione dei messaggi **o versione successiva** tale livello.

| Livello | Descrizione |
| ----- | ----------- |
| `None` | Non vengono registrati messaggi. |
| `Critical` | Messaggi che indicano un errore nell'intera app. |
| `Error` | Messaggi che indicano un errore nell'operazione corrente. |
| `Warning` | Messaggi che indicano un problema non irreversibile. |
| `Information` | Messaggi informativi. |
| `Debug` | Messaggi di diagnostica utili per il debug. |
| `Trace` | Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici. |

### <a name="configure-allowed-transports"></a>Configurare i trasporti consentiti

È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript). Un OR bit per bit dei valori di `HttpTransportType` può essere usato per limitare il client per usare solo i trasporti specificati. Tutti i trasporti sono abilitati per impostazione predefinita.

Ad esempio, per disabilitare il trasporto Server-Sent eventi, ma consentire le connessioni WebSocket e di Polling lungo:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

Nel client di JavaScript, i trasporti configurati impostando il `transport` campo per l'oggetto di opzioni fornita per `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Configurare l'autenticazione della connessione

Per fornire i dati di autenticazione insieme richieste SignalR, utilizzare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato. Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (utilizzando la `Authorization` intestazione con un tipo di `Bearer`). Nel client di JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste WebSocket ed eventi Server-Sent). In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.

Nel client di .NET, il `AccessTokenProvider` opzione può essere specificata utilizzando il delegato opzioni `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

Nel client di JavaScript, il token di accesso è configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Configurare timeout e opzioni keep-alive

Sono disponibili in opzioni aggiuntive per la configurazione dei timeout e il comportamento keep-alive il `HubConnection` oggetto stesso:

| Opzione .NET | Opzione di JavaScript | Descrizione |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Timeout per l'attività del server. Se tutti i messaggi non inviati dal server in questo intervallo, il client considera il trigger server disconnesso il `Closed` evento (`onclose` in JavaScript). |
| `HandshakeTimeout` | Non è configurabile | Timeout per l'handshake iniziale del server. Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger il `Closed` evento (`onclose` in JavaScript). |

Nel Client di .NET, i valori di timeout vengono specificati come `TimeSpan` valori. Nel client di JavaScript, i valori di timeout vengono specificati come numeri. I numeri rappresentano i valori di tempo in millisecondi.

### <a name="configure-additional-options"></a>Configurare le opzioni aggiuntive

Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo su `HubConnectionBuilder`:

| Opzione .NET | Opzione di JavaScript | Descrizione |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Una funzione che restituisce una stringa che viene fornita come un token di autenticazione della connessione nelle richieste HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Impostare questa proprietà su `true` per ignorare il passaggio di negoziazione. **Supportato solo quando il trasporto WebSocket è il trasporto attivato solo**. Questa impostazione non può essere abilitata quando si utilizza il servizio di Azure SignalR. |
| `ClientCertificates` | Non è configurabile * | Raccolta di certificati TLS da inviare autenticare le richieste. |
| `Cookies` | Non è configurabile * | La raccolta dei cookie HTTP da inviare con ogni richiesta HTTP. |
| `Credentials` | Non è configurabile * | Credenziali per l'invio con tutte le richieste HTTP. |
| `CloseTimeout` | Non è configurabile * | Solo i WebSocket. La quantità massima di tempo il client è in attesa dopo la chiusura per il server confermare la richiesta di chiusura. Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette. |
| `Headers` | Non è configurabile * | Un dizionario di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP. |
| `HttpMessageHandlerFactory` | Non è configurabile * | Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` consente di inviare richieste HTTP. Non utilizzato per le connessioni WebSocket. Questo delegato deve restituire un valore diverso da null e riceve il valore predefinito come parametro. Modificare le impostazioni del valore predefinito e restituirlo o restituiscono un completamente nuovo `HttpMessageHandler` istanza. |
| `Proxy` | Non è configurabile * | Un proxy HTTP da utilizzare quando si inviano richieste HTTP. |
| `UseDefaultCredentials` | Non è configurabile * | Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e i WebSocket. In questo modo l'utilizzo dell'autenticazione di Windows. |
| `WebSocketConfiguration` | Non è configurabile * | Delegato che può essere utilizzato per configurare le opzioni WebSocket aggiuntive. Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni. |

Opzioni contrassegnate con un asterisco (*) non sono configurabili nel client di JavaScript, a causa delle limitazioni nell'API di browser.

Nel Client di .NET, queste opzioni possono essere modificate dal delegato opzioni fornito per `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

Nel Client di JavaScript, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
