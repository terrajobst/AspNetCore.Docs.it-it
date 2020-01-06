---
title: Configurazione SignalR ASP.NET Core
author: bradygaster
description: Informazioni su come configurare ASP.NET Core SignalR app.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358112"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="6624b-103">Configurazione SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6624b-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6624b-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6624b-105">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6624b-106">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="6624b-107">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="6624b-108">è possibile aggiungere `AddJsonProtocol` dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6624b-109">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="6624b-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6624b-110">La proprietà [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) su tale oggetto è un oggetto `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="6624b-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6624b-111">Per ulteriori informazioni, vedere la [documentazione di System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="6624b-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="6624b-112">Ad esempio, per configurare il serializzatore in modo da non modificare la combinazione di maiuscole e minuscole dei nomi di proprietà, anziché i nomi predefiniti "camelCase", usare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6624b-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="6624b-113">Nel client .NET è presente lo stesso `AddJsonProtocol` metodo di estensione in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6624b-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6624b-114">È necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection` per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6624b-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="6624b-115">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="6624b-116">Passa a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="6624b-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="6624b-117">Se sono necessarie funzionalità di `Newtonsoft.Json` che non sono supportate in `System.Text.Json`, vedere [passare a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="6624b-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6624b-118">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-118">MessagePack serialization options</span></span>

<span data-ttu-id="6624b-119">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6624b-120">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-121">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6624b-122">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="6624b-122">Configure server options</span></span>

<span data-ttu-id="6624b-123">La tabella seguente descrive le opzioni per la configurazione di hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="6624b-124">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-124">Option</span></span> | <span data-ttu-id="6624b-125">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-125">Default Value</span></span> | <span data-ttu-id="6624b-126">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6624b-127">30 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-127">30 seconds</span></span> | <span data-ttu-id="6624b-128">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6624b-129">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6624b-130">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6624b-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6624b-131">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-131">15 seconds</span></span> | <span data-ttu-id="6624b-132">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6624b-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6624b-133">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-134">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6624b-135">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-135">15 seconds</span></span> | <span data-ttu-id="6624b-136">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6624b-137">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="6624b-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6624b-138">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6624b-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6624b-139">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6624b-139">All installed protocols</span></span> | <span data-ttu-id="6624b-140">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-140">Protocols supported by this hub.</span></span> <span data-ttu-id="6624b-141">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6624b-142">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6624b-143">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6624b-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="6624b-144">Numero massimo di elementi che possono essere memorizzati nel buffer per i flussi di caricamento client.</span><span class="sxs-lookup"><span data-stu-id="6624b-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="6624b-145">Se viene raggiunto questo limite, l'elaborazione delle chiamate viene bloccata fino a quando il server non elabora gli elementi del flusso.</span><span class="sxs-lookup"><span data-stu-id="6624b-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="6624b-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-146">32 KB</span></span> | <span data-ttu-id="6624b-147">Dimensione massima di un singolo messaggio dell'hub in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6624b-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="6624b-148">È possibile configurare le opzioni per tutti gli hub fornendo le opzioni delegate alla chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6624b-149">Le opzioni per un singolo Hub eseguono l'override delle opzioni globali fornite in `AddSignalR` e possono essere configurate utilizzando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="6624b-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6624b-150">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="6624b-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="6624b-151">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6624b-152">Queste opzioni vengono configurate passando un delegato a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6624b-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="6624b-153">Nella tabella seguente vengono descritte le opzioni per la configurazione di ASP.NET Core opzioni HTTP avanzate di SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="6624b-154">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-154">Option</span></span> | <span data-ttu-id="6624b-155">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-155">Default Value</span></span> | <span data-ttu-id="6624b-156">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6624b-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-157">32 KB</span></span> | <span data-ttu-id="6624b-158">Il numero massimo di byte ricevuti dal client che il server memorizza nel buffer prima di applicare la contropressione.</span><span class="sxs-lookup"><span data-stu-id="6624b-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="6624b-159">L'aumento di questo valore consente al server di ricevere più rapidamente messaggi di dimensioni maggiori senza applicare la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6624b-160">Dati raccolti automaticamente da `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6624b-161">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6624b-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-162">32 KB</span></span> | <span data-ttu-id="6624b-163">Numero massimo di byte inviati dall'app che il server memorizza nel buffer prima di osservare la sovrapressione.</span><span class="sxs-lookup"><span data-stu-id="6624b-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="6624b-164">L'aumento di questo valore consente al server di memorizzare nel buffer i messaggi di dimensioni maggiori più rapidamente senza attendere la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6624b-165">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="6624b-165">All Transports are enabled.</span></span> | <span data-ttu-id="6624b-166">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6624b-167">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-167">See below.</span></span> | <span data-ttu-id="6624b-168">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="6624b-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6624b-169">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-169">See below.</span></span> | <span data-ttu-id="6624b-170">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="6624b-171">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate utilizzando la proprietà `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="6624b-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6624b-172">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-172">Option</span></span> | <span data-ttu-id="6624b-173">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-173">Default Value</span></span> | <span data-ttu-id="6624b-174">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6624b-175">90 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-175">90 seconds</span></span> | <span data-ttu-id="6624b-176">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6624b-177">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6624b-178">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate usando la proprietà `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="6624b-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6624b-179">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-179">Option</span></span> | <span data-ttu-id="6624b-180">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-180">Default Value</span></span> | <span data-ttu-id="6624b-181">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6624b-182">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-182">5 seconds</span></span> | <span data-ttu-id="6624b-183">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="6624b-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6624b-184">Delegato che può essere utilizzato per impostare l'intestazione `Sec-WebSocket-Protocol` su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="6624b-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6624b-185">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6624b-186">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="6624b-186">Configure client options</span></span>

<span data-ttu-id="6624b-187">Le opzioni client possono essere configurate nel tipo di `HubConnectionBuilder`, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6624b-188">È anche disponibile nel client Java, ma la sottoclasse `HttpHubConnectionBuilder` contiene le opzioni di configurazione del compilatore, oltre al `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6624b-189">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="6624b-189">Configure logging</span></span>

<span data-ttu-id="6624b-190">La registrazione viene configurata nel client .NET utilizzando il metodo `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6624b-191">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="6624b-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6624b-192">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="6624b-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-193">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="6624b-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6624b-194">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="6624b-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6624b-195">Ad esempio, per abilitare la registrazione della console, installare il pacchetto NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="6624b-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6624b-196">Chiamare il metodo di estensione `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="6624b-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6624b-197">Nel client JavaScript esiste un metodo di `configureLogging` simile.</span><span class="sxs-lookup"><span data-stu-id="6624b-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6624b-198">Consente di specificare un valore `LogLevel` che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="6624b-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6624b-199">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="6624b-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="6624b-200">Anziché un valore di `LogLevel`, è anche possibile fornire un valore `string` che rappresenta il nome di un livello di log.</span><span class="sxs-lookup"><span data-stu-id="6624b-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="6624b-201">Questa operazione è utile quando si configura SignalR la registrazione in ambienti in cui non si ha accesso alle costanti `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="6624b-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="6624b-202">Nella tabella seguente sono elencati i livelli di log disponibili.</span><span class="sxs-lookup"><span data-stu-id="6624b-202">The following table lists the available log levels.</span></span> <span data-ttu-id="6624b-203">Il valore fornito per `configureLogging` imposta il livello di registrazione **minimo** che verrà registrato.</span><span class="sxs-lookup"><span data-stu-id="6624b-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="6624b-204">Verranno registrati i messaggi registrati a questo livello **o i livelli elencati dopo tale operazione nella tabella**.</span><span class="sxs-lookup"><span data-stu-id="6624b-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="6624b-205">Stringa</span><span class="sxs-lookup"><span data-stu-id="6624b-205">String</span></span>                      | <span data-ttu-id="6624b-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="6624b-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="6624b-207">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="6624b-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="6624b-208">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="6624b-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="6624b-209">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nel metodo `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6624b-210">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnosticaSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6624b-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6624b-211">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6624b-212">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="6624b-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6624b-213">Il frammento di codice seguente illustra come usare `java.util.logging` con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="6624b-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6624b-214">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="6624b-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6624b-215">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="6624b-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6624b-216">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="6624b-216">Configure allowed transports</span></span>

<span data-ttu-id="6624b-217">I trasporti utilizzati da SignalR possono essere configurati nella chiamata `WithUrl` (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6624b-218">È possibile utilizzare un operatore OR bit per bit dei valori di `HttpTransportType` per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="6624b-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6624b-219">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6624b-219">All transports are enabled by default.</span></span>

<span data-ttu-id="6624b-220">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="6624b-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6624b-221">Nel client JavaScript, i trasporti vengono configurati impostando il campo `transport` nell'oggetto options fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="6624b-222">In questa versione dei WebSocket client Java è l'unico trasporto disponibile.</span><span class="sxs-lookup"><span data-stu-id="6624b-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="6624b-223">Nel client Java il trasporto viene selezionato con il metodo `withTransport` nel `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6624b-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="6624b-224">Per impostazione predefinita, il client Java usa il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6624b-225">Il client Java SignalR non supporta ancora il fallback del trasporto.</span><span class="sxs-lookup"><span data-stu-id="6624b-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6624b-226">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="6624b-226">Configure bearer authentication</span></span>

<span data-ttu-id="6624b-227">Per fornire i dati di autenticazione insieme alle richieste di SignalR, usare l'opzione `AccessTokenProvider` (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6624b-228">Nel client .NET questo token di accesso viene passato come token "autenticazione di connessione" HTTP (usando l'intestazione di `Authorization` con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="6624b-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6624b-229">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="6624b-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6624b-230">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="6624b-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6624b-231">Nel client .NET è possibile specificare l'opzione `AccessTokenProvider` usando il delegato options in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6624b-232">Nel client JavaScript, il token di accesso viene configurato impostando il campo `accessTokenFactory` nell'oggetto opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6624b-233">Nel client SignalR Java è possibile configurare una bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6624b-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6624b-234">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire una [RxJava](https://github.com/ReactiveX/RxJava) [stringa a\<> singola](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6624b-235">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="6624b-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6624b-236">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="6624b-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6624b-237">Opzioni aggiuntive per la configurazione del comportamento di timeout e Keep-Alive sono disponibili nell'oggetto `HubConnection` stesso:</span><span class="sxs-lookup"><span data-stu-id="6624b-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-238">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-239">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-239">Option</span></span> | <span data-ttu-id="6624b-240">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-240">Default value</span></span> | <span data-ttu-id="6624b-241">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6624b-242">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-243">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-243">Timeout for server activity.</span></span> <span data-ttu-id="6624b-244">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-245">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-246">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6624b-247">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-247">15 seconds</span></span> | <span data-ttu-id="6624b-248">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-249">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-250">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-251">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6624b-252">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-252">15 seconds</span></span> | <span data-ttu-id="6624b-253">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-254">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-255">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="6624b-256">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6624b-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-258">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-258">Option</span></span> | <span data-ttu-id="6624b-259">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-259">Default value</span></span> | <span data-ttu-id="6624b-260">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6624b-261">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-262">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-262">Timeout for server activity.</span></span> <span data-ttu-id="6624b-263">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6624b-264">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-265">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="6624b-266">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6624b-267">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-268">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-269">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-270">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-270">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-271">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-271">Option</span></span> | <span data-ttu-id="6624b-272">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-272">Default value</span></span> | <span data-ttu-id="6624b-273">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6624b-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6624b-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6624b-275">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-276">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-276">Timeout for server activity.</span></span> <span data-ttu-id="6624b-277">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-278">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-279">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6624b-280">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-280">15 seconds</span></span> | <span data-ttu-id="6624b-281">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-282">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-283">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-284">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="6624b-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6624b-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="6624b-286">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6624b-287">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-288">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-289">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="6624b-290">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-290">Configure additional options</span></span>

<span data-ttu-id="6624b-291">È possibile configurare opzioni aggiuntive nel metodo `WithUrl` (`withUrl` in JavaScript) in `HubConnectionBuilder` o nelle varie API di configurazione del `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="6624b-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-292">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-293">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="6624b-293">.NET Option</span></span> |  <span data-ttu-id="6624b-294">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-294">Default value</span></span> | <span data-ttu-id="6624b-295">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6624b-296">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6624b-297">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-298">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-299">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6624b-300">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-300">Empty</span></span> | <span data-ttu-id="6624b-301">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="6624b-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6624b-302">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-302">Empty</span></span> | <span data-ttu-id="6624b-303">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6624b-304">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-304">Empty</span></span> | <span data-ttu-id="6624b-305">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6624b-306">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-306">5 seconds</span></span> | <span data-ttu-id="6624b-307">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-307">WebSockets only.</span></span> <span data-ttu-id="6624b-308">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="6624b-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6624b-309">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6624b-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6624b-310">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-310">Empty</span></span> | <span data-ttu-id="6624b-311">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6624b-312">Delegato che può essere utilizzato per configurare o sostituire la `HttpMessageHandler` utilizzata per inviare le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6624b-313">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="6624b-314">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="6624b-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6624b-315">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova istanza di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="6624b-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6624b-316">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="6624b-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6624b-317">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6624b-318">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6624b-319">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6624b-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6624b-320">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="6624b-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6624b-321">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="6624b-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-323">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-323">JavaScript Option</span></span> | <span data-ttu-id="6624b-324">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-324">Default Value</span></span> | <span data-ttu-id="6624b-325">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6624b-326">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6624b-327">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-328">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-329">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-330">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-330">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-331">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="6624b-331">Java Option</span></span> | <span data-ttu-id="6624b-332">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-332">Default Value</span></span> | <span data-ttu-id="6624b-333">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6624b-334">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6624b-335">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-336">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-337">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6624b-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6624b-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6624b-339">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-339">Empty</span></span> | <span data-ttu-id="6624b-340">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6624b-341">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito per `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6624b-342">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6624b-343">Nel client Java queste opzioni possono essere configurate con i metodi nella `HttpHubConnectionBuilder` restituita dalla `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6624b-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6624b-344">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6624b-345">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6624b-346">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6624b-347">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="6624b-348">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6624b-349">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="6624b-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6624b-350">La proprietà [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) su tale oggetto è un oggetto `JsonSerializerSettings` JSON.NET che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="6624b-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6624b-351">Per ulteriori informazioni, vedere la [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="6624b-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="6624b-352">Per configurare, ad esempio, il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi predefiniti "camelCase", utilizzare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6624b-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="6624b-353">Nel client .NET è presente lo stesso `AddJsonProtocol` metodo di estensione in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6624b-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6624b-354">È necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection` per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6624b-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="6624b-355">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6624b-356">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-356">MessagePack serialization options</span></span>

<span data-ttu-id="6624b-357">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6624b-358">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-359">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6624b-360">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="6624b-360">Configure server options</span></span>

<span data-ttu-id="6624b-361">La tabella seguente descrive le opzioni per la configurazione di hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="6624b-362">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-362">Option</span></span> | <span data-ttu-id="6624b-363">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-363">Default Value</span></span> | <span data-ttu-id="6624b-364">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6624b-365">30 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-365">30 seconds</span></span> | <span data-ttu-id="6624b-366">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6624b-367">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6624b-368">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6624b-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6624b-369">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-369">15 seconds</span></span> | <span data-ttu-id="6624b-370">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6624b-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6624b-371">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-372">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6624b-373">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-373">15 seconds</span></span> | <span data-ttu-id="6624b-374">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6624b-375">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="6624b-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6624b-376">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6624b-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6624b-377">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6624b-377">All installed protocols</span></span> | <span data-ttu-id="6624b-378">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-378">Protocols supported by this hub.</span></span> <span data-ttu-id="6624b-379">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6624b-380">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6624b-381">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6624b-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="6624b-382">È possibile configurare le opzioni per tutti gli hub fornendo le opzioni delegate alla chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6624b-383">Le opzioni per un singolo Hub eseguono l'override delle opzioni globali fornite in `AddSignalR` e possono essere configurate utilizzando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="6624b-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6624b-384">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="6624b-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="6624b-385">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6624b-386">Queste opzioni vengono configurate passando un delegato a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6624b-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="6624b-387">Nella tabella seguente vengono descritte le opzioni per la configurazione di ASP.NET Core opzioni HTTP avanzate di SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="6624b-388">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-388">Option</span></span> | <span data-ttu-id="6624b-389">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-389">Default Value</span></span> | <span data-ttu-id="6624b-390">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6624b-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-391">32 KB</span></span> | <span data-ttu-id="6624b-392">Numero massimo di byte ricevuti dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6624b-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="6624b-393">L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6624b-394">Dati raccolti automaticamente da `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6624b-395">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6624b-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-396">32 KB</span></span> | <span data-ttu-id="6624b-397">Numero massimo di byte inviati dall'app che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6624b-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="6624b-398">L'aumento di questo valore consente al server di inviare messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6624b-399">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="6624b-399">All Transports are enabled.</span></span> | <span data-ttu-id="6624b-400">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6624b-401">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-401">See below.</span></span> | <span data-ttu-id="6624b-402">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="6624b-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6624b-403">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-403">See below.</span></span> | <span data-ttu-id="6624b-404">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="6624b-405">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate utilizzando la proprietà `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="6624b-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6624b-406">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-406">Option</span></span> | <span data-ttu-id="6624b-407">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-407">Default Value</span></span> | <span data-ttu-id="6624b-408">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6624b-409">90 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-409">90 seconds</span></span> | <span data-ttu-id="6624b-410">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6624b-411">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6624b-412">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate usando la proprietà `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="6624b-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6624b-413">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-413">Option</span></span> | <span data-ttu-id="6624b-414">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-414">Default Value</span></span> | <span data-ttu-id="6624b-415">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6624b-416">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-416">5 seconds</span></span> | <span data-ttu-id="6624b-417">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="6624b-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6624b-418">Delegato che può essere utilizzato per impostare l'intestazione `Sec-WebSocket-Protocol` su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="6624b-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6624b-419">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6624b-420">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="6624b-420">Configure client options</span></span>

<span data-ttu-id="6624b-421">Le opzioni client possono essere configurate nel tipo di `HubConnectionBuilder`, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6624b-422">È anche disponibile nel client Java, ma la sottoclasse `HttpHubConnectionBuilder` contiene le opzioni di configurazione del compilatore, oltre al `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6624b-423">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="6624b-423">Configure logging</span></span>

<span data-ttu-id="6624b-424">La registrazione viene configurata nel client .NET utilizzando il metodo `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6624b-425">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="6624b-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6624b-426">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="6624b-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-427">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="6624b-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6624b-428">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="6624b-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6624b-429">Ad esempio, per abilitare la registrazione della console, installare il pacchetto NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="6624b-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6624b-430">Chiamare il metodo di estensione `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="6624b-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6624b-431">Nel client JavaScript esiste un metodo di `configureLogging` simile.</span><span class="sxs-lookup"><span data-stu-id="6624b-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6624b-432">Consente di specificare un valore `LogLevel` che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="6624b-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6624b-433">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="6624b-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6624b-434">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nel metodo `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6624b-435">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnosticaSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6624b-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6624b-436">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6624b-437">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="6624b-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6624b-438">Il frammento di codice seguente illustra come usare `java.util.logging` con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="6624b-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6624b-439">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="6624b-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6624b-440">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="6624b-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6624b-441">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="6624b-441">Configure allowed transports</span></span>

<span data-ttu-id="6624b-442">I trasporti utilizzati da SignalR possono essere configurati nella chiamata `WithUrl` (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6624b-443">È possibile utilizzare un operatore OR bit per bit dei valori di `HttpTransportType` per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="6624b-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6624b-444">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6624b-444">All transports are enabled by default.</span></span>

<span data-ttu-id="6624b-445">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="6624b-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6624b-446">Nel client JavaScript, i trasporti vengono configurati impostando il campo `transport` nell'oggetto options fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="6624b-447">In questa versione dei WebSocket client Java è l'unico trasporto disponibile.</span><span class="sxs-lookup"><span data-stu-id="6624b-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6624b-448">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="6624b-448">Configure bearer authentication</span></span>

<span data-ttu-id="6624b-449">Per fornire i dati di autenticazione insieme alle richieste di SignalR, usare l'opzione `AccessTokenProvider` (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6624b-450">Nel client .NET questo token di accesso viene passato come token "autenticazione di connessione" HTTP (usando l'intestazione di `Authorization` con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="6624b-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6624b-451">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="6624b-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6624b-452">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="6624b-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6624b-453">Nel client .NET è possibile specificare l'opzione `AccessTokenProvider` usando il delegato options in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6624b-454">Nel client JavaScript, il token di accesso viene configurato impostando il campo `accessTokenFactory` nell'oggetto opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6624b-455">Nel client SignalR Java è possibile configurare una bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6624b-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6624b-456">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire una [RxJava](https://github.com/ReactiveX/RxJava) [stringa a\<> singola](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6624b-457">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="6624b-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6624b-458">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="6624b-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6624b-459">Opzioni aggiuntive per la configurazione del comportamento di timeout e Keep-Alive sono disponibili nell'oggetto `HubConnection` stesso:</span><span class="sxs-lookup"><span data-stu-id="6624b-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-460">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-461">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-461">Option</span></span> | <span data-ttu-id="6624b-462">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-462">Default value</span></span> | <span data-ttu-id="6624b-463">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6624b-464">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-465">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-465">Timeout for server activity.</span></span> <span data-ttu-id="6624b-466">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-467">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-468">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6624b-469">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-469">15 seconds</span></span> | <span data-ttu-id="6624b-470">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-471">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-472">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-473">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6624b-474">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-474">15 seconds</span></span> | <span data-ttu-id="6624b-475">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-476">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-477">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="6624b-478">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6624b-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-480">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-480">Option</span></span> | <span data-ttu-id="6624b-481">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-481">Default value</span></span> | <span data-ttu-id="6624b-482">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6624b-483">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-484">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-484">Timeout for server activity.</span></span> <span data-ttu-id="6624b-485">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6624b-486">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-487">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="6624b-488">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6624b-489">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-490">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-491">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-492">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-492">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-493">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-493">Option</span></span> | <span data-ttu-id="6624b-494">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-494">Default value</span></span> | <span data-ttu-id="6624b-495">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6624b-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6624b-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6624b-497">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-498">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-498">Timeout for server activity.</span></span> <span data-ttu-id="6624b-499">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-500">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-501">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6624b-502">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-502">15 seconds</span></span> | <span data-ttu-id="6624b-503">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-504">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-505">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-506">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="6624b-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6624b-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="6624b-508">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6624b-509">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6624b-510">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6624b-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6624b-511">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="6624b-512">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-512">Configure additional options</span></span>

<span data-ttu-id="6624b-513">È possibile configurare opzioni aggiuntive nel metodo `WithUrl` (`withUrl` in JavaScript) in `HubConnectionBuilder` o nelle varie API di configurazione del `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="6624b-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-514">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-515">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="6624b-515">.NET Option</span></span> |  <span data-ttu-id="6624b-516">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-516">Default value</span></span> | <span data-ttu-id="6624b-517">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6624b-518">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6624b-519">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-520">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-521">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6624b-522">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-522">Empty</span></span> | <span data-ttu-id="6624b-523">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="6624b-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6624b-524">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-524">Empty</span></span> | <span data-ttu-id="6624b-525">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6624b-526">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-526">Empty</span></span> | <span data-ttu-id="6624b-527">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6624b-528">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-528">5 seconds</span></span> | <span data-ttu-id="6624b-529">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-529">WebSockets only.</span></span> <span data-ttu-id="6624b-530">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="6624b-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6624b-531">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6624b-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6624b-532">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-532">Empty</span></span> | <span data-ttu-id="6624b-533">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6624b-534">Delegato che può essere utilizzato per configurare o sostituire la `HttpMessageHandler` utilizzata per inviare le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6624b-535">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="6624b-536">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="6624b-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6624b-537">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova istanza di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="6624b-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6624b-538">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="6624b-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6624b-539">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6624b-540">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6624b-541">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6624b-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6624b-542">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="6624b-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6624b-543">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="6624b-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-545">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-545">JavaScript Option</span></span> | <span data-ttu-id="6624b-546">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-546">Default Value</span></span> | <span data-ttu-id="6624b-547">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6624b-548">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6624b-549">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-550">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-551">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-552">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-552">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-553">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="6624b-553">Java Option</span></span> | <span data-ttu-id="6624b-554">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-554">Default Value</span></span> | <span data-ttu-id="6624b-555">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6624b-556">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6624b-557">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-558">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-559">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6624b-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6624b-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6624b-561">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-561">Empty</span></span> | <span data-ttu-id="6624b-562">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6624b-563">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito per `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6624b-564">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6624b-565">Nel client Java queste opzioni possono essere configurate con i metodi nella `HttpHubConnectionBuilder` restituita dalla `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6624b-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6624b-566">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6624b-567">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6624b-568">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6624b-569">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="6624b-570">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6624b-571">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="6624b-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6624b-572">La proprietà [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) su tale oggetto è un oggetto `JsonSerializerSettings` JSON.NET che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="6624b-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6624b-573">Per ulteriori informazioni, vedere la [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="6624b-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="6624b-574">Per configurare, ad esempio, il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi predefiniti "camelCase", utilizzare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6624b-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="6624b-575">Nel client .NET è presente lo stesso `AddJsonProtocol` metodo di estensione in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6624b-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6624b-576">È necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection` per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6624b-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="6624b-577">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6624b-578">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="6624b-578">MessagePack serialization options</span></span>

<span data-ttu-id="6624b-579">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6624b-580">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6624b-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-581">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6624b-582">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="6624b-582">Configure server options</span></span>

<span data-ttu-id="6624b-583">La tabella seguente descrive le opzioni per la configurazione di hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="6624b-584">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-584">Option</span></span> | <span data-ttu-id="6624b-585">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-585">Default Value</span></span> | <span data-ttu-id="6624b-586">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="6624b-587">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-587">15 seconds</span></span> | <span data-ttu-id="6624b-588">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6624b-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6624b-589">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-590">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6624b-591">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-591">15 seconds</span></span> | <span data-ttu-id="6624b-592">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6624b-593">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="6624b-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6624b-594">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6624b-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6624b-595">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6624b-595">All installed protocols</span></span> | <span data-ttu-id="6624b-596">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-596">Protocols supported by this hub.</span></span> <span data-ttu-id="6624b-597">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6624b-598">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6624b-599">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6624b-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="6624b-600">È possibile configurare le opzioni per tutti gli hub fornendo le opzioni delegate alla chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6624b-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6624b-601">Le opzioni per un singolo Hub eseguono l'override delle opzioni globali fornite in `AddSignalR` e possono essere configurate utilizzando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="6624b-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6624b-602">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="6624b-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="6624b-603">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6624b-604">Queste opzioni vengono configurate passando un delegato a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6624b-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="6624b-605">Nella tabella seguente vengono descritte le opzioni per la configurazione di ASP.NET Core opzioni HTTP avanzate di SignalR:</span><span class="sxs-lookup"><span data-stu-id="6624b-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="6624b-606">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-606">Option</span></span> | <span data-ttu-id="6624b-607">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-607">Default Value</span></span> | <span data-ttu-id="6624b-608">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6624b-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-609">32 KB</span></span> | <span data-ttu-id="6624b-610">Numero massimo di byte ricevuti dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6624b-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="6624b-611">L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6624b-612">Dati raccolti automaticamente da `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6624b-613">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6624b-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6624b-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="6624b-614">32 KB</span></span> | <span data-ttu-id="6624b-615">Numero massimo di byte inviati dall'app che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6624b-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="6624b-616">L'aumento di questo valore consente al server di inviare messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6624b-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6624b-617">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="6624b-617">All Transports are enabled.</span></span> | <span data-ttu-id="6624b-618">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6624b-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6624b-619">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-619">See below.</span></span> | <span data-ttu-id="6624b-620">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="6624b-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6624b-621">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6624b-621">See below.</span></span> | <span data-ttu-id="6624b-622">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="6624b-623">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate utilizzando la proprietà `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="6624b-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6624b-624">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-624">Option</span></span> | <span data-ttu-id="6624b-625">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-625">Default Value</span></span> | <span data-ttu-id="6624b-626">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6624b-627">90 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-627">90 seconds</span></span> | <span data-ttu-id="6624b-628">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6624b-629">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="6624b-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6624b-630">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate usando la proprietà `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="6624b-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6624b-631">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-631">Option</span></span> | <span data-ttu-id="6624b-632">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-632">Default Value</span></span> | <span data-ttu-id="6624b-633">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6624b-634">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-634">5 seconds</span></span> | <span data-ttu-id="6624b-635">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="6624b-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6624b-636">Delegato che può essere utilizzato per impostare l'intestazione `Sec-WebSocket-Protocol` su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="6624b-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6624b-637">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6624b-638">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="6624b-638">Configure client options</span></span>

<span data-ttu-id="6624b-639">Le opzioni client possono essere configurate nel tipo di `HubConnectionBuilder`, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6624b-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6624b-640">È anche disponibile nel client Java, ma la sottoclasse `HttpHubConnectionBuilder` contiene le opzioni di configurazione del compilatore, oltre al `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="6624b-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6624b-641">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="6624b-641">Configure logging</span></span>

<span data-ttu-id="6624b-642">La registrazione viene configurata nel client .NET utilizzando il metodo `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6624b-643">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="6624b-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6624b-644">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="6624b-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6624b-645">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="6624b-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6624b-646">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="6624b-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6624b-647">Ad esempio, per abilitare la registrazione della console, installare il pacchetto NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="6624b-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6624b-648">Chiamare il metodo di estensione `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="6624b-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6624b-649">Nel client JavaScript esiste un metodo di `configureLogging` simile.</span><span class="sxs-lookup"><span data-stu-id="6624b-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6624b-650">Consente di specificare un valore `LogLevel` che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="6624b-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6624b-651">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="6624b-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6624b-652">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nel metodo `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6624b-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6624b-653">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnosticaSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6624b-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6624b-654">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6624b-655">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="6624b-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6624b-656">Il frammento di codice seguente illustra come usare `java.util.logging` con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="6624b-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6624b-657">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="6624b-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6624b-658">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="6624b-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6624b-659">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="6624b-659">Configure allowed transports</span></span>

<span data-ttu-id="6624b-660">I trasporti utilizzati da SignalR possono essere configurati nella chiamata `WithUrl` (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6624b-661">È possibile utilizzare un operatore OR bit per bit dei valori di `HttpTransportType` per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="6624b-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6624b-662">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6624b-662">All transports are enabled by default.</span></span>

<span data-ttu-id="6624b-663">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="6624b-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6624b-664">Nel client JavaScript, i trasporti vengono configurati impostando il campo `transport` nell'oggetto options fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6624b-665">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="6624b-665">Configure bearer authentication</span></span>

<span data-ttu-id="6624b-666">Per fornire i dati di autenticazione insieme alle richieste di SignalR, usare l'opzione `AccessTokenProvider` (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="6624b-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6624b-667">Nel client .NET questo token di accesso viene passato come token "autenticazione di connessione" HTTP (usando l'intestazione di `Authorization` con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="6624b-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6624b-668">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="6624b-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6624b-669">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="6624b-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6624b-670">Nel client .NET è possibile specificare l'opzione `AccessTokenProvider` usando il delegato options in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6624b-671">Nel client JavaScript, il token di accesso viene configurato impostando il campo `accessTokenFactory` nell'oggetto opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6624b-672">Nel client SignalR Java è possibile configurare una bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6624b-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6624b-673">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire una [RxJava](https://github.com/ReactiveX/RxJava) [stringa a\<> singola](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6624b-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6624b-674">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="6624b-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6624b-675">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="6624b-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6624b-676">Opzioni aggiuntive per la configurazione del comportamento di timeout e Keep-Alive sono disponibili nell'oggetto `HubConnection` stesso:</span><span class="sxs-lookup"><span data-stu-id="6624b-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-677">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-678">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-678">Option</span></span> | <span data-ttu-id="6624b-679">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-679">Default value</span></span> | <span data-ttu-id="6624b-680">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6624b-681">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-682">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-682">Timeout for server activity.</span></span> <span data-ttu-id="6624b-683">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-684">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-685">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6624b-686">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-686">15 seconds</span></span> | <span data-ttu-id="6624b-687">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-688">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6624b-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6624b-689">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-690">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="6624b-691">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6624b-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-693">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-693">Option</span></span> | <span data-ttu-id="6624b-694">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-694">Default value</span></span> | <span data-ttu-id="6624b-695">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6624b-696">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-697">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-697">Timeout for server activity.</span></span> <span data-ttu-id="6624b-698">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6624b-699">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-700">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-701">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-701">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-702">Opzione</span><span class="sxs-lookup"><span data-stu-id="6624b-702">Option</span></span> | <span data-ttu-id="6624b-703">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-703">Default value</span></span> | <span data-ttu-id="6624b-704">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6624b-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6624b-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6624b-706">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6624b-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6624b-707">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-707">Timeout for server activity.</span></span> <span data-ttu-id="6624b-708">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-709">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6624b-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6624b-710">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server, per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6624b-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6624b-711">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-711">15 seconds</span></span> | <span data-ttu-id="6624b-712">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6624b-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="6624b-713">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6624b-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6624b-714">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6624b-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6624b-715">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6624b-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="6624b-716">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-716">Configure additional options</span></span>

<span data-ttu-id="6624b-717">È possibile configurare opzioni aggiuntive nel metodo `WithUrl` (`withUrl` in JavaScript) in `HubConnectionBuilder` o nelle varie API di configurazione del `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="6624b-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6624b-718">.NET</span><span class="sxs-lookup"><span data-stu-id="6624b-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6624b-719">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="6624b-719">.NET Option</span></span> |  <span data-ttu-id="6624b-720">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-720">Default value</span></span> | <span data-ttu-id="6624b-721">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6624b-722">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6624b-723">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-724">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-725">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6624b-726">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-726">Empty</span></span> | <span data-ttu-id="6624b-727">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="6624b-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6624b-728">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-728">Empty</span></span> | <span data-ttu-id="6624b-729">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6624b-730">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-730">Empty</span></span> | <span data-ttu-id="6624b-731">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6624b-732">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6624b-732">5 seconds</span></span> | <span data-ttu-id="6624b-733">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-733">WebSockets only.</span></span> <span data-ttu-id="6624b-734">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="6624b-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6624b-735">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6624b-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6624b-736">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-736">Empty</span></span> | <span data-ttu-id="6624b-737">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6624b-738">Delegato che può essere utilizzato per configurare o sostituire la `HttpMessageHandler` utilizzata per inviare le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6624b-739">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="6624b-740">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="6624b-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6624b-741">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova istanza di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="6624b-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6624b-742">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="6624b-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6624b-743">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6624b-744">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6624b-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6624b-745">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6624b-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6624b-746">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="6624b-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6624b-747">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="6624b-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6624b-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6624b-749">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="6624b-749">JavaScript Option</span></span> | <span data-ttu-id="6624b-750">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-750">Default Value</span></span> | <span data-ttu-id="6624b-751">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6624b-752">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6624b-753">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-754">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-755">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6624b-756">Java</span><span class="sxs-lookup"><span data-stu-id="6624b-756">Java</span></span>](#tab/java)

| <span data-ttu-id="6624b-757">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="6624b-757">Java Option</span></span> | <span data-ttu-id="6624b-758">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6624b-758">Default Value</span></span> | <span data-ttu-id="6624b-759">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6624b-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6624b-760">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6624b-761">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6624b-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6624b-762">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6624b-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6624b-763">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="6624b-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6624b-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6624b-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6624b-765">Vuota</span><span class="sxs-lookup"><span data-stu-id="6624b-765">Empty</span></span> | <span data-ttu-id="6624b-766">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6624b-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6624b-767">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito per `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6624b-768">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6624b-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6624b-769">Nel client Java queste opzioni possono essere configurate con i metodi nella `HttpHubConnectionBuilder` restituita dalla `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6624b-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6624b-770">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6624b-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end