---
title: Configurazione di ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come configurare le app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/31/2018
uid: signalr/configuration
ms.openlocfilehash: 32c0ad94fba09fa099c2ab4a6b1d6d79a5542d7f
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396062"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="4b5fd-103">Configurazione di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4b5fd-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="4b5fd-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="4b5fd-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="4b5fd-105">ASP.NET Core SignalR supporta due protocolli per la codifica messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="4b5fd-106">Ogni protocollo presenta le opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="4b5fd-107">Serializzazione JSON può essere configurata nel server usando il [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4b5fd-108">Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="4b5fd-109">Il [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="4b5fd-110">Vedere le [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="4b5fd-111">Ad esempio, per configurare il serializzatore per usare i nomi delle proprietà "PascalCase", anziché i nomi predefiniti "camelCase", usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="4b5fd-112">Nel client .NET, lo stesso `AddJsonHubProtocol` metodo di estensione esiste sul [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="4b5fd-113">Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="4b5fd-114">Non è possibile configurare la serializzazione JSON nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="4b5fd-115">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="4b5fd-115">MessagePack serialization options</span></span>

<span data-ttu-id="4b5fd-116">MessagePack serializzazione può essere configurata specificando un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="4b5fd-117">Visualizzare [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="4b5fd-118">Non è possibile configurare la serializzazione MessagePack nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="4b5fd-119">Configurare le opzioni server</span><span class="sxs-lookup"><span data-stu-id="4b5fd-119">Configure server options</span></span>

<span data-ttu-id="4b5fd-120">La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="4b5fd-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-121">Option</span></span> | <span data-ttu-id="4b5fd-122">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-122">Default Value</span></span> | <span data-ttu-id="4b5fd-123">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="4b5fd-124">15 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-124">15 seconds</span></span> | <span data-ttu-id="4b5fd-125">Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="4b5fd-126">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4b5fd-127">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="4b5fd-128">15 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-128">15 seconds</span></span> | <span data-ttu-id="4b5fd-129">Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="4b5fd-130">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="4b5fd-130">All installed protocols</span></span> | <span data-ttu-id="4b5fd-131">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-131">Protocols supported by this hub.</span></span> <span data-ttu-id="4b5fd-132">Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-132">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="4b5fd-133">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-133">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="4b5fd-134">Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-134">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="4b5fd-135">Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-135">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="4b5fd-136">Le opzioni per un singolo hub ignorare le opzioni globali disponibili in `AddSignalR` e possono essere configurati usando [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="4b5fd-136">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="4b5fd-137">Usare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate per i trasporti e gestione della memoria buffer.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-137">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="4b5fd-138">Queste opzioni vengono configurate passando un delegato [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-138">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="4b5fd-139">Opzione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-139">Option</span></span> | <span data-ttu-id="4b5fd-140">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-140">Default Value</span></span> | <span data-ttu-id="4b5fd-141">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-141">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="4b5fd-142">32 KB</span><span class="sxs-lookup"><span data-stu-id="4b5fd-142">32 KB</span></span> | <span data-ttu-id="4b5fd-143">Il numero massimo di byte ricevuti dal client che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-143">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="4b5fd-144">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-144">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="4b5fd-145">Dati raccolti automaticamente dal `Authorize` gli attributi applicati alla classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-145">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="4b5fd-146">Un elenco delle [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) oggetti utilizzati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-146">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="4b5fd-147">32 KB</span><span class="sxs-lookup"><span data-stu-id="4b5fd-147">32 KB</span></span> | <span data-ttu-id="4b5fd-148">Il numero massimo di byte inviati dall'app che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-148">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="4b5fd-149">Se si aumenta questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-149">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="4b5fd-150">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-150">All Transports are enabled.</span></span> | <span data-ttu-id="4b5fd-151">Maschera di bit del `HttpTransportType` valori che è possono limitare i trasporti un client può usare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-151">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="4b5fd-152">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-152">See below.</span></span> | <span data-ttu-id="4b5fd-153">Opzioni aggiuntive specifiche per il trasporto di Polling lungo.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-153">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="4b5fd-154">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-154">See below.</span></span> | <span data-ttu-id="4b5fd-155">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-155">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="4b5fd-156">Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate usando il `LongPolling` proprietà:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-156">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="4b5fd-157">Opzione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-157">Option</span></span> | <span data-ttu-id="4b5fd-158">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-158">Default Value</span></span> | <span data-ttu-id="4b5fd-159">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-159">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="4b5fd-160">90 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-160">90 seconds</span></span> | <span data-ttu-id="4b5fd-161">La quantità massima di tempo il server attende un messaggio da inviare al client prima di terminare una richiesta di poll singolo.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-161">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="4b5fd-162">Riduzione di questo valore fa sì che il client inviare nuove richieste di polling più frequente.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-162">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="4b5fd-163">Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate usando il `WebSockets` proprietà:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-163">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="4b5fd-164">Opzione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-164">Option</span></span> | <span data-ttu-id="4b5fd-165">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-165">Default Value</span></span> | <span data-ttu-id="4b5fd-166">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-166">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="4b5fd-167">5 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-167">5 seconds</span></span> | <span data-ttu-id="4b5fd-168">Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-168">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="4b5fd-169">Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-169">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="4b5fd-170">Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-170">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="4b5fd-171">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="4b5fd-171">Configure client options</span></span>

<span data-ttu-id="4b5fd-172">Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo, disponibile nel client sia .NET sia JavaScript, nonché nel `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-172">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="4b5fd-173">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-173">Configure logging</span></span>

<span data-ttu-id="4b5fd-174">La registrazione è configurata nel Client .NET usando il `ConfigureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-174">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="4b5fd-175">Registrazione provider e i filtri possono essere registrata nello stesso modo come sono nel server.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-175">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="4b5fd-176">Vedere le [registrazione in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-176">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="4b5fd-177">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-177">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="4b5fd-178">Vedere le [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-178">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="4b5fd-179">Ad esempio, per abilitare la registrazione della Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-179">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="4b5fd-180">Chiamare il `AddConsole` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-180">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="4b5fd-181">Nel client JavaScript, un simile `configureLogging` esiste alcun metodo.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-181">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="4b5fd-182">Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-182">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="4b5fd-183">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-183">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="4b5fd-184">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-184">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="4b5fd-185">Livelli di log disponibili per il client JavaScript sono elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-185">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="4b5fd-186">Impostazione del livello di log a uno di questi valori consente la registrazione di messaggi **o versione successiva** tale livello.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-186">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="4b5fd-187">Livello</span><span class="sxs-lookup"><span data-stu-id="4b5fd-187">Level</span></span> | <span data-ttu-id="4b5fd-188">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-188">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="4b5fd-189">Viene registrato alcun messaggio.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-189">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="4b5fd-190">Messaggi che indicano un errore nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-190">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="4b5fd-191">Messaggi che indicano un errore dell'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-191">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="4b5fd-192">Messaggi che indicano un problema non grave.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-192">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="4b5fd-193">Messaggi informativi.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-193">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="4b5fd-194">Messaggi di diagnostica utili per il debug.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-194">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="4b5fd-195">Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-195">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="4b5fd-196">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="4b5fd-196">Configure allowed transports</span></span>

<span data-ttu-id="4b5fd-197">È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-197">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="4b5fd-198">Un OR bit per bit dei valori di `HttpTransportType` può essere utilizzato per limitare il client per usare solo i tipi di trasporto specificati.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-198">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="4b5fd-199">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-199">All transports are enabled by default.</span></span>

<span data-ttu-id="4b5fd-200">Ad esempio, per disabilitare il trasporto di eventi Server-Sent, ma consentire le connessioni WebSocket e di Polling lungo:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-200">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="4b5fd-201">Nel client JavaScript, i trasporti vengono configurati impostando il `transport` campo per l'oggetto di opzioni fornita a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-201">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="4b5fd-202">Configurare l'autenticazione della connessione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-202">Configure bearer authentication</span></span>

<span data-ttu-id="4b5fd-203">Per fornire i dati di autenticazione con le richieste di SignalR, usare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-203">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="4b5fd-204">Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (tramite il `Authorization` intestazione con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-204">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="4b5fd-205">Nel client JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste Server-Sent eventi e WebSockets).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-205">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="4b5fd-206">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-206">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="4b5fd-207">Nel client .NET, il `AccessTokenProvider` opzione può essere specificata tramite il delegato di opzioni in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-207">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="4b5fd-208">Nel client JavaScript, il token di accesso viene configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-208">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="4b5fd-209">Configurare i timeout e opzioni keep-alive</span><span class="sxs-lookup"><span data-stu-id="4b5fd-209">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="4b5fd-210">Sono disponibili in opzioni aggiuntive per la configurazione di timeout e il comportamento keep-alive di `HubConnection` oggetto stesso:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-210">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="4b5fd-211">Opzione di .NET</span><span class="sxs-lookup"><span data-stu-id="4b5fd-211">.NET Option</span></span> | <span data-ttu-id="4b5fd-212">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b5fd-212">JavaScript Option</span></span> | <span data-ttu-id="4b5fd-213">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-213">Default Value</span></span> | <span data-ttu-id="4b5fd-214">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-214">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="4b5fd-215">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="4b5fd-215">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="4b5fd-216">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-216">Timeout for server activity.</span></span> <span data-ttu-id="4b5fd-217">Se il server non ha inviato un messaggio in questo intervallo, il client considera il trigger server disconnessa la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-217">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="4b5fd-218">Non è configurabile</span><span class="sxs-lookup"><span data-stu-id="4b5fd-218">Not configurable</span></span> | <span data-ttu-id="4b5fd-219">15 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-219">15 seconds</span></span> | <span data-ttu-id="4b5fd-220">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-220">Timeout for initial server handshake.</span></span> <span data-ttu-id="4b5fd-221">Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-221">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="4b5fd-222">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-222">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4b5fd-223">Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4b5fd-223">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="4b5fd-224">Nel Client .NET, i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-224">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="4b5fd-225">Nel client JavaScript, i valori di timeout vengono specificati come un numero che indica la durata in millisecondi.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-225">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="4b5fd-226">Configurare le opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4b5fd-226">Configure additional options</span></span>

<span data-ttu-id="4b5fd-227">Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-227">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="4b5fd-228">Opzione di .NET</span><span class="sxs-lookup"><span data-stu-id="4b5fd-228">.NET Option</span></span> | <span data-ttu-id="4b5fd-229">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b5fd-229">JavaScript Option</span></span> | <span data-ttu-id="4b5fd-230">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="4b5fd-230">Default Value</span></span> | <span data-ttu-id="4b5fd-231">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4b5fd-231">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="4b5fd-232">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-232">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="4b5fd-233">Impostare questa proprietà su `true` per ignorare la fase di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-233">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="4b5fd-234">**Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-234">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="4b5fd-235">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-235">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="4b5fd-236">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-236">Not configurable \*</span></span> | <span data-ttu-id="4b5fd-237">Empty</span><span class="sxs-lookup"><span data-stu-id="4b5fd-237">Empty</span></span> | <span data-ttu-id="4b5fd-238">Raccolta di certificati TLS da inviare autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-238">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="4b5fd-239">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-239">Not configurable \*</span></span> | <span data-ttu-id="4b5fd-240">Empty</span><span class="sxs-lookup"><span data-stu-id="4b5fd-240">Empty</span></span> | <span data-ttu-id="4b5fd-241">Una raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-241">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="4b5fd-242">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-242">Not configurable \*</span></span> | <span data-ttu-id="4b5fd-243">Empty</span><span class="sxs-lookup"><span data-stu-id="4b5fd-243">Empty</span></span> | <span data-ttu-id="4b5fd-244">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-244">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="4b5fd-245">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-245">Not configurable \*</span></span> | <span data-ttu-id="4b5fd-246">5 secondi</span><span class="sxs-lookup"><span data-stu-id="4b5fd-246">5 seconds</span></span> | <span data-ttu-id="4b5fd-247">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-247">WebSockets only.</span></span> <span data-ttu-id="4b5fd-248">La quantità massima di tempo è in attesa dopo la chiusura per il server confermare la richiesta di chiusura al client.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-248">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="4b5fd-249">Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-249">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="4b5fd-250">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-250">Not configurable \*</span></span> | <span data-ttu-id="4b5fd-251">Empty</span><span class="sxs-lookup"><span data-stu-id="4b5fd-251">Empty</span></span> | <span data-ttu-id="4b5fd-252">Dizionario di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-252">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="4b5fd-253">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-253">Not configurable \*</span></span> | `null` | <span data-ttu-id="4b5fd-254">Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` usato per inviare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-254">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="4b5fd-255">Non è utilizzato per le connessioni di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-255">Not used for WebSocket connections.</span></span> <span data-ttu-id="4b5fd-256">Questo delegato deve restituire un valore diverso da null, e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-256">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="4b5fd-257">Modificare le impostazioni del valore predefinito e restituirlo o restituire una completamente nuova `HttpMessageHandler` istanza.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-257">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="4b5fd-258">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-258">Not configurable \*</span></span> | `null` | <span data-ttu-id="4b5fd-259">Un proxy HTTP da usare quando si inviano richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-259">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="4b5fd-260">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-260">Not configurable \*</span></span> | `false` | <span data-ttu-id="4b5fd-261">Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-261">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="4b5fd-262">In questo modo l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-262">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="4b5fd-263">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="4b5fd-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="4b5fd-264">Delegato che può essere utilizzato per configurare le opzioni aggiuntive di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-264">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="4b5fd-265">Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-265">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="4b5fd-266">Opzioni contrassegnate con un asterisco (\*) non sono configurabili nel client JavaScript, a causa delle limitazioni nel browser API.</span><span class="sxs-lookup"><span data-stu-id="4b5fd-266">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="4b5fd-267">Nel Client .NET, queste opzioni possono essere modificate dal delegato opzioni fornito a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-267">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="4b5fd-268">Nel JavaScript Client, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4b5fd-268">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="4b5fd-269">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4b5fd-269">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
