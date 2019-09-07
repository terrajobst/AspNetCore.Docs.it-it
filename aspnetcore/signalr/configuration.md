---
title: Configurazione di ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come configurare ASP.NET Core le app SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773922"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="6ddf6-103">Configurazione di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6ddf6-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6ddf6-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6ddf6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6ddf6-105">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6ddf6-106">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="6ddf6-107">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , che può essere aggiunto `Startup.ConfigureServices` dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel metodo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6ddf6-108">Il `AddJsonProtocol` metodo accetta un delegato che riceve un `options` oggetto.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6ddf6-109">La proprietà [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) su tale oggetto è un oggetto `JsonSerializerSettings` JSON.NET che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6ddf6-110">Per altri dettagli, vedere la [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="6ddf6-111">Per configurare, ad esempio, il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi predefiniti "camelCase", utilizzare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="6ddf6-112">Nel client .NET lo stesso `AddJsonProtocol` metodo di estensione è presente in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6ddf6-113">Per `Microsoft.Extensions.DependencyInjection` risolvere il metodo di estensione, è necessario importare lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="6ddf6-114">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6ddf6-115">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="6ddf6-115">MessagePack serialization options</span></span>

<span data-ttu-id="6ddf6-116">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6ddf6-117">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6ddf6-118">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6ddf6-119">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="6ddf6-119">Configure server options</span></span>

<span data-ttu-id="6ddf6-120">La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6ddf6-121">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-121">Option</span></span> | <span data-ttu-id="6ddf6-122">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-122">Default Value</span></span> | <span data-ttu-id="6ddf6-123">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6ddf6-124">30 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-124">30 seconds</span></span> | <span data-ttu-id="6ddf6-125">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6ddf6-126">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6ddf6-127">Il valore consigliato è Double `KeepAliveInterval` .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6ddf6-128">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-128">15 seconds</span></span> | <span data-ttu-id="6ddf6-129">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6ddf6-130">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-131">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6ddf6-132">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-132">15 seconds</span></span> | <span data-ttu-id="6ddf6-133">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6ddf6-134">Quando si `KeepAliveInterval`modifica, modificare `ServerTimeout` / l'`serverTimeoutInMilliseconds` impostazione nel client.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6ddf6-135">Il valore `ServerTimeout` consigliato / è`serverTimeoutInMilliseconds` Double .`KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6ddf6-136">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6ddf6-136">All installed protocols</span></span> | <span data-ttu-id="6ddf6-137">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-137">Protocols supported by this hub.</span></span> <span data-ttu-id="6ddf6-138">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6ddf6-139">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6ddf6-140">Il valore predefinito `false`è, poiché questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="6ddf6-141">Numero massimo di elementi che possono essere memorizzati nel buffer per i flussi di caricamento client.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="6ddf6-142">Se viene raggiunto questo limite, l'elaborazione delle chiamate viene bloccata fino a quando il server non elabora gli elementi del flusso.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="6ddf6-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="6ddf6-143">32 KB</span></span> | <span data-ttu-id="6ddf6-144">Dimensione massima di un singolo messaggio dell'hub in ingresso.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="6ddf6-145">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-145">Option</span></span> | <span data-ttu-id="6ddf6-146">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-146">Default Value</span></span> | <span data-ttu-id="6ddf6-147">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6ddf6-148">30 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-148">30 seconds</span></span> | <span data-ttu-id="6ddf6-149">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6ddf6-150">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6ddf6-151">Il valore consigliato è Double `KeepAliveInterval` .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6ddf6-152">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-152">15 seconds</span></span> | <span data-ttu-id="6ddf6-153">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6ddf6-154">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-155">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6ddf6-156">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-156">15 seconds</span></span> | <span data-ttu-id="6ddf6-157">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6ddf6-158">Quando si `KeepAliveInterval`modifica, modificare `ServerTimeout` / l'`serverTimeoutInMilliseconds` impostazione nel client.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6ddf6-159">Il valore `ServerTimeout` consigliato / è`serverTimeoutInMilliseconds` Double .`KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6ddf6-160">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6ddf6-160">All installed protocols</span></span> | <span data-ttu-id="6ddf6-161">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-161">Protocols supported by this hub.</span></span> <span data-ttu-id="6ddf6-162">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6ddf6-163">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6ddf6-164">Il valore predefinito `false`è, poiché questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="6ddf6-165">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-165">Option</span></span> | <span data-ttu-id="6ddf6-166">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-166">Default Value</span></span> | <span data-ttu-id="6ddf6-167">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="6ddf6-168">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-168">15 seconds</span></span> | <span data-ttu-id="6ddf6-169">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6ddf6-170">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-171">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6ddf6-172">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-172">15 seconds</span></span> | <span data-ttu-id="6ddf6-173">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6ddf6-174">Quando si `KeepAliveInterval`modifica, modificare `ServerTimeout` / l'`serverTimeoutInMilliseconds` impostazione nel client.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6ddf6-175">Il valore `ServerTimeout` consigliato / è`serverTimeoutInMilliseconds` Double .`KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6ddf6-176">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="6ddf6-176">All installed protocols</span></span> | <span data-ttu-id="6ddf6-177">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-177">Protocols supported by this hub.</span></span> <span data-ttu-id="6ddf6-178">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6ddf6-179">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6ddf6-180">Il valore predefinito `false`è, poiché questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="6ddf6-181">È possibile configurare le opzioni per tutti gli hub fornendo le `AddSignalR` opzioni delegate alla chiamata in. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6ddf6-182">Le opzioni per un singolo Hub eseguono l'override delle opzioni `AddSignalR` globali fornite in e possono <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>essere configurate utilizzando:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6ddf6-183">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="6ddf6-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6ddf6-184">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6ddf6-185">Queste opzioni vengono configurate passando un delegato [a\<MapHub T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="6ddf6-186">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6ddf6-187">Queste opzioni vengono configurate passando un delegato [a\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="6ddf6-188">La tabella seguente descrive le opzioni per la configurazione delle opzioni HTTP avanzate di ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6ddf6-189">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-189">Option</span></span> | <span data-ttu-id="6ddf6-190">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-190">Default Value</span></span> | <span data-ttu-id="6ddf6-191">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6ddf6-192">32 KB</span><span class="sxs-lookup"><span data-stu-id="6ddf6-192">32 KB</span></span> | <span data-ttu-id="6ddf6-193">Il numero massimo di byte ricevuti dal client che il server memorizza nel buffer prima di applicare la contropressione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="6ddf6-194">L'aumento di questo valore consente al server di ricevere più rapidamente messaggi di dimensioni maggiori senza applicare la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6ddf6-195">Dati raccolti automaticamente dagli `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6ddf6-196">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6ddf6-197">32 KB</span><span class="sxs-lookup"><span data-stu-id="6ddf6-197">32 KB</span></span> | <span data-ttu-id="6ddf6-198">Numero massimo di byte inviati dall'app che il server memorizza nel buffer prima di osservare la sovrapressione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="6ddf6-199">L'aumento di questo valore consente al server di memorizzare nel buffer i messaggi di dimensioni maggiori più rapidamente senza attendere la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6ddf6-200">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-200">All Transports are enabled.</span></span> | <span data-ttu-id="6ddf6-201">Enumerazione dei flag di bit `HttpTransportType` di valori che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6ddf6-202">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-202">See below.</span></span> | <span data-ttu-id="6ddf6-203">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6ddf6-204">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-204">See below.</span></span> | <span data-ttu-id="6ddf6-205">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="6ddf6-206">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-206">Option</span></span> | <span data-ttu-id="6ddf6-207">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-207">Default Value</span></span> | <span data-ttu-id="6ddf6-208">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6ddf6-209">32 KB</span><span class="sxs-lookup"><span data-stu-id="6ddf6-209">32 KB</span></span> | <span data-ttu-id="6ddf6-210">Numero massimo di byte ricevuti dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="6ddf6-211">L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6ddf6-212">Dati raccolti automaticamente dagli `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6ddf6-213">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6ddf6-214">32 KB</span><span class="sxs-lookup"><span data-stu-id="6ddf6-214">32 KB</span></span> | <span data-ttu-id="6ddf6-215">Numero massimo di byte inviati dall'app che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="6ddf6-216">L'aumento di questo valore consente al server di inviare messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6ddf6-217">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-217">All Transports are enabled.</span></span> | <span data-ttu-id="6ddf6-218">Enumerazione dei flag di bit `HttpTransportType` di valori che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6ddf6-219">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-219">See below.</span></span> | <span data-ttu-id="6ddf6-220">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6ddf6-221">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-221">See below.</span></span> | <span data-ttu-id="6ddf6-222">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="6ddf6-223">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate tramite la `LongPolling` proprietà:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6ddf6-224">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-224">Option</span></span> | <span data-ttu-id="6ddf6-225">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-225">Default Value</span></span> | <span data-ttu-id="6ddf6-226">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6ddf6-227">90 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-227">90 seconds</span></span> | <span data-ttu-id="6ddf6-228">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6ddf6-229">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6ddf6-230">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate tramite la `WebSockets` proprietà:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6ddf6-231">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-231">Option</span></span> | <span data-ttu-id="6ddf6-232">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-232">Default Value</span></span> | <span data-ttu-id="6ddf6-233">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6ddf6-234">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-234">5 seconds</span></span> | <span data-ttu-id="6ddf6-235">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6ddf6-236">Delegato che può essere utilizzato per impostare l' `Sec-WebSocket-Protocol` intestazione su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6ddf6-237">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6ddf6-238">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="6ddf6-238">Configure client options</span></span>

<span data-ttu-id="6ddf6-239">Le `HubConnectionBuilder` opzioni client possono essere configurate nel tipo, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6ddf6-240">È anche disponibile nel client Java, ma la `HttpHubConnectionBuilder` sottoclasse è quella che contiene le opzioni di configurazione del compilatore, oltre `HubConnection` a quella di.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6ddf6-241">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-241">Configure logging</span></span>

<span data-ttu-id="6ddf6-242">La registrazione viene configurata nel client .NET `ConfigureLogging` utilizzando il metodo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6ddf6-243">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6ddf6-244">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6ddf6-245">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6ddf6-246">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6ddf6-247">Ad esempio, per abilitare la registrazione della console, `Microsoft.Extensions.Logging.Console` installare il pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6ddf6-248">Chiamare il `AddConsole` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6ddf6-249">Nel client JavaScript esiste un metodo simile `configureLogging` .</span><span class="sxs-lookup"><span data-stu-id="6ddf6-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6ddf6-250">Fornire un `LogLevel` valore che indichi il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6ddf6-251">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6ddf6-252">Anziché un `LogLevel` valore, è anche possibile fornire un `string` valore che rappresenta il nome di un livello di log.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="6ddf6-253">Questa operazione è utile quando si configura la `LogLevel` registrazione di SignalR negli ambienti in cui non è possibile accedere alle costanti.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="6ddf6-254">Nella tabella seguente sono elencati i livelli di log disponibili.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-254">The following table lists the available log levels.</span></span> <span data-ttu-id="6ddf6-255">Il valore fornito per impostare `configureLogging` il livello di registrazione **minimo** che verrà registrato.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="6ddf6-256">Verranno registrati i messaggi registrati a questo livello **o i livelli elencati dopo tale operazione nella tabella**.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="6ddf6-257">String</span><span class="sxs-lookup"><span data-stu-id="6ddf6-257">String</span></span>                      | <span data-ttu-id="6ddf6-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="6ddf6-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="6ddf6-259">`info`**o**`information`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="6ddf6-260">`warn`**o**`warning`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6ddf6-261">Per disabilitare completamente la `signalR.LogLevel.None` `configureLogging` registrazione, specificare nel metodo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6ddf6-262">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnostica di SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6ddf6-263">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6ddf6-264">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6ddf6-265">Il frammento di codice seguente illustra come `java.util.logging` usare con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6ddf6-266">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6ddf6-267">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6ddf6-268">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="6ddf6-268">Configure allowed transports</span></span>

<span data-ttu-id="6ddf6-269">I trasporti usati da SignalR possono essere configurati nella `WithUrl` chiamata (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6ddf6-270">Un operatore OR bit per bit dei valori `HttpTransportType` di può essere utilizzato per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6ddf6-271">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-271">All transports are enabled by default.</span></span>

<span data-ttu-id="6ddf6-272">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6ddf6-273">Nel client JavaScript, i trasporti vengono configurati impostando il `transport` campo nell'oggetto opzioni fornito a: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6ddf6-274">In questa versione dei WebSocket client Java è l'unico trasporto disponibile.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="6ddf6-275">Nel client Java il trasporto viene selezionato con il `withTransport` metodo `HttpHubConnectionBuilder`in.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="6ddf6-276">Per impostazione predefinita, il client Java usa il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6ddf6-277">Il client Java SignalR non supporta ancora il fallback del trasporto.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6ddf6-278">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="6ddf6-278">Configure bearer authentication</span></span>

<span data-ttu-id="6ddf6-279">Per fornire i dati di autenticazione insieme alle richieste SignalR, `AccessTokenProvider` usare l'`accessTokenFactory` opzione (in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6ddf6-280">Nel client .NET questo token di accesso viene passato come token http "Bearer Authentication" (usando l' `Authorization` intestazione con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6ddf6-281">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6ddf6-282">In questi casi, il token di accesso viene fornito come valore `access_token`stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6ddf6-283">Nel client .NET è possibile specificare `AccessTokenProvider` l'opzione usando il delegato options in: `WithUrl`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6ddf6-284">Nel client JavaScript, il token di accesso viene configurato impostando il `accessTokenFactory` campo nell'oggetto opzioni in: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6ddf6-285">Nel client Java SignalR è possibile configurare un bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6ddf6-286">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire una [RxJava](https://github.com/ReactiveX/RxJava) [stringa a\<> singola](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6ddf6-287">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6ddf6-288">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="6ddf6-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6ddf6-289">Opzioni aggiuntive per la configurazione del timeout e del `HubConnection` comportamento keep-alive sono disponibili nell'oggetto stesso:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6ddf6-290">.NET</span><span class="sxs-lookup"><span data-stu-id="6ddf6-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6ddf6-291">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-291">Option</span></span> | <span data-ttu-id="6ddf6-292">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-292">Default value</span></span> | <span data-ttu-id="6ddf6-293">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6ddf6-294">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-295">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-295">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-296">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `Closed` attiva l'`onclose` evento (in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6ddf6-297">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-298">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6ddf6-299">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-299">15 seconds</span></span> | <span data-ttu-id="6ddf6-300">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="6ddf6-301">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l' `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6ddf6-302">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-303">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6ddf6-304">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-304">15 seconds</span></span> | <span data-ttu-id="6ddf6-305">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6ddf6-306">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6ddf6-307">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` set nel server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="6ddf6-308">Nel client .NET i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6ddf6-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6ddf6-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6ddf6-310">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-310">Option</span></span> | <span data-ttu-id="6ddf6-311">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-311">Default value</span></span> | <span data-ttu-id="6ddf6-312">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6ddf6-313">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-314">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-314">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-315">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `onclose` attiva l'evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6ddf6-316">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-317">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="6ddf6-318">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="6ddf6-319">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6ddf6-320">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6ddf6-321">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` set nel server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6ddf6-322">Java</span><span class="sxs-lookup"><span data-stu-id="6ddf6-322">Java</span></span>](#tab/java)

| <span data-ttu-id="6ddf6-323">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-323">Option</span></span> | <span data-ttu-id="6ddf6-324">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-324">Default value</span></span> | <span data-ttu-id="6ddf6-325">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6ddf6-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6ddf6-327">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-328">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-328">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-329">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `onClose` attiva l'evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6ddf6-330">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-331">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6ddf6-332">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-332">15 seconds</span></span> | <span data-ttu-id="6ddf6-333">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="6ddf6-334">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l' `onClose` evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6ddf6-335">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-336">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="6ddf6-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="6ddf6-338">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-339">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6ddf6-340">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6ddf6-341">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` set nel server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6ddf6-342">.NET</span><span class="sxs-lookup"><span data-stu-id="6ddf6-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6ddf6-343">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-343">Option</span></span> | <span data-ttu-id="6ddf6-344">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-344">Default value</span></span> | <span data-ttu-id="6ddf6-345">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6ddf6-346">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-347">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-347">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-348">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `Closed` attiva l'`onclose` evento (in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6ddf6-349">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-350">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6ddf6-351">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-351">15 seconds</span></span> | <span data-ttu-id="6ddf6-352">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="6ddf6-353">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l' `Closed` evento (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6ddf6-354">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-355">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="6ddf6-356">Nel client .NET i valori di timeout vengono specificati come `TimeSpan` valori.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6ddf6-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6ddf6-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6ddf6-358">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-358">Option</span></span> | <span data-ttu-id="6ddf6-359">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-359">Default value</span></span> | <span data-ttu-id="6ddf6-360">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6ddf6-361">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-362">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-362">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-363">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `onclose` attiva l'evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6ddf6-364">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-365">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6ddf6-366">Java</span><span class="sxs-lookup"><span data-stu-id="6ddf6-366">Java</span></span>](#tab/java)

| <span data-ttu-id="6ddf6-367">Opzione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-367">Option</span></span> | <span data-ttu-id="6ddf6-368">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-368">Default value</span></span> | <span data-ttu-id="6ddf6-369">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6ddf6-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6ddf6-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6ddf6-371">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="6ddf6-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6ddf6-372">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-372">Timeout for server activity.</span></span> <span data-ttu-id="6ddf6-373">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e `onClose` attiva l'evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6ddf6-374">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6ddf6-375">Il valore consigliato è un numero almeno il doppio del `KeepAliveInterval` valore del server, per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6ddf6-376">15 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-376">15 seconds</span></span> | <span data-ttu-id="6ddf6-377">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="6ddf6-378">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l' `onClose` evento.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6ddf6-379">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6ddf6-380">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6ddf6-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="6ddf6-381">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6ddf6-381">Configure additional options</span></span>

<span data-ttu-id="6ddf6-382">Opzioni aggiuntive possono essere configurate `WithUrl` nel`withUrl` metodo ( `HubConnectionBuilder` in JavaScript) in `HttpHubConnectionBuilder` o nelle varie API di configurazione di nel client Java:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6ddf6-383">.NET</span><span class="sxs-lookup"><span data-stu-id="6ddf6-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6ddf6-384">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="6ddf6-384">.NET Option</span></span> |  <span data-ttu-id="6ddf6-385">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="6ddf6-385">Default value</span></span> | <span data-ttu-id="6ddf6-386">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6ddf6-387">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6ddf6-388">Impostare questa impostazione `true` su per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6ddf6-389">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6ddf6-390">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6ddf6-391">Empty</span><span class="sxs-lookup"><span data-stu-id="6ddf6-391">Empty</span></span> | <span data-ttu-id="6ddf6-392">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6ddf6-393">Empty</span><span class="sxs-lookup"><span data-stu-id="6ddf6-393">Empty</span></span> | <span data-ttu-id="6ddf6-394">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6ddf6-395">Empty</span><span class="sxs-lookup"><span data-stu-id="6ddf6-395">Empty</span></span> | <span data-ttu-id="6ddf6-396">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6ddf6-397">5 secondi</span><span class="sxs-lookup"><span data-stu-id="6ddf6-397">5 seconds</span></span> | <span data-ttu-id="6ddf6-398">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-398">WebSockets only.</span></span> <span data-ttu-id="6ddf6-399">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6ddf6-400">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6ddf6-401">Empty</span><span class="sxs-lookup"><span data-stu-id="6ddf6-401">Empty</span></span> | <span data-ttu-id="6ddf6-402">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6ddf6-403">Delegato che può essere utilizzato per configurare o sostituire l'oggetto `HttpMessageHandler` utilizzato per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6ddf6-404">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="6ddf6-405">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6ddf6-406">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova `HttpMessageHandler` istanza.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6ddf6-407">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="6ddf6-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6ddf6-408">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6ddf6-409">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6ddf6-410">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6ddf6-411">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6ddf6-412">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6ddf6-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6ddf6-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6ddf6-414">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="6ddf6-414">JavaScript Option</span></span> | <span data-ttu-id="6ddf6-415">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-415">Default Value</span></span> | <span data-ttu-id="6ddf6-416">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6ddf6-417">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6ddf6-418">Impostare questa impostazione `true` su per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6ddf6-419">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6ddf6-420">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6ddf6-421">Java</span><span class="sxs-lookup"><span data-stu-id="6ddf6-421">Java</span></span>](#tab/java)

| <span data-ttu-id="6ddf6-422">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="6ddf6-422">Java Option</span></span> | <span data-ttu-id="6ddf6-423">Default Value</span><span class="sxs-lookup"><span data-stu-id="6ddf6-423">Default Value</span></span> | <span data-ttu-id="6ddf6-424">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="6ddf6-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6ddf6-425">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6ddf6-426">Impostare questa impostazione `true` su per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6ddf6-427">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6ddf6-428">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6ddf6-429">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6ddf6-430">Empty</span><span class="sxs-lookup"><span data-stu-id="6ddf6-430">Empty</span></span> | <span data-ttu-id="6ddf6-431">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ddf6-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6ddf6-432">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6ddf6-433">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6ddf6-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6ddf6-434">Nel client Java queste opzioni possono essere configurate con i metodi sull'oggetto `HttpHubConnectionBuilder` restituito dal`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6ddf6-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6ddf6-435">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6ddf6-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
