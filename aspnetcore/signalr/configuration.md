---
title: Configurazione di ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come configurare le app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 6c7bd602e621917c491bfb1e26ff0fcfc3a565b0
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470364"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="cd282-103">Configurazione di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cd282-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="cd282-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="cd282-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="cd282-105">ASP.NET Core SignalR supporta due protocolli per la codifica messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="cd282-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="cd282-106">Ogni protocollo presenta le opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="cd282-107">Serializzazione JSON può essere configurata nel server usando il [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodo di estensione, che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel `Startup.ConfigureServices` (metodo).</span><span class="sxs-lookup"><span data-stu-id="cd282-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="cd282-108">Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto.</span><span class="sxs-lookup"><span data-stu-id="cd282-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="cd282-109">Il [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) proprietà su tale oggetto è un JSON.NET `JsonSerializerSettings` oggetto che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="cd282-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="cd282-110">Vedere le [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="cd282-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="cd282-111">Ad esempio, per configurare il serializzatore per usare i nomi delle proprietà "PascalCase", anziché i nomi predefiniti "camelCase", usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cd282-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="cd282-112">Nel client .NET, lo stesso `AddJsonProtocol` metodo di estensione esiste sul [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="cd282-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="cd282-113">Il `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi deve essere importato per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="cd282-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="cd282-114">Non è possibile configurare la serializzazione JSON nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="cd282-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="cd282-115">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="cd282-115">MessagePack serialization options</span></span>

<span data-ttu-id="cd282-116">MessagePack serializzazione può essere configurata specificando un delegato per il [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chiamare.</span><span class="sxs-lookup"><span data-stu-id="cd282-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="cd282-117">Visualizzare [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="cd282-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="cd282-118">Non è possibile configurare la serializzazione MessagePack nel client JavaScript in questo momento.</span><span class="sxs-lookup"><span data-stu-id="cd282-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="cd282-119">Configurare le opzioni server</span><span class="sxs-lookup"><span data-stu-id="cd282-119">Configure server options</span></span>

<span data-ttu-id="cd282-120">La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="cd282-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="cd282-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-121">Option</span></span> | <span data-ttu-id="cd282-122">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-122">Default Value</span></span> | <span data-ttu-id="cd282-123">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="cd282-124">30 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-124">30 seconds</span></span> | <span data-ttu-id="cd282-125">Il server considererà il client disconnesso se non è stato ricevuto un messaggio (incluso keep-alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="cd282-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="cd282-126">Potrebbero richiedere più tempo rispetto a questo intervallo di timeout per il client deve essere contrassegnato effettivamente disconnessi, a causa di questa modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="cd282-127">Il valore consigliato è double il `KeepAliveInterval` valore.</span><span class="sxs-lookup"><span data-stu-id="cd282-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="cd282-128">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-128">15 seconds</span></span> | <span data-ttu-id="cd282-129">Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="cd282-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="cd282-130">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="cd282-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="cd282-131">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="cd282-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="cd282-132">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-132">15 seconds</span></span> | <span data-ttu-id="cd282-133">Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="cd282-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="cd282-134">Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client.</span><span class="sxs-lookup"><span data-stu-id="cd282-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="cd282-135">L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.</span><span class="sxs-lookup"><span data-stu-id="cd282-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="cd282-136">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="cd282-136">All installed protocols</span></span> | <span data-ttu-id="cd282-137">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-137">Protocols supported by this hub.</span></span> <span data-ttu-id="cd282-138">Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="cd282-139">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="cd282-140">Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili.</span><span class="sxs-lookup"><span data-stu-id="cd282-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="cd282-141">Il numero massimo di elementi che possono essere memorizzati nel buffer per il client di carica i flussi.</span><span class="sxs-lookup"><span data-stu-id="cd282-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="cd282-142">Se viene raggiunto questo limite, l'elaborazione di chiamate viene bloccata fino a quando il server elabora gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="cd282-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

| <span data-ttu-id="cd282-143">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-143">Option</span></span> | <span data-ttu-id="cd282-144">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-144">Default Value</span></span> | <span data-ttu-id="cd282-145">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="cd282-146">30 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-146">30 seconds</span></span> | <span data-ttu-id="cd282-147">Il server considererà il client disconnesso se non è stato ricevuto un messaggio (incluso keep-alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="cd282-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="cd282-148">Potrebbero richiedere più tempo rispetto a questo intervallo di timeout per il client deve essere contrassegnato effettivamente disconnessi, a causa di questa modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="cd282-149">Il valore consigliato è double il `KeepAliveInterval` valore.</span><span class="sxs-lookup"><span data-stu-id="cd282-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="cd282-150">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-150">15 seconds</span></span> | <span data-ttu-id="cd282-151">Se il client non invia un messaggio di handshake iniziale all'interno di questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="cd282-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="cd282-152">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="cd282-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="cd282-153">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="cd282-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="cd282-154">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-154">15 seconds</span></span> | <span data-ttu-id="cd282-155">Se il server non ha inviato un messaggio all'interno di questo intervallo, un messaggio ping viene inviato automaticamente a mantenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="cd282-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="cd282-156">Quando si modificano `KeepAliveInterval`, modificare il `ServerTimeout` / `serverTimeoutInMilliseconds` impostazione sul client.</span><span class="sxs-lookup"><span data-stu-id="cd282-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="cd282-157">L'elemento consigliato `ServerTimeout` / `serverTimeoutInMilliseconds` valore è double il `KeepAliveInterval` valore.</span><span class="sxs-lookup"><span data-stu-id="cd282-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="cd282-158">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="cd282-158">All installed protocols</span></span> | <span data-ttu-id="cd282-159">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-159">Protocols supported by this hub.</span></span> <span data-ttu-id="cd282-160">Per impostazione predefinita, sono consentiti tutti i protocolli registrati nel server, ma i protocolli possono essere rimossi da questo elenco per disabilitare i protocolli specifici per singoli hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="cd282-161">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="cd282-162">Il valore predefinito è `false`, come i messaggi di eccezione possono contenere informazioni sensibili.</span><span class="sxs-lookup"><span data-stu-id="cd282-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="cd282-163">Opzioni possono essere configurate per tutti gli hub, fornendo un delegato di opzioni per la `AddSignalR` chiamare in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cd282-163">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="cd282-164">Le opzioni per un singolo hub ignorare le opzioni globali disponibili in `AddSignalR` e possono essere configurati usando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="cd282-164">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="cd282-165">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="cd282-165">Advanced HTTP configuration options</span></span>

<span data-ttu-id="cd282-166">Usare `HttpConnectionDispatcherOptions` per configurare impostazioni avanzate per i trasporti e gestione della memoria buffer.</span><span class="sxs-lookup"><span data-stu-id="cd282-166">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="cd282-167">Queste opzioni vengono configurate passando un delegato [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="cd282-167">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="cd282-168">La tabella seguente descrive le opzioni per la configurazione delle opzioni di HTTP avanzate del ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="cd282-168">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="cd282-169">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-169">Option</span></span> | <span data-ttu-id="cd282-170">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-170">Default Value</span></span> | <span data-ttu-id="cd282-171">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-171">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="cd282-172">32 KB</span><span class="sxs-lookup"><span data-stu-id="cd282-172">32 KB</span></span> | <span data-ttu-id="cd282-173">Il numero massimo di byte ricevuti dal client che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-173">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="cd282-174">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="cd282-174">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="cd282-175">Dati raccolti automaticamente dal `Authorize` gli attributi applicati alla classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-175">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="cd282-176">Un elenco delle [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) oggetti utilizzati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="cd282-176">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="cd282-177">32 KB</span><span class="sxs-lookup"><span data-stu-id="cd282-177">32 KB</span></span> | <span data-ttu-id="cd282-178">Il numero massimo di byte inviati dall'app che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-178">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="cd282-179">Se si aumenta questo valore consente al server di inviare messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="cd282-179">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="cd282-180">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="cd282-180">All Transports are enabled.</span></span> | <span data-ttu-id="cd282-181">Maschera di bit del `HttpTransportType` valori che è possono limitare i trasporti un client può usare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="cd282-181">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="cd282-182">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="cd282-182">See below.</span></span> | <span data-ttu-id="cd282-183">Opzioni aggiuntive specifiche per il trasporto di Polling lungo.</span><span class="sxs-lookup"><span data-stu-id="cd282-183">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="cd282-184">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="cd282-184">See below.</span></span> | <span data-ttu-id="cd282-185">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-185">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="cd282-186">Il trasporto di Polling lungo è presenti opzioni aggiuntive che possono essere configurate usando il `LongPolling` proprietà:</span><span class="sxs-lookup"><span data-stu-id="cd282-186">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="cd282-187">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-187">Option</span></span> | <span data-ttu-id="cd282-188">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-188">Default Value</span></span> | <span data-ttu-id="cd282-189">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-189">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="cd282-190">90 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-190">90 seconds</span></span> | <span data-ttu-id="cd282-191">La quantità massima di tempo il server attende un messaggio da inviare al client prima di terminare una richiesta di poll singolo.</span><span class="sxs-lookup"><span data-stu-id="cd282-191">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="cd282-192">Riduzione di questo valore fa sì che il client inviare nuove richieste di polling più frequente.</span><span class="sxs-lookup"><span data-stu-id="cd282-192">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="cd282-193">Il trasporto WebSocket è presenti opzioni aggiuntive che possono essere configurate usando il `WebSockets` proprietà:</span><span class="sxs-lookup"><span data-stu-id="cd282-193">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="cd282-194">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-194">Option</span></span> | <span data-ttu-id="cd282-195">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-195">Default Value</span></span> | <span data-ttu-id="cd282-196">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-196">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="cd282-197">5 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-197">5 seconds</span></span> | <span data-ttu-id="cd282-198">Dopo aver chiuso il server, se il client non riesce a chiudere entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="cd282-198">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="cd282-199">Un delegato che può essere utilizzato per impostare il `Sec-WebSocket-Protocol` intestazione su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="cd282-199">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="cd282-200">Il delegato riceve i valori richiesti dal client come input e deve restituire il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="cd282-200">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="cd282-201">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="cd282-201">Configure client options</span></span>

<span data-ttu-id="cd282-202">Opzioni client possono essere configurate nella `HubConnectionBuilder` tipo (disponibile nel client .NET e JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd282-202">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="cd282-203">È anche disponibile nel client di Java, ma il `HttpHubConnectionBuilder` sottoclasse corrisponde a ciò che contiene le opzioni di configurazione generatore, anche nel `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="cd282-203">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="cd282-204">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="cd282-204">Configure logging</span></span>

<span data-ttu-id="cd282-205">La registrazione è configurata nel Client .NET usando il `ConfigureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="cd282-205">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="cd282-206">Registrazione provider e i filtri possono essere registrata nello stesso modo come sono nel server.</span><span class="sxs-lookup"><span data-stu-id="cd282-206">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="cd282-207">Vedere le [registrazione in ASP.NET Core](xref:fundamentals/logging/index) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="cd282-207">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="cd282-208">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="cd282-208">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="cd282-209">Vedere le [provider di registrazione predefiniti](xref:fundamentals/logging/index#built-in-logging-providers) sezione della documentazione per un elenco completo.</span><span class="sxs-lookup"><span data-stu-id="cd282-209">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="cd282-210">Ad esempio, per abilitare la registrazione della Console, installare il `Microsoft.Extensions.Logging.Console` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd282-210">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="cd282-211">Chiamare il `AddConsole` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="cd282-211">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="cd282-212">Nel client JavaScript, un simile `configureLogging` esiste alcun metodo.</span><span class="sxs-lookup"><span data-stu-id="cd282-212">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="cd282-213">Fornire un `LogLevel` valore che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="cd282-213">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="cd282-214">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="cd282-214">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cd282-215">Invece di un `LogLevel` valore, è anche possibile fornire un `string` valore che rappresenta un nome a livello di log.</span><span class="sxs-lookup"><span data-stu-id="cd282-215">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="cd282-216">Ciò è utile quando si configura la registrazione in ambienti di SignalR in cui non si ha accesso al `LogLevel` costanti.</span><span class="sxs-lookup"><span data-stu-id="cd282-216">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="cd282-217">Nella tabella seguente sono elencati i livelli di log disponibili.</span><span class="sxs-lookup"><span data-stu-id="cd282-217">The following table lists the available log levels.</span></span> <span data-ttu-id="cd282-218">Il valore specificato per `configureLogging` imposta la **minimo** livello che verrà registrato di registrazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-218">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="cd282-219">I messaggi registrati a questo livello **o i livelli elencati dopo di esso nella tabella**, verranno registrati.</span><span class="sxs-lookup"><span data-stu-id="cd282-219">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="cd282-220">string</span><span class="sxs-lookup"><span data-stu-id="cd282-220">string</span></span> | <span data-ttu-id="cd282-221">LogLevel</span><span class="sxs-lookup"><span data-stu-id="cd282-221">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="cd282-222">`"info"` **or** `"information"`</span><span class="sxs-lookup"><span data-stu-id="cd282-222">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="cd282-223">`"warn"` **or** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="cd282-223">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="cd282-224">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).</span><span class="sxs-lookup"><span data-stu-id="cd282-224">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="cd282-225">Per altre informazioni sulla registrazione, vedere la [documentazione di diagnostica di SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="cd282-225">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="cd282-226">Il client SignalR Java Usa il [SLF4J](https://www.slf4j.org/) libreria per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-226">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="cd282-227">È un'API di registrazione di alto livello che consente agli utenti della libreria scegliere la propria implementazione di registrazione specifici, grazie alla disponibilità di una dipendenza di registrazione specifici.</span><span class="sxs-lookup"><span data-stu-id="cd282-227">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="cd282-228">Il frammento di codice seguente viene illustrato come utilizzare `java.util.logging` con il client Java di SignalR.</span><span class="sxs-lookup"><span data-stu-id="cd282-228">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="cd282-229">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger di alcuna operazione predefinita con il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="cd282-229">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="cd282-230">Ciò può essere ignorato.</span><span class="sxs-lookup"><span data-stu-id="cd282-230">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="cd282-231">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="cd282-231">Configure allowed transports</span></span>

<span data-ttu-id="cd282-232">È possibile configurare i trasporti utilizzati da SignalR nel `WithUrl` chiamare (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd282-232">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="cd282-233">Un OR bit per bit dei valori di `HttpTransportType` può essere utilizzato per limitare il client per usare solo i tipi di trasporto specificati.</span><span class="sxs-lookup"><span data-stu-id="cd282-233">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="cd282-234">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cd282-234">All transports are enabled by default.</span></span>

<span data-ttu-id="cd282-235">Ad esempio, per disabilitare il trasporto di eventi Server-Sent, ma consentire le connessioni WebSocket e di Polling lungo:</span><span class="sxs-lookup"><span data-stu-id="cd282-235">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="cd282-236">Nel client JavaScript, i trasporti vengono configurati impostando il `transport` campo per l'oggetto di opzioni fornita a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd282-236">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="cd282-237">In questa versione di Java client WebSocket è il trasporto è disponibile solo.</span><span class="sxs-lookup"><span data-stu-id="cd282-237">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="cd282-238">Nel client Java, il trasporto è selezionato con il `withTransport` metodo di `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cd282-238">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="cd282-239">Impostazioni predefinite del client Java per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-239">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="cd282-240">Il client SignalR Java non supporta ancora trasporto fallback.</span><span class="sxs-lookup"><span data-stu-id="cd282-240">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="cd282-241">Configurare l'autenticazione della connessione</span><span class="sxs-lookup"><span data-stu-id="cd282-241">Configure bearer authentication</span></span>

<span data-ttu-id="cd282-242">Per fornire i dati di autenticazione con le richieste di SignalR, usare il `AccessTokenProvider` opzione (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="cd282-242">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="cd282-243">Nel Client .NET, questo token di accesso viene passato come un HTTP "Autenticazione della connessione" token (tramite il `Authorization` intestazione con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="cd282-243">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="cd282-244">Nel client JavaScript, il token di accesso viene utilizzato come un token di connessione **eccetto** in alcuni casi in cui browser API limitare la possibilità di applicare le intestazioni (in particolare, in richieste Server-Sent eventi e WebSockets).</span><span class="sxs-lookup"><span data-stu-id="cd282-244">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="cd282-245">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="cd282-245">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="cd282-246">Nel client .NET, il `AccessTokenProvider` opzione può essere specificata tramite il delegato di opzioni in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd282-246">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="cd282-247">Nel client JavaScript, il token di accesso viene configurato impostando la `accessTokenFactory` campo per l'oggetto di opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd282-247">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="cd282-248">Nel client Java di SignalR, è possibile configurare un token di connessione da utilizzare per l'autenticazione, fornendo una factory di token di accesso per il [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="cd282-248">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="cd282-249">Uso [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire un' [RxJava](https://github.com/ReactiveX/RxJava) [singolo\<String >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="cd282-249">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="cd282-250">Con una chiamata a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="cd282-250">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="cd282-251">Configurare i timeout e opzioni keep-alive</span><span class="sxs-lookup"><span data-stu-id="cd282-251">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="cd282-252">Sono disponibili in opzioni aggiuntive per la configurazione di timeout e il comportamento keep-alive di `HubConnection` oggetto stesso:</span><span class="sxs-lookup"><span data-stu-id="cd282-252">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="cd282-253">.NET</span><span class="sxs-lookup"><span data-stu-id="cd282-253">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="cd282-254">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-254">Option</span></span> | <span data-ttu-id="cd282-255">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-255">Default value</span></span> | <span data-ttu-id="cd282-256">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-256">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="cd282-257">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="cd282-257">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="cd282-258">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-258">Timeout for server activity.</span></span> <span data-ttu-id="cd282-259">Se il server non ha inviato un messaggio in questo intervallo, il client considera il trigger server disconnessa la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd282-259">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="cd282-260">Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="cd282-260">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="cd282-261">Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo.</span><span class="sxs-lookup"><span data-stu-id="cd282-261">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="cd282-262">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-262">15 seconds</span></span> | <span data-ttu-id="cd282-263">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-263">Timeout for initial server handshake.</span></span> <span data-ttu-id="cd282-264">Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger la `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd282-264">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="cd282-265">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="cd282-265">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="cd282-266">Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="cd282-266">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="cd282-267">Nel Client .NET, i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="cd282-267">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd282-268">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd282-268">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="cd282-269">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-269">Option</span></span> | <span data-ttu-id="cd282-270">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-270">Default value</span></span> | <span data-ttu-id="cd282-271">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-271">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="cd282-272">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="cd282-272">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="cd282-273">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-273">Timeout for server activity.</span></span> <span data-ttu-id="cd282-274">Se il server non ha inviato un messaggio in questo intervallo, il client considera i server disconnesso e i trigger di `onclose` evento.</span><span class="sxs-lookup"><span data-stu-id="cd282-274">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="cd282-275">Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="cd282-275">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="cd282-276">Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo.</span><span class="sxs-lookup"><span data-stu-id="cd282-276">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="cd282-277">Java</span><span class="sxs-lookup"><span data-stu-id="cd282-277">Java</span></span>](#tab/java)

| <span data-ttu-id="cd282-278">Opzione</span><span class="sxs-lookup"><span data-stu-id="cd282-278">Option</span></span> | <span data-ttu-id="cd282-279">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-279">Default value</span></span> | <span data-ttu-id="cd282-280">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-280">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="cd282-281">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="cd282-281">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="cd282-282">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="cd282-282">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="cd282-283">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-283">Timeout for server activity.</span></span> <span data-ttu-id="cd282-284">Se il server non ha inviato un messaggio in questo intervallo, il client considera i server disconnesso e i trigger di `onClose` evento.</span><span class="sxs-lookup"><span data-stu-id="cd282-284">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="cd282-285">Questo valore deve essere sufficientemente grande essere inviati dal server di un messaggio ping **e** ricevuti dal client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="cd282-285">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="cd282-286">Il valore consigliato è un numero almeno il doppio server `KeepAliveInterval` valore, per consentire tempo perché ping in arrivo.</span><span class="sxs-lookup"><span data-stu-id="cd282-286">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="cd282-287">15 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-287">15 seconds</span></span> | <span data-ttu-id="cd282-288">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="cd282-288">Timeout for initial server handshake.</span></span> <span data-ttu-id="cd282-289">Se il server non invia una risposta di handshake in questo intervallo, il client viene annullata l'handshake e trigger di `onClose` evento.</span><span class="sxs-lookup"><span data-stu-id="cd282-289">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="cd282-290">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano a causa della latenza di rete gravi errori di timeout di handshake.</span><span class="sxs-lookup"><span data-stu-id="cd282-290">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="cd282-291">Per informazioni dettagliate sul processo di Handshake, vedere la [specifica del protocollo dell'Hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="cd282-291">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="cd282-292">Configurare le opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cd282-292">Configure additional options</span></span>

<span data-ttu-id="cd282-293">Opzioni aggiuntive possono essere configurate nel `WithUrl` (`withUrl` in JavaScript) metodo sul `HubConnectionBuilder` o le API di configurazione diversi sul `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="cd282-293">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="cd282-294">.NET</span><span class="sxs-lookup"><span data-stu-id="cd282-294">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="cd282-295">Opzione di .NET</span><span class="sxs-lookup"><span data-stu-id="cd282-295">.NET Option</span></span> |  <span data-ttu-id="cd282-296">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-296">Default value</span></span> | <span data-ttu-id="cd282-297">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-297">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="cd282-298">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-298">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="cd282-299">Impostare questa proprietà su `true` per ignorare la fase di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-299">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="cd282-300">**Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="cd282-300">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="cd282-301">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="cd282-301">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="cd282-302">Empty</span><span class="sxs-lookup"><span data-stu-id="cd282-302">Empty</span></span> | <span data-ttu-id="cd282-303">Raccolta di certificati TLS da inviare autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="cd282-303">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="cd282-304">Empty</span><span class="sxs-lookup"><span data-stu-id="cd282-304">Empty</span></span> | <span data-ttu-id="cd282-305">Una raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-305">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="cd282-306">Empty</span><span class="sxs-lookup"><span data-stu-id="cd282-306">Empty</span></span> | <span data-ttu-id="cd282-307">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-307">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="cd282-308">5 secondi</span><span class="sxs-lookup"><span data-stu-id="cd282-308">5 seconds</span></span> | <span data-ttu-id="cd282-309">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-309">WebSockets only.</span></span> <span data-ttu-id="cd282-310">La quantità massima di tempo è in attesa dopo la chiusura per il server confermare la richiesta di chiusura al client.</span><span class="sxs-lookup"><span data-stu-id="cd282-310">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="cd282-311">Se il server non riconosciuto alla chiusura entro questo intervallo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="cd282-311">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="cd282-312">Empty</span><span class="sxs-lookup"><span data-stu-id="cd282-312">Empty</span></span> | <span data-ttu-id="cd282-313">Mappa delle intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-313">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="cd282-314">Un delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` usato per inviare richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-314">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="cd282-315">Non è utilizzato per le connessioni di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-315">Not used for WebSocket connections.</span></span> <span data-ttu-id="cd282-316">Questo delegato deve restituire un valore diverso da null, e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="cd282-316">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="cd282-317">Modificare le impostazioni del valore predefinito e restituirlo o restituire un nuovo `HttpMessageHandler` istanza.</span><span class="sxs-lookup"><span data-stu-id="cd282-317">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="cd282-318">**Quando si sostituisce il gestore assicurarsi di copiare le impostazioni desiderate per impedire che il gestore fornito, in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non si applicano al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="cd282-318">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="cd282-319">Un proxy HTTP da usare quando si inviano richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-319">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="cd282-320">Impostare il valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-320">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="cd282-321">In questo modo l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="cd282-321">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="cd282-322">Delegato che può essere utilizzato per configurare le opzioni aggiuntive di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd282-322">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="cd282-323">Riceve un'istanza di [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzato per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="cd282-323">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd282-324">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd282-324">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="cd282-325">Opzione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd282-325">JavaScript Option</span></span> | <span data-ttu-id="cd282-326">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-326">Default Value</span></span> | <span data-ttu-id="cd282-327">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-327">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="cd282-328">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-328">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="cd282-329">Impostare questa proprietà su `true` per ignorare la fase di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-329">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="cd282-330">**Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="cd282-330">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="cd282-331">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="cd282-331">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="cd282-332">Java</span><span class="sxs-lookup"><span data-stu-id="cd282-332">Java</span></span>](#tab/java)

| <span data-ttu-id="cd282-333">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="cd282-333">Java Option</span></span> | <span data-ttu-id="cd282-334">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="cd282-334">Default Value</span></span> | <span data-ttu-id="cd282-335">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cd282-335">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="cd282-336">Una funzione che restituisce una stringa che viene fornita come un token di autenticazione nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-336">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="cd282-337">Impostare questa proprietà su `true` per ignorare la fase di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="cd282-337">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="cd282-338">**Supportato solo quando il trasporto abilitato solo il trasporto WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="cd282-338">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="cd282-339">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="cd282-339">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="cd282-340">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="cd282-340">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="cd282-341">Empty</span><span class="sxs-lookup"><span data-stu-id="cd282-341">Empty</span></span> | <span data-ttu-id="cd282-342">Mappa delle intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd282-342">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="cd282-343">Nel Client .NET, queste opzioni possono essere modificate dal delegato opzioni fornito a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd282-343">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="cd282-344">Nel JavaScript Client, queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd282-344">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="cd282-345">Nel client Java, queste opzioni possono essere configurate con i metodi su di `HttpHubConnectionBuilder` restituito dal `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="cd282-345">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="cd282-346">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cd282-346">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
