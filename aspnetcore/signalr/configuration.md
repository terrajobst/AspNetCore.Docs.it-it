---
title: Configurazione di ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come configurare le app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: bb18ba242584afa7181dcc19a5295f86996aeaa3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837520"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="3dd1a-103">Configurazione di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3dd1a-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="3dd1a-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="3dd1a-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="3dd1a-105">ASP.NET Core SignalR supporta due protocolli per la codifica messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="3dd1a-106">Ogni protocollo presenta le opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="3dd1a-107">Serializzazione JSON può essere configurata nel server usando il [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` (metodo).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3dd1a-108">Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="3dd1a-109">Il [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="3dd1a-110">Vedere le [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="3dd1a-111">Ad esempio, per configurare il serializzatore per usare i nomi delle proprietà "PascalCase", anziché i nomi predefiniti "camelCase", usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="3dd1a-112">Nel client .NET, lo stesso `AddJsonProtocol` metodo di estensione esiste sul [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="3dd1a-113">Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="3dd1a-114">Non è possibile configurare la serializzazione JSON nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="3dd1a-115">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="3dd1a-115">MessagePack serialization options</span></span>

<span data-ttu-id="3dd1a-116">MessagePack serializzazione può essere configurata specificando un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="3dd1a-117">Visualizzare [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="3dd1a-118">Non è possibile configurare la serializzazione MessagePack nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="3dd1a-119">Configurare le opzioni server</span><span class="sxs-lookup"><span data-stu-id="3dd1a-119">Configure server options</span></span>

<span data-ttu-id="3dd1a-120">La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="3dd1a-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-121">Option</span></span> | <span data-ttu-id="3dd1a-122">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-122">Default Value</span></span> | <span data-ttu-id="3dd1a-123">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="3dd1a-124">15 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-124">15 seconds</span></span> | <span data-ttu-id="3dd1a-125">Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="3dd1a-126">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3dd1a-127">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3dd1a-128">15 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-128">15 seconds</span></span> | <span data-ttu-id="3dd1a-129">Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="3dd1a-130">Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="3dd1a-131">L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="3dd1a-132">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="3dd1a-132">All installed protocols</span></span> | <span data-ttu-id="3dd1a-133">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-133">Protocols supported by this hub.</span></span> <span data-ttu-id="3dd1a-134">Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3dd1a-135">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="3dd1a-136">Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="3dd1a-137">Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="3dd1a-138">Le opzioni per un singolo hub ignorare le opzioni globali disponibili in `AddSignalR` e possono essere configurati usando [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="3dd1a-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="3dd1a-139">Usare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate per i trasporti e gestione della memoria buffer.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="3dd1a-140">Queste opzioni vengono configurate passando un delegato [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="3dd1a-141">Opzione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-141">Option</span></span> | <span data-ttu-id="3dd1a-142">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-142">Default Value</span></span> | <span data-ttu-id="3dd1a-143">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="3dd1a-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="3dd1a-144">32 KB</span></span> | <span data-ttu-id="3dd1a-145">Il numero massimo di byte ricevuti dal client che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="3dd1a-146">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="3dd1a-147">Dati raccolti automaticamente dal `Authorize` gli attributi applicati alla classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="3dd1a-148">Un elenco delle [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) oggetti utilizzati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="3dd1a-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="3dd1a-149">32 KB</span></span> | <span data-ttu-id="3dd1a-150">Il numero massimo di byte inviati dall'app che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="3dd1a-151">Se si aumenta questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="3dd1a-152">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-152">All Transports are enabled.</span></span> | <span data-ttu-id="3dd1a-153">Maschera di bit del `HttpTransportType` valori che è possono limitare i trasporti un client può usare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="3dd1a-154">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-154">See below.</span></span> | <span data-ttu-id="3dd1a-155">Opzioni aggiuntive specifiche per il trasporto di Polling lungo.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="3dd1a-156">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-156">See below.</span></span> | <span data-ttu-id="3dd1a-157">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="3dd1a-158">Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate usando il `LongPolling` proprietà:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="3dd1a-159">Opzione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-159">Option</span></span> | <span data-ttu-id="3dd1a-160">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-160">Default Value</span></span> | <span data-ttu-id="3dd1a-161">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="3dd1a-162">90 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-162">90 seconds</span></span> | <span data-ttu-id="3dd1a-163">La quantità massima di tempo il server attende un messaggio da inviare al client prima di terminare una richiesta di poll singolo.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="3dd1a-164">Riduzione di questo valore fa sì che il client inviare nuove richieste di polling più frequente.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="3dd1a-165">Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate usando il `WebSockets` proprietà:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="3dd1a-166">Opzione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-166">Option</span></span> | <span data-ttu-id="3dd1a-167">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-167">Default Value</span></span> | <span data-ttu-id="3dd1a-168">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="3dd1a-169">5 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-169">5 seconds</span></span> | <span data-ttu-id="3dd1a-170">Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="3dd1a-171">Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="3dd1a-172">Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="3dd1a-173">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="3dd1a-173">Configure client options</span></span>

<span data-ttu-id="3dd1a-174">Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo, disponibile nel client sia .NET sia JavaScript, nonché nel `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="3dd1a-175">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-175">Configure logging</span></span>

<span data-ttu-id="3dd1a-176">La registrazione è configurata nel Client .NET usando il `ConfigureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="3dd1a-177">Registrazione provider e i filtri possono essere registrata nello stesso modo come sono nel server.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="3dd1a-178">Vedere le [registrazione in ASP.NET Core](xref:fundamentals/logging/index) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="3dd1a-179">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="3dd1a-180">Vedere le [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="3dd1a-181">Ad esempio, per abilitare la registrazione della Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="3dd1a-182">Chiamare il `AddConsole` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="3dd1a-183">Nel client JavaScript, un simile `configureLogging` esiste alcun metodo.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="3dd1a-184">Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="3dd1a-185">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="3dd1a-186">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="3dd1a-187">Livelli di log disponibili per il client JavaScript sono elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="3dd1a-188">Impostazione del livello di log a uno di questi valori consente la registrazione di messaggi **o versione successiva** tale livello.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="3dd1a-189">Livello</span><span class="sxs-lookup"><span data-stu-id="3dd1a-189">Level</span></span> | <span data-ttu-id="3dd1a-190">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="3dd1a-191">Viene registrato alcun messaggio.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="3dd1a-192">Messaggi che indicano un errore nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="3dd1a-193">Messaggi che indicano un errore dell'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="3dd1a-194">Messaggi che indicano un problema non grave.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="3dd1a-195">Messaggi informativi.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="3dd1a-196">Messaggi di diagnostica utili per il debug.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="3dd1a-197">Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="3dd1a-198">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="3dd1a-198">Configure allowed transports</span></span>

<span data-ttu-id="3dd1a-199">È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="3dd1a-200">Un OR bit per bit dei valori di `HttpTransportType` può essere utilizzato per limitare il client per usare solo i tipi di trasporto specificati.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="3dd1a-201">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-201">All transports are enabled by default.</span></span>

<span data-ttu-id="3dd1a-202">Ad esempio, per disabilitare il trasporto di eventi Server-Sent, ma consentire le connessioni WebSocket e di Polling lungo:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="3dd1a-203">Nel client JavaScript, i trasporti vengono configurati impostando il `transport` campo per l'oggetto di opzioni fornita a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="3dd1a-204">Configurare l'autenticazione della connessione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-204">Configure bearer authentication</span></span>

<span data-ttu-id="3dd1a-205">Per fornire i dati di autenticazione con le richieste di SignalR, usare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="3dd1a-206">Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (tramite il `Authorization` intestazione con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="3dd1a-207">Nel client JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste Server-Sent eventi e WebSockets).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="3dd1a-208">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="3dd1a-209">Nel client .NET, il `AccessTokenProvider` opzione può essere specificata tramite il delegato di opzioni in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="3dd1a-210">Nel client JavaScript, il token di accesso viene configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="3dd1a-211">Configurare i timeout e opzioni keep-alive</span><span class="sxs-lookup"><span data-stu-id="3dd1a-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="3dd1a-212">Sono disponibili in opzioni aggiuntive per la configurazione di timeout e il comportamento keep-alive di `HubConnection` oggetto stesso:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="3dd1a-213">Opzione di .NET</span><span class="sxs-lookup"><span data-stu-id="3dd1a-213">.NET Option</span></span> | <span data-ttu-id="3dd1a-214">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="3dd1a-214">JavaScript Option</span></span> | <span data-ttu-id="3dd1a-215">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-215">Default Value</span></span> | <span data-ttu-id="3dd1a-216">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="3dd1a-217">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="3dd1a-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3dd1a-218">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-218">Timeout for server activity.</span></span> <span data-ttu-id="3dd1a-219">Se il server non ha inviato un messaggio in questo intervallo, il client considera il trigger server disconnessa la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3dd1a-220">Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3dd1a-221">Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="3dd1a-222">Non è configurabile</span><span class="sxs-lookup"><span data-stu-id="3dd1a-222">Not configurable</span></span> | <span data-ttu-id="3dd1a-223">15 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-223">15 seconds</span></span> | <span data-ttu-id="3dd1a-224">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="3dd1a-225">Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3dd1a-226">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3dd1a-227">Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3dd1a-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="3dd1a-228">Nel Client .NET, i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="3dd1a-229">Nel client JavaScript, i valori di timeout vengono specificati come un numero che indica la durata in millisecondi.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="3dd1a-230">Configurare le opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3dd1a-230">Configure additional options</span></span>

<span data-ttu-id="3dd1a-231">Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="3dd1a-232">Opzione di .NET</span><span class="sxs-lookup"><span data-stu-id="3dd1a-232">.NET Option</span></span> | <span data-ttu-id="3dd1a-233">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="3dd1a-233">JavaScript Option</span></span> | <span data-ttu-id="3dd1a-234">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3dd1a-234">Default Value</span></span> | <span data-ttu-id="3dd1a-235">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3dd1a-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="3dd1a-236">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="3dd1a-237">Impostare questa proprietà su `true` per ignorare la fase di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3dd1a-238">**Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3dd1a-239">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="3dd1a-240">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-240">Not configurable \*</span></span> | <span data-ttu-id="3dd1a-241">Empty</span><span class="sxs-lookup"><span data-stu-id="3dd1a-241">Empty</span></span> | <span data-ttu-id="3dd1a-242">Raccolta di certificati TLS da inviare autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="3dd1a-243">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-243">Not configurable \*</span></span> | <span data-ttu-id="3dd1a-244">Empty</span><span class="sxs-lookup"><span data-stu-id="3dd1a-244">Empty</span></span> | <span data-ttu-id="3dd1a-245">Una raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="3dd1a-246">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-246">Not configurable \*</span></span> | <span data-ttu-id="3dd1a-247">Empty</span><span class="sxs-lookup"><span data-stu-id="3dd1a-247">Empty</span></span> | <span data-ttu-id="3dd1a-248">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="3dd1a-249">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-249">Not configurable \*</span></span> | <span data-ttu-id="3dd1a-250">5 secondi</span><span class="sxs-lookup"><span data-stu-id="3dd1a-250">5 seconds</span></span> | <span data-ttu-id="3dd1a-251">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-251">WebSockets only.</span></span> <span data-ttu-id="3dd1a-252">La quantità massima di tempo è in attesa dopo la chiusura per il server confermare la richiesta di chiusura al client.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="3dd1a-253">Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="3dd1a-254">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-254">Not configurable \*</span></span> | <span data-ttu-id="3dd1a-255">Empty</span><span class="sxs-lookup"><span data-stu-id="3dd1a-255">Empty</span></span> | <span data-ttu-id="3dd1a-256">Dizionario di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="3dd1a-257">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="3dd1a-258">Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` usato per inviare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="3dd1a-259">Non è utilizzato per le connessioni di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="3dd1a-260">Questo delegato deve restituire un valore diverso da null, e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="3dd1a-261">Modificare le impostazioni del valore predefinito e restituirlo o restituire un nuovo `HttpMessageHandler` istanza.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="3dd1a-262">**Quando si sostituisce il gestore assicurarsi di copiare le impostazioni desiderate per impedire che il gestore fornito, in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non si applicano al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="3dd1a-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="3dd1a-263">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="3dd1a-264">Un proxy HTTP da usare quando si inviano richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="3dd1a-265">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="3dd1a-266">Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="3dd1a-267">In questo modo l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="3dd1a-268">Non è configurabile \*</span><span class="sxs-lookup"><span data-stu-id="3dd1a-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="3dd1a-269">Delegato che può essere utilizzato per configurare le opzioni aggiuntive di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="3dd1a-270">Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="3dd1a-271">Opzioni contrassegnate con un asterisco (\*) non sono configurabili nel client JavaScript, a causa delle limitazioni nel browser API.</span><span class="sxs-lookup"><span data-stu-id="3dd1a-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="3dd1a-272">Nel Client .NET, queste opzioni possono essere modificate dal delegato opzioni fornito a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="3dd1a-273">Nel JavaScript Client, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3dd1a-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="3dd1a-274">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3dd1a-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
