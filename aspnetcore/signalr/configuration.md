---
title: Configurazione SignalR ASP.NET Core
author: bradygaster
description: Informazioni su come configurare ASP.NET Core SignalR app.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963976"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="9a400-103">Configurazione SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a400-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="9a400-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="9a400-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="9a400-105">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9a400-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="9a400-106">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9a400-107">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="9a400-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="9a400-108">è possibile aggiungere `AddJsonProtocol` dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9a400-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9a400-109">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="9a400-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="9a400-110">La proprietà [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) su tale oggetto è un oggetto `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="9a400-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="9a400-111">Per ulteriori informazioni, vedere la [documentazione di System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="9a400-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="9a400-112">Ad esempio, per configurare il serializzatore in modo da non modificare la combinazione di maiuscole e minuscole dei nomi di proprietà, anziché i nomi predefiniti "camelCase", usare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9a400-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="9a400-113">Nel client .NET è presente lo stesso `AddJsonProtocol` metodo di estensione in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="9a400-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="9a400-114">È necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection` per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="9a400-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="9a400-115">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a400-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="9a400-116">Passa a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="9a400-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="9a400-117">Se sono necessarie funzionalità di `Newtonsoft.Json` che non sono supportate in `System.Text.Json`, vedere [passare a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="9a400-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="9a400-118">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9a400-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9a400-119">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="9a400-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="9a400-120">La proprietà [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) su tale oggetto è un oggetto `JsonSerializerSettings` JSON.NET che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="9a400-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="9a400-121">Per ulteriori informazioni, vedere la [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="9a400-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="9a400-122">Per configurare, ad esempio, il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi predefiniti "camelCase", utilizzare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9a400-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="9a400-123">Nel client .NET è presente lo stesso `AddJsonProtocol` metodo di estensione in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="9a400-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="9a400-124">È necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection` per risolvere il metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="9a400-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="9a400-125">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a400-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="9a400-126">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="9a400-126">MessagePack serialization options</span></span>

<span data-ttu-id="9a400-127">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="9a400-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="9a400-128">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="9a400-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="9a400-129">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a400-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="9a400-130">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="9a400-130">Configure server options</span></span>

<span data-ttu-id="9a400-131">La tabella seguente descrive le opzioni per la configurazione di hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="9a400-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="9a400-132">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-132">Option</span></span> | <span data-ttu-id="9a400-133">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-133">Default Value</span></span> | <span data-ttu-id="9a400-134">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="9a400-135">30 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-135">30 seconds</span></span> | <span data-ttu-id="9a400-136">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="9a400-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="9a400-137">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="9a400-138">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="9a400-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="9a400-139">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-139">15 seconds</span></span> | <span data-ttu-id="9a400-140">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="9a400-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="9a400-141">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-142">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="9a400-143">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-143">15 seconds</span></span> | <span data-ttu-id="9a400-144">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="9a400-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="9a400-145">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="9a400-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="9a400-146">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="9a400-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="9a400-147">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="9a400-147">All installed protocols</span></span> | <span data-ttu-id="9a400-148">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-148">Protocols supported by this hub.</span></span> <span data-ttu-id="9a400-149">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="9a400-150">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="9a400-151">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="9a400-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="9a400-152">Numero massimo di elementi che possono essere memorizzati nel buffer per i flussi di caricamento client.</span><span class="sxs-lookup"><span data-stu-id="9a400-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="9a400-153">Se viene raggiunto questo limite, l'elaborazione delle chiamate viene bloccata fino a quando il server non elabora gli elementi del flusso.</span><span class="sxs-lookup"><span data-stu-id="9a400-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="9a400-154">32 KB</span><span class="sxs-lookup"><span data-stu-id="9a400-154">32 KB</span></span> | <span data-ttu-id="9a400-155">Dimensione massima di un singolo messaggio dell'hub in ingresso.</span><span class="sxs-lookup"><span data-stu-id="9a400-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="9a400-156">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-156">Option</span></span> | <span data-ttu-id="9a400-157">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-157">Default Value</span></span> | <span data-ttu-id="9a400-158">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="9a400-159">30 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-159">30 seconds</span></span> | <span data-ttu-id="9a400-160">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="9a400-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="9a400-161">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="9a400-162">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="9a400-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="9a400-163">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-163">15 seconds</span></span> | <span data-ttu-id="9a400-164">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="9a400-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="9a400-165">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-166">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="9a400-167">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-167">15 seconds</span></span> | <span data-ttu-id="9a400-168">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="9a400-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="9a400-169">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="9a400-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="9a400-170">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="9a400-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="9a400-171">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="9a400-171">All installed protocols</span></span> | <span data-ttu-id="9a400-172">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-172">Protocols supported by this hub.</span></span> <span data-ttu-id="9a400-173">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="9a400-174">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="9a400-175">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="9a400-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="9a400-176">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-176">Option</span></span> | <span data-ttu-id="9a400-177">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-177">Default Value</span></span> | <span data-ttu-id="9a400-178">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="9a400-179">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-179">15 seconds</span></span> | <span data-ttu-id="9a400-180">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="9a400-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="9a400-181">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-182">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="9a400-183">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-183">15 seconds</span></span> | <span data-ttu-id="9a400-184">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="9a400-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="9a400-185">Quando si modifica `KeepAliveInterval`, modificare l'impostazione di `ServerTimeout`/`serverTimeoutInMilliseconds` nel client.</span><span class="sxs-lookup"><span data-stu-id="9a400-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="9a400-186">Il valore `ServerTimeout`/`serverTimeoutInMilliseconds` consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="9a400-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="9a400-187">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="9a400-187">All installed protocols</span></span> | <span data-ttu-id="9a400-188">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-188">Protocols supported by this hub.</span></span> <span data-ttu-id="9a400-189">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="9a400-190">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="9a400-191">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="9a400-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="9a400-192">È possibile configurare le opzioni per tutti gli hub fornendo le opzioni delegate alla chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9a400-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="9a400-193">Le opzioni per un singolo Hub eseguono l'override delle opzioni globali fornite in `AddSignalR` e possono essere configurate utilizzando <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="9a400-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="9a400-194">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="9a400-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9a400-195">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="9a400-196">Queste opzioni vengono configurate passando un delegato a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9a400-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="9a400-197">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="9a400-198">Queste opzioni vengono configurate passando un delegato a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9a400-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="9a400-199">Nella tabella seguente vengono descritte le opzioni per la configurazione di ASP.NET Core opzioni HTTP avanzate di SignalR:</span><span class="sxs-lookup"><span data-stu-id="9a400-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="9a400-200">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-200">Option</span></span> | <span data-ttu-id="9a400-201">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-201">Default Value</span></span> | <span data-ttu-id="9a400-202">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="9a400-203">32 KB</span><span class="sxs-lookup"><span data-stu-id="9a400-203">32 KB</span></span> | <span data-ttu-id="9a400-204">Il numero massimo di byte ricevuti dal client che il server memorizza nel buffer prima di applicare la contropressione.</span><span class="sxs-lookup"><span data-stu-id="9a400-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="9a400-205">L'aumento di questo valore consente al server di ricevere più rapidamente messaggi di dimensioni maggiori senza applicare la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="9a400-206">Dati raccolti automaticamente da `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="9a400-207">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="9a400-208">32 KB</span><span class="sxs-lookup"><span data-stu-id="9a400-208">32 KB</span></span> | <span data-ttu-id="9a400-209">Numero massimo di byte inviati dall'app che il server memorizza nel buffer prima di osservare la sovrapressione.</span><span class="sxs-lookup"><span data-stu-id="9a400-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="9a400-210">L'aumento di questo valore consente al server di memorizzare nel buffer i messaggi di dimensioni maggiori più rapidamente senza attendere la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="9a400-211">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="9a400-211">All Transports are enabled.</span></span> | <span data-ttu-id="9a400-212">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="9a400-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="9a400-213">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="9a400-213">See below.</span></span> | <span data-ttu-id="9a400-214">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="9a400-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="9a400-215">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="9a400-215">See below.</span></span> | <span data-ttu-id="9a400-216">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="9a400-217">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-217">Option</span></span> | <span data-ttu-id="9a400-218">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-218">Default Value</span></span> | <span data-ttu-id="9a400-219">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="9a400-220">32 KB</span><span class="sxs-lookup"><span data-stu-id="9a400-220">32 KB</span></span> | <span data-ttu-id="9a400-221">Numero massimo di byte ricevuti dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="9a400-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="9a400-222">L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="9a400-223">Dati raccolti automaticamente da `Authorize` attributi applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="9a400-224">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="9a400-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="9a400-225">32 KB</span><span class="sxs-lookup"><span data-stu-id="9a400-225">32 KB</span></span> | <span data-ttu-id="9a400-226">Numero massimo di byte inviati dall'app che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="9a400-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="9a400-227">L'aumento di questo valore consente al server di inviare messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="9a400-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="9a400-228">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="9a400-228">All Transports are enabled.</span></span> | <span data-ttu-id="9a400-229">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="9a400-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="9a400-230">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="9a400-230">See below.</span></span> | <span data-ttu-id="9a400-231">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="9a400-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="9a400-232">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="9a400-232">See below.</span></span> | <span data-ttu-id="9a400-233">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="9a400-234">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate utilizzando la proprietà `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="9a400-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="9a400-235">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-235">Option</span></span> | <span data-ttu-id="9a400-236">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-236">Default Value</span></span> | <span data-ttu-id="9a400-237">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="9a400-238">90 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-238">90 seconds</span></span> | <span data-ttu-id="9a400-239">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="9a400-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="9a400-240">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="9a400-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="9a400-241">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate usando la proprietà `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="9a400-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="9a400-242">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-242">Option</span></span> | <span data-ttu-id="9a400-243">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-243">Default Value</span></span> | <span data-ttu-id="9a400-244">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="9a400-245">5 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-245">5 seconds</span></span> | <span data-ttu-id="9a400-246">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="9a400-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="9a400-247">Delegato che può essere utilizzato per impostare l'intestazione `Sec-WebSocket-Protocol` su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="9a400-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="9a400-248">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="9a400-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="9a400-249">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="9a400-249">Configure client options</span></span>

<span data-ttu-id="9a400-250">Le opzioni client possono essere configurate nel tipo di `HubConnectionBuilder`, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a400-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="9a400-251">È anche disponibile nel client Java, ma la sottoclasse `HttpHubConnectionBuilder` contiene le opzioni di configurazione del compilatore, oltre al `HubConnection` stesso.</span><span class="sxs-lookup"><span data-stu-id="9a400-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="9a400-252">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="9a400-252">Configure logging</span></span>

<span data-ttu-id="9a400-253">La registrazione viene configurata nel client .NET utilizzando il metodo `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="9a400-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="9a400-254">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="9a400-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="9a400-255">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="9a400-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="9a400-256">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="9a400-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="9a400-257">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="9a400-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="9a400-258">Ad esempio, per abilitare la registrazione della console, installare il pacchetto NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="9a400-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="9a400-259">Chiamare il metodo di estensione `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="9a400-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="9a400-260">Nel client JavaScript esiste un metodo di `configureLogging` simile.</span><span class="sxs-lookup"><span data-stu-id="9a400-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="9a400-261">Consente di specificare un valore `LogLevel` che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="9a400-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="9a400-262">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="9a400-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9a400-263">Anziché un valore di `LogLevel`, è anche possibile fornire un valore `string` che rappresenta il nome di un livello di log.</span><span class="sxs-lookup"><span data-stu-id="9a400-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="9a400-264">Questa operazione è utile quando si configura SignalR la registrazione in ambienti in cui non si ha accesso alle costanti `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="9a400-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="9a400-265">Nella tabella seguente sono elencati i livelli di log disponibili.</span><span class="sxs-lookup"><span data-stu-id="9a400-265">The following table lists the available log levels.</span></span> <span data-ttu-id="9a400-266">Il valore fornito per `configureLogging` imposta il livello di registrazione **minimo** che verrà registrato.</span><span class="sxs-lookup"><span data-stu-id="9a400-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="9a400-267">Verranno registrati i messaggi registrati a questo livello **o i livelli elencati dopo tale operazione nella tabella**.</span><span class="sxs-lookup"><span data-stu-id="9a400-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="9a400-268">Stringa</span><span class="sxs-lookup"><span data-stu-id="9a400-268">String</span></span>                      | <span data-ttu-id="9a400-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="9a400-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="9a400-270">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="9a400-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="9a400-271">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="9a400-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="9a400-272">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nel metodo `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="9a400-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="9a400-273">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnosticaSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="9a400-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="9a400-274">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="9a400-275">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="9a400-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="9a400-276">Il frammento di codice seguente illustra come usare `java.util.logging` con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="9a400-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="9a400-277">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="9a400-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="9a400-278">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="9a400-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="9a400-279">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="9a400-279">Configure allowed transports</span></span>

<span data-ttu-id="9a400-280">I trasporti utilizzati da SignalR possono essere configurati nella chiamata `WithUrl` (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9a400-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="9a400-281">È possibile utilizzare un operatore OR bit per bit dei valori di `HttpTransportType` per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="9a400-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="9a400-282">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9a400-282">All transports are enabled by default.</span></span>

<span data-ttu-id="9a400-283">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="9a400-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="9a400-284">Nel client JavaScript, i trasporti vengono configurati impostando il campo `transport` nell'oggetto options fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9a400-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9a400-285">In questa versione dei WebSocket client Java è l'unico trasporto disponibile.</span><span class="sxs-lookup"><span data-stu-id="9a400-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="9a400-286">Nel client Java il trasporto viene selezionato con il metodo `withTransport` nel `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9a400-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="9a400-287">Per impostazione predefinita, il client Java usa il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="9a400-288">Il client Java SignalR non supporta ancora il fallback del trasporto.</span><span class="sxs-lookup"><span data-stu-id="9a400-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="9a400-289">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="9a400-289">Configure bearer authentication</span></span>

<span data-ttu-id="9a400-290">Per fornire i dati di autenticazione insieme alle richieste di SignalR, usare l'opzione `AccessTokenProvider` (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="9a400-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="9a400-291">Nel client .NET questo token di accesso viene passato come token "autenticazione di connessione" HTTP (usando l'intestazione di `Authorization` con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="9a400-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="9a400-292">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="9a400-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="9a400-293">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="9a400-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="9a400-294">Nel client .NET è possibile specificare l'opzione `AccessTokenProvider` usando il delegato options in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="9a400-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="9a400-295">Nel client JavaScript, il token di accesso viene configurato impostando il campo `accessTokenFactory` nell'oggetto opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9a400-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="9a400-296">Nel client SignalR Java è possibile configurare una bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="9a400-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="9a400-297">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per [fornire una](https://github.com/ReactiveX/RxJava) [> stringa\<Single](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="9a400-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="9a400-298">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="9a400-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="9a400-299">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="9a400-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="9a400-300">Opzioni aggiuntive per la configurazione del comportamento di timeout e Keep-Alive sono disponibili nell'oggetto `HubConnection` stesso:</span><span class="sxs-lookup"><span data-stu-id="9a400-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="9a400-301">.NET</span><span class="sxs-lookup"><span data-stu-id="9a400-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="9a400-302">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-302">Option</span></span> | <span data-ttu-id="9a400-303">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-303">Default value</span></span> | <span data-ttu-id="9a400-304">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="9a400-305">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-306">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-306">Timeout for server activity.</span></span> <span data-ttu-id="9a400-307">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9a400-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9a400-308">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-309">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="9a400-310">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-310">15 seconds</span></span> | <span data-ttu-id="9a400-311">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="9a400-312">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9a400-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9a400-313">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-314">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="9a400-315">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-315">15 seconds</span></span> | <span data-ttu-id="9a400-316">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="9a400-317">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="9a400-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="9a400-318">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="9a400-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="9a400-319">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="9a400-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="9a400-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a400-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="9a400-321">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-321">Option</span></span> | <span data-ttu-id="9a400-322">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-322">Default value</span></span> | <span data-ttu-id="9a400-323">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="9a400-324">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-325">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-325">Timeout for server activity.</span></span> <span data-ttu-id="9a400-326">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="9a400-327">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-328">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="9a400-329">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="9a400-330">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="9a400-331">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="9a400-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="9a400-332">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="9a400-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="9a400-333">Java</span><span class="sxs-lookup"><span data-stu-id="9a400-333">Java</span></span>](#tab/java)

| <span data-ttu-id="9a400-334">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-334">Option</span></span> | <span data-ttu-id="9a400-335">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-335">Default value</span></span> | <span data-ttu-id="9a400-336">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="9a400-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="9a400-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="9a400-338">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-339">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-339">Timeout for server activity.</span></span> <span data-ttu-id="9a400-340">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="9a400-341">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-342">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="9a400-343">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-343">15 seconds</span></span> | <span data-ttu-id="9a400-344">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="9a400-345">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="9a400-346">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-347">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="9a400-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="9a400-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="9a400-349">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="9a400-350">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="9a400-351">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="9a400-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="9a400-352">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="9a400-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="9a400-353">.NET</span><span class="sxs-lookup"><span data-stu-id="9a400-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="9a400-354">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-354">Option</span></span> | <span data-ttu-id="9a400-355">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-355">Default value</span></span> | <span data-ttu-id="9a400-356">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="9a400-357">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-358">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-358">Timeout for server activity.</span></span> <span data-ttu-id="9a400-359">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9a400-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9a400-360">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-361">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="9a400-362">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-362">15 seconds</span></span> | <span data-ttu-id="9a400-363">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="9a400-364">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="9a400-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="9a400-365">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-366">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="9a400-367">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="9a400-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="9a400-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a400-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="9a400-369">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-369">Option</span></span> | <span data-ttu-id="9a400-370">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-370">Default value</span></span> | <span data-ttu-id="9a400-371">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="9a400-372">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-373">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-373">Timeout for server activity.</span></span> <span data-ttu-id="9a400-374">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="9a400-375">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-376">Il valore consigliato è un numero almeno doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="9a400-377">Java</span><span class="sxs-lookup"><span data-stu-id="9a400-377">Java</span></span>](#tab/java)

| <span data-ttu-id="9a400-378">Opzione</span><span class="sxs-lookup"><span data-stu-id="9a400-378">Option</span></span> | <span data-ttu-id="9a400-379">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-379">Default value</span></span> | <span data-ttu-id="9a400-380">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="9a400-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="9a400-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="9a400-382">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="9a400-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="9a400-383">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-383">Timeout for server activity.</span></span> <span data-ttu-id="9a400-384">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="9a400-385">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="9a400-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="9a400-386">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server, per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="9a400-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="9a400-387">15 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-387">15 seconds</span></span> | <span data-ttu-id="9a400-388">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="9a400-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="9a400-389">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="9a400-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="9a400-390">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="9a400-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="9a400-391">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'HubSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="9a400-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="9a400-392">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9a400-392">Configure additional options</span></span>

<span data-ttu-id="9a400-393">È possibile configurare opzioni aggiuntive nel metodo `WithUrl` (`withUrl` in JavaScript) in `HubConnectionBuilder` o nelle varie API di configurazione del `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="9a400-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="9a400-394">.NET</span><span class="sxs-lookup"><span data-stu-id="9a400-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="9a400-395">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="9a400-395">.NET Option</span></span> |  <span data-ttu-id="9a400-396">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-396">Default value</span></span> | <span data-ttu-id="9a400-397">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="9a400-398">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="9a400-399">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="9a400-400">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="9a400-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="9a400-401">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a400-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="9a400-402">Empty</span><span class="sxs-lookup"><span data-stu-id="9a400-402">Empty</span></span> | <span data-ttu-id="9a400-403">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="9a400-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="9a400-404">Empty</span><span class="sxs-lookup"><span data-stu-id="9a400-404">Empty</span></span> | <span data-ttu-id="9a400-405">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="9a400-406">Empty</span><span class="sxs-lookup"><span data-stu-id="9a400-406">Empty</span></span> | <span data-ttu-id="9a400-407">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="9a400-408">5 secondi</span><span class="sxs-lookup"><span data-stu-id="9a400-408">5 seconds</span></span> | <span data-ttu-id="9a400-409">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-409">WebSockets only.</span></span> <span data-ttu-id="9a400-410">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="9a400-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="9a400-411">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="9a400-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="9a400-412">Empty</span><span class="sxs-lookup"><span data-stu-id="9a400-412">Empty</span></span> | <span data-ttu-id="9a400-413">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="9a400-414">Delegato che può essere utilizzato per configurare o sostituire la `HttpMessageHandler` utilizzata per inviare le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="9a400-415">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="9a400-416">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="9a400-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="9a400-417">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova istanza di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="9a400-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="9a400-418">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="9a400-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="9a400-419">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="9a400-420">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9a400-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="9a400-421">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="9a400-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="9a400-422">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="9a400-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="9a400-423">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="9a400-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="9a400-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a400-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="9a400-425">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a400-425">JavaScript Option</span></span> | <span data-ttu-id="9a400-426">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-426">Default Value</span></span> | <span data-ttu-id="9a400-427">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="9a400-428">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="9a400-429">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="9a400-430">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="9a400-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="9a400-431">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a400-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="9a400-432">Java</span><span class="sxs-lookup"><span data-stu-id="9a400-432">Java</span></span>](#tab/java)

| <span data-ttu-id="9a400-433">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="9a400-433">Java Option</span></span> | <span data-ttu-id="9a400-434">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="9a400-434">Default Value</span></span> | <span data-ttu-id="9a400-435">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9a400-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="9a400-436">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="9a400-437">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="9a400-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="9a400-438">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="9a400-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="9a400-439">Questa impostazione non può essere abilitata quando si usa il servizio SignalR di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a400-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="9a400-440">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="9a400-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="9a400-441">Empty</span><span class="sxs-lookup"><span data-stu-id="9a400-441">Empty</span></span> | <span data-ttu-id="9a400-442">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a400-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="9a400-443">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito per `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="9a400-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="9a400-444">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito per `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="9a400-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="9a400-445">Nel client Java queste opzioni possono essere configurate con i metodi nella `HttpHubConnectionBuilder` restituita dalla `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="9a400-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="9a400-446">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9a400-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
