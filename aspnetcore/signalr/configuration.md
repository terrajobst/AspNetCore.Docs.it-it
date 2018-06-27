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
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="13b30-103">Configurazione di ASP.NET SignalR Core</span><span class="sxs-lookup"><span data-stu-id="13b30-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="13b30-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="13b30-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="13b30-105">ASP.NET SignalR Core supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="13b30-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="13b30-106">Ogni protocollo presenta opzioni di configurazione di serializzazione.</span><span class="sxs-lookup"><span data-stu-id="13b30-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="13b30-107">Serializzazione JSON può essere configurata sul server utilizzando il [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunti dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="13b30-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="13b30-108">Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto.</span><span class="sxs-lookup"><span data-stu-id="13b30-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="13b30-109">Il [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere usato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="13b30-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="13b30-110">Vedere la [JSON.NET documentazione](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="13b30-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="13b30-111">Ad esempio, per configurare il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi di "camelCase" predefiniti, usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="13b30-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="13b30-112">Nel client di .NET, lo stesso `AddJsonHubProtocol` metodo di estensione esiste nel [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="13b30-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="13b30-113">Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="13b30-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="13b30-114">Non è possibile configurare la serializzazione JSON nel client di JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="13b30-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="13b30-115">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="13b30-115">MessagePack serialization options</span></span>

<span data-ttu-id="13b30-116">Serializzazione MessagePack può essere configurata, fornendo un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare.</span><span class="sxs-lookup"><span data-stu-id="13b30-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="13b30-117">Vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="13b30-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="13b30-118">Non è possibile configurare la serializzazione MessagePack nel client di JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="13b30-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="13b30-119">Configurare le opzioni server</span><span class="sxs-lookup"><span data-stu-id="13b30-119">Configure server options</span></span>

<span data-ttu-id="13b30-120">Nella tabella seguente vengono descritte le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="13b30-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="13b30-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="13b30-121">Option</span></span> | <span data-ttu-id="13b30-122">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="13b30-123">Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="13b30-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="13b30-124">Se il server non è stato inviato un messaggio entro questo intervallo, per tenere aperta la connessione viene automaticamente inviato un messaggio di ping.</span><span class="sxs-lookup"><span data-stu-id="13b30-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="13b30-125">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="13b30-125">Protocols supported by this hub.</span></span> <span data-ttu-id="13b30-126">Per impostazione predefinita, sono consentiti tutti i protocolli registrati sul server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per gli hub singoli.</span><span class="sxs-lookup"><span data-stu-id="13b30-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="13b30-127">Se `true`dettagliate i messaggi di eccezione vengono restituiti al client quando viene generata un'eccezione in un metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="13b30-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="13b30-128">Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili.</span><span class="sxs-lookup"><span data-stu-id="13b30-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="13b30-129">Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="13b30-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="13b30-130">Opzioni per un singolo hub sostituiranno globale fornita `AddSignalR` e possono essere configurate tramite [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="13b30-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="13b30-131">Utilizzare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate relative alla gestione buffer di memoria e trasporti.</span><span class="sxs-lookup"><span data-stu-id="13b30-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="13b30-132">Queste opzioni vengono configurate passando un delegato da [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="13b30-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="13b30-133">Opzione</span><span class="sxs-lookup"><span data-stu-id="13b30-133">Option</span></span> | <span data-ttu-id="13b30-134">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="13b30-135">Il numero massimo di byte ricevuti dal client che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="13b30-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="13b30-136">Aumentando questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="13b30-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="13b30-137">Il valore predefinito è 32KB.</span><span class="sxs-lookup"><span data-stu-id="13b30-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="13b30-138">Un elenco di [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) gli oggetti usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="13b30-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="13b30-139">Per impostazione predefinita, viene popolato con i valori di `Authorize` gli attributi applicati alla classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="13b30-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="13b30-140">Il numero massimo di byte inviati dall'app che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="13b30-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="13b30-141">Aumentando questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="13b30-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="13b30-142">Il valore predefinito è 32KB.</span><span class="sxs-lookup"><span data-stu-id="13b30-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="13b30-143">Maschera di bit di `HttpTransportType` valori che limitano i trasporti un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="13b30-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="13b30-144">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="13b30-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="13b30-145">Opzioni aggiuntive specifiche per il trasporto di Polling lungo.</span><span class="sxs-lookup"><span data-stu-id="13b30-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="13b30-146">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="13b30-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="13b30-147">Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate tramite la `LongPolling` proprietà:</span><span class="sxs-lookup"><span data-stu-id="13b30-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="13b30-148">Opzione</span><span class="sxs-lookup"><span data-stu-id="13b30-148">Option</span></span> | <span data-ttu-id="13b30-149">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="13b30-150">La quantità massima di tempo il server attende un messaggio da inviare al client prima di interrompere una richiesta di polling singolo.</span><span class="sxs-lookup"><span data-stu-id="13b30-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="13b30-151">Diminuendo il valore, il client per l'emissione di nuove richieste di polling più frequente.</span><span class="sxs-lookup"><span data-stu-id="13b30-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="13b30-152">Il valore predefinito è 90 secondi.</span><span class="sxs-lookup"><span data-stu-id="13b30-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="13b30-153">Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate tramite la `WebSockets` proprietà:</span><span class="sxs-lookup"><span data-stu-id="13b30-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="13b30-154">Opzione</span><span class="sxs-lookup"><span data-stu-id="13b30-154">Option</span></span> | <span data-ttu-id="13b30-155">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="13b30-156">Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="13b30-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="13b30-157">Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="13b30-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="13b30-158">Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="13b30-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="13b30-159">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="13b30-159">Configure client options</span></span>

<span data-ttu-id="13b30-160">Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo (disponibile nel client sia .NET e JavaScript), nonché dal `HubConnection` se stesso.</span><span class="sxs-lookup"><span data-stu-id="13b30-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="13b30-161">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="13b30-161">Configure logging</span></span>

<span data-ttu-id="13b30-162">La registrazione è configurata nel Client di .NET tramite il `ConfigureLogging` metodo.</span><span class="sxs-lookup"><span data-stu-id="13b30-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="13b30-163">Registrazione provider e i filtri possono essere registrata nello stesso modo come se fossero nel server.</span><span class="sxs-lookup"><span data-stu-id="13b30-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="13b30-164">Vedere la [la registrazione in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentazione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="13b30-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="13b30-165">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="13b30-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="13b30-166">Vedere la [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.</span><span class="sxs-lookup"><span data-stu-id="13b30-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="13b30-167">Ad esempio, per abilitare la registrazione nella Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="13b30-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="13b30-168">Chiamare il `AddConsole` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="13b30-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="13b30-169">Nel client di JavaScript, un simile `configureLogging` metodo esiste.</span><span class="sxs-lookup"><span data-stu-id="13b30-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="13b30-170">Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="13b30-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="13b30-171">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="13b30-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="13b30-172">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` metodo.</span><span class="sxs-lookup"><span data-stu-id="13b30-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="13b30-173">I livelli di log disponibili per il client JavaScript sono elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="13b30-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="13b30-174">Impostando il livello di registrazione su uno di questi valori consente la registrazione dei messaggi **o versione successiva** tale livello.</span><span class="sxs-lookup"><span data-stu-id="13b30-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="13b30-175">Livello</span><span class="sxs-lookup"><span data-stu-id="13b30-175">Level</span></span> | <span data-ttu-id="13b30-176">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="13b30-177">Non vengono registrati messaggi.</span><span class="sxs-lookup"><span data-stu-id="13b30-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="13b30-178">Messaggi che indicano un errore nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="13b30-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="13b30-179">Messaggi che indicano un errore nell'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="13b30-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="13b30-180">Messaggi che indicano un problema non irreversibile.</span><span class="sxs-lookup"><span data-stu-id="13b30-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="13b30-181">Messaggi informativi.</span><span class="sxs-lookup"><span data-stu-id="13b30-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="13b30-182">Messaggi di diagnostica utili per il debug.</span><span class="sxs-lookup"><span data-stu-id="13b30-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="13b30-183">Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici.</span><span class="sxs-lookup"><span data-stu-id="13b30-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="13b30-184">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="13b30-184">Configure allowed transports</span></span>

<span data-ttu-id="13b30-185">È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="13b30-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="13b30-186">Un OR bit per bit dei valori di `HttpTransportType` può essere usato per limitare il client per usare solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="13b30-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="13b30-187">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="13b30-187">All transports are enabled by default.</span></span>

<span data-ttu-id="13b30-188">Ad esempio, per disabilitare il trasporto Server-Sent eventi, ma consentire le connessioni WebSocket e di Polling lungo:</span><span class="sxs-lookup"><span data-stu-id="13b30-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="13b30-189">Nel client di JavaScript, i trasporti configurati impostando il `transport` campo per l'oggetto di opzioni fornita per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="13b30-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="13b30-190">Configurare l'autenticazione della connessione</span><span class="sxs-lookup"><span data-stu-id="13b30-190">Configure bearer authentication</span></span>

<span data-ttu-id="13b30-191">Per fornire i dati di autenticazione insieme richieste SignalR, utilizzare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="13b30-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="13b30-192">Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (utilizzando la `Authorization` intestazione con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="13b30-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="13b30-193">Nel client di JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste WebSocket ed eventi Server-Sent).</span><span class="sxs-lookup"><span data-stu-id="13b30-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="13b30-194">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="13b30-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="13b30-195">Nel client di .NET, il `AccessTokenProvider` opzione può essere specificata utilizzando il delegato opzioni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="13b30-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="13b30-196">Nel client di JavaScript, il token di accesso è configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="13b30-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="13b30-197">Configurare timeout e opzioni keep-alive</span><span class="sxs-lookup"><span data-stu-id="13b30-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="13b30-198">Sono disponibili in opzioni aggiuntive per la configurazione dei timeout e il comportamento keep-alive il `HubConnection` oggetto stesso:</span><span class="sxs-lookup"><span data-stu-id="13b30-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="13b30-199">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="13b30-199">.NET Option</span></span> | <span data-ttu-id="13b30-200">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="13b30-200">JavaScript Option</span></span> | <span data-ttu-id="13b30-201">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="13b30-202">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="13b30-202">Timeout for server activity.</span></span> <span data-ttu-id="13b30-203">Se tutti i messaggi non inviati dal server in questo intervallo, il client considera il trigger server disconnesso il `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="13b30-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="13b30-204">Non è configurabile</span><span class="sxs-lookup"><span data-stu-id="13b30-204">Not configurable</span></span> | <span data-ttu-id="13b30-205">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="13b30-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="13b30-206">Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger il `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="13b30-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="13b30-207">Nel Client di .NET, i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="13b30-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="13b30-208">Nel client di JavaScript, i valori di timeout vengono specificati come numeri.</span><span class="sxs-lookup"><span data-stu-id="13b30-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="13b30-209">I numeri rappresentano i valori di tempo in millisecondi.</span><span class="sxs-lookup"><span data-stu-id="13b30-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="13b30-210">Configurare le opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="13b30-210">Configure additional options</span></span>

<span data-ttu-id="13b30-211">Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo su `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="13b30-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="13b30-212">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="13b30-212">.NET Option</span></span> | <span data-ttu-id="13b30-213">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="13b30-213">JavaScript Option</span></span> | <span data-ttu-id="13b30-214">Descrizione</span><span class="sxs-lookup"><span data-stu-id="13b30-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="13b30-215">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione della connessione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="13b30-216">Impostare questa proprietà su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="13b30-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="13b30-217">**Supportato solo quando il trasporto WebSocket è il trasporto attivato solo**.</span><span class="sxs-lookup"><span data-stu-id="13b30-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="13b30-218">Questa impostazione non può essere abilitata quando si utilizza il servizio di Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="13b30-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="13b30-219">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-219">Not configurable \*</span></span> | <span data-ttu-id="13b30-220">Raccolta di certificati TLS da inviare autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="13b30-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="13b30-221">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-221">Not configurable \*</span></span> | <span data-ttu-id="13b30-222">La raccolta dei cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="13b30-223">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-223">Not configurable \*</span></span> | <span data-ttu-id="13b30-224">Credenziali per l'invio con tutte le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="13b30-225">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-225">Not configurable \*</span></span> | <span data-ttu-id="13b30-226">Solo i WebSocket.</span><span class="sxs-lookup"><span data-stu-id="13b30-226">WebSockets only.</span></span> <span data-ttu-id="13b30-227">La quantità massima di tempo il client è in attesa dopo la chiusura per il server confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="13b30-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="13b30-228">Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="13b30-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="13b30-229">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-229">Not configurable \*</span></span> | <span data-ttu-id="13b30-230">Un dizionario di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="13b30-231">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-231">Not configurable \*</span></span> | <span data-ttu-id="13b30-232">Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` consente di inviare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="13b30-233">Non utilizzato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="13b30-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="13b30-234">Questo delegato deve restituire un valore diverso da null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="13b30-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="13b30-235">Modificare le impostazioni del valore predefinito e restituirlo o restituiscono un completamente nuovo `HttpMessageHandler` istanza.</span><span class="sxs-lookup"><span data-stu-id="13b30-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="13b30-236">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-236">Not configurable \*</span></span> | <span data-ttu-id="13b30-237">Un proxy HTTP da utilizzare quando si inviano richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="13b30-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="13b30-238">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-238">Not configurable \*</span></span> | <span data-ttu-id="13b30-239">Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e i WebSocket.</span><span class="sxs-lookup"><span data-stu-id="13b30-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="13b30-240">In questo modo l'utilizzo dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="13b30-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="13b30-241">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="13b30-241">Not configurable \*</span></span> | <span data-ttu-id="13b30-242">Delegato che può essere utilizzato per configurare le opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="13b30-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="13b30-243">Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="13b30-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="13b30-244">Opzioni contrassegnate con un asterisco (\*) non sono configurabili nel client di JavaScript, a causa delle limitazioni nell'API di browser.</span><span class="sxs-lookup"><span data-stu-id="13b30-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="13b30-245">Nel Client di .NET, queste opzioni possono essere modificate dal delegato opzioni fornito per `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="13b30-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="13b30-246">Nel Client di JavaScript, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="13b30-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="13b30-247">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="13b30-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
