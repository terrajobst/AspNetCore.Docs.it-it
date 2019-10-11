---
title: Configurazione di ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come configurare ASP.NET Core le app SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246489"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="a2405-103">Configurazione di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a2405-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="a2405-104">Opzioni di serializzazione JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="a2405-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="a2405-105">ASP.NET Core SignalR supporta due protocolli per la codifica dei messaggi: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="a2405-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="a2405-106">Ogni protocollo dispone di opzioni di configurazione della serializzazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2405-107">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="a2405-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="a2405-108">è possibile aggiungere `AddJsonProtocol` dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a2405-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a2405-109">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="a2405-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="a2405-110">La proprietà [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) su tale oggetto è un oggetto `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="a2405-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="a2405-111">Per ulteriori informazioni, vedere la [documentazione di System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="a2405-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="a2405-112">Ad esempio, per configurare il serializzatore in modo da non modificare la combinazione di maiuscole e minuscole dei nomi di proprietà, anziché i nomi predefiniti "camelCase", usare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a2405-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="a2405-113">Nel client .NET è presente lo stesso metodo di estensione `AddJsonProtocol` in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="a2405-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="a2405-114">Per risolvere il metodo di estensione, è necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="a2405-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="a2405-115">Passa a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="a2405-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="a2405-116">Se sono necessarie funzionalità di `Newtonsoft.Json` che non sono supportate in `System.Text.Json`, vedere [passare a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="a2405-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="a2405-117">La serializzazione JSON può essere configurata nel server usando il metodo di estensione [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , che può essere aggiunto dopo [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a2405-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a2405-118">Il metodo `AddJsonProtocol` accetta un delegato che riceve un oggetto `options`.</span><span class="sxs-lookup"><span data-stu-id="a2405-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="a2405-119">La proprietà [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) su tale oggetto è un oggetto JSON.NET `JsonSerializerSettings` che può essere utilizzato per configurare la serializzazione di argomenti e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="a2405-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="a2405-120">Per ulteriori informazioni, vedere la [documentazione di JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="a2405-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="a2405-121">Per configurare, ad esempio, il serializzatore per l'utilizzo di nomi di proprietà "PascalCase", anziché i nomi predefiniti "camelCase", utilizzare il codice seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a2405-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="a2405-122">Nel client .NET è presente lo stesso metodo di estensione `AddJsonProtocol` in [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="a2405-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="a2405-123">Per risolvere il metodo di estensione, è necessario importare lo spazio dei nomi `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="a2405-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a2405-124">Al momento non è possibile configurare la serializzazione JSON nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2405-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="a2405-125">Opzioni di serializzazione MessagePack</span><span class="sxs-lookup"><span data-stu-id="a2405-125">MessagePack serialization options</span></span>

<span data-ttu-id="a2405-126">La serializzazione MessagePack può essere configurata fornendo un delegato alla chiamata [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="a2405-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="a2405-127">Per ulteriori informazioni, vedere [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="a2405-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="a2405-128">Al momento non è possibile configurare la serializzazione MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2405-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="a2405-129">Configurare le opzioni del server</span><span class="sxs-lookup"><span data-stu-id="a2405-129">Configure server options</span></span>

<span data-ttu-id="a2405-130">La tabella seguente descrive le opzioni per la configurazione degli hub SignalR:</span><span class="sxs-lookup"><span data-stu-id="a2405-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="a2405-131">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-131">Option</span></span> | <span data-ttu-id="a2405-132">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-132">Default Value</span></span> | <span data-ttu-id="a2405-133">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="a2405-134">30 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-134">30 seconds</span></span> | <span data-ttu-id="a2405-135">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="a2405-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="a2405-136">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="a2405-137">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="a2405-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="a2405-138">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-138">15 seconds</span></span> | <span data-ttu-id="a2405-139">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="a2405-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a2405-140">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-141">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a2405-142">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-142">15 seconds</span></span> | <span data-ttu-id="a2405-143">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="a2405-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a2405-144">Quando si modifica `KeepAliveInterval`, modificare l'impostazione `ServerTimeout` @ no__t-2 @ no__t-3 nel client.</span><span class="sxs-lookup"><span data-stu-id="a2405-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a2405-145">Il valore `ServerTimeout` @ no__t-1 @ no__t-2 consigliato è il doppio del valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="a2405-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a2405-146">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="a2405-146">All installed protocols</span></span> | <span data-ttu-id="a2405-147">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-147">Protocols supported by this hub.</span></span> <span data-ttu-id="a2405-148">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a2405-149">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a2405-150">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="a2405-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="a2405-151">Numero massimo di elementi che possono essere memorizzati nel buffer per i flussi di caricamento client.</span><span class="sxs-lookup"><span data-stu-id="a2405-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="a2405-152">Se viene raggiunto questo limite, l'elaborazione delle chiamate viene bloccata fino a quando il server non elabora gli elementi del flusso.</span><span class="sxs-lookup"><span data-stu-id="a2405-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="a2405-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="a2405-153">32 KB</span></span> | <span data-ttu-id="a2405-154">Dimensione massima di un singolo messaggio dell'hub in ingresso.</span><span class="sxs-lookup"><span data-stu-id="a2405-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="a2405-155">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-155">Option</span></span> | <span data-ttu-id="a2405-156">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-156">Default Value</span></span> | <span data-ttu-id="a2405-157">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="a2405-158">30 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-158">30 seconds</span></span> | <span data-ttu-id="a2405-159">Il server prenderà in considerazione il client disconnesso se non ha ricevuto un messaggio (incluso Keep-Alive) in questo intervallo.</span><span class="sxs-lookup"><span data-stu-id="a2405-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="a2405-160">Potrebbe essere necessario più tempo di questo intervallo di timeout affinché il client venga effettivamente contrassegnato come disconnesso, a causa della modalità di implementazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="a2405-161">Il valore consigliato è Double il valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="a2405-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="a2405-162">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-162">15 seconds</span></span> | <span data-ttu-id="a2405-163">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="a2405-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a2405-164">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-165">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a2405-166">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-166">15 seconds</span></span> | <span data-ttu-id="a2405-167">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="a2405-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a2405-168">Quando si modifica `KeepAliveInterval`, modificare l'impostazione `ServerTimeout` @ no__t-2 @ no__t-3 nel client.</span><span class="sxs-lookup"><span data-stu-id="a2405-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a2405-169">Il valore `ServerTimeout` @ no__t-1 @ no__t-2 consigliato è il doppio del valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="a2405-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a2405-170">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="a2405-170">All installed protocols</span></span> | <span data-ttu-id="a2405-171">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-171">Protocols supported by this hub.</span></span> <span data-ttu-id="a2405-172">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a2405-173">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a2405-174">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="a2405-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="a2405-175">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-175">Option</span></span> | <span data-ttu-id="a2405-176">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-176">Default Value</span></span> | <span data-ttu-id="a2405-177">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="a2405-178">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-178">15 seconds</span></span> | <span data-ttu-id="a2405-179">Se il client non invia un messaggio di handshake iniziale entro questo intervallo di tempo, la connessione viene chiusa.</span><span class="sxs-lookup"><span data-stu-id="a2405-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a2405-180">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-181">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a2405-182">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-182">15 seconds</span></span> | <span data-ttu-id="a2405-183">Se il server non ha inviato un messaggio entro questo intervallo, viene inviato automaticamente un messaggio di ping per lasciare aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="a2405-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a2405-184">Quando si modifica `KeepAliveInterval`, modificare l'impostazione `ServerTimeout` @ no__t-2 @ no__t-3 nel client.</span><span class="sxs-lookup"><span data-stu-id="a2405-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a2405-185">Il valore `ServerTimeout` @ no__t-1 @ no__t-2 consigliato è il doppio del valore `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="a2405-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a2405-186">Tutti i protocolli installati</span><span class="sxs-lookup"><span data-stu-id="a2405-186">All installed protocols</span></span> | <span data-ttu-id="a2405-187">Protocolli supportati da questo hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-187">Protocols supported by this hub.</span></span> <span data-ttu-id="a2405-188">Per impostazione predefinita, tutti i protocolli registrati nel server sono consentiti, ma i protocolli possono essere rimossi da questo elenco per disabilitare protocolli specifici per i singoli Hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a2405-189">Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a2405-190">Il valore predefinito è `false`, in quanto questi messaggi di eccezione possono contenere informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="a2405-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="a2405-191">È possibile configurare le opzioni per tutti gli hub fornendo un delegato Options per la chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a2405-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="a2405-192">Le opzioni per un singolo Hub eseguono l'override delle opzioni globali fornite in `AddSignalR` e possono essere configurate con <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="a2405-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="a2405-193">Opzioni di configurazione HTTP avanzate</span><span class="sxs-lookup"><span data-stu-id="a2405-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2405-194">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="a2405-195">Queste opzioni vengono configurate passando un delegato a [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a2405-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="a2405-196">Utilizzare `HttpConnectionDispatcherOptions` per configurare le impostazioni avanzate relative ai trasporti e alla gestione dei buffer di memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="a2405-197">Queste opzioni vengono configurate passando un delegato a [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a2405-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="a2405-198">La tabella seguente descrive le opzioni per la configurazione delle opzioni HTTP avanzate di ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="a2405-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="a2405-199">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-199">Option</span></span> | <span data-ttu-id="a2405-200">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-200">Default Value</span></span> | <span data-ttu-id="a2405-201">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="a2405-202">32 KB</span><span class="sxs-lookup"><span data-stu-id="a2405-202">32 KB</span></span> | <span data-ttu-id="a2405-203">Il numero massimo di byte ricevuti dal client che il server memorizza nel buffer prima di applicare la contropressione.</span><span class="sxs-lookup"><span data-stu-id="a2405-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="a2405-204">L'aumento di questo valore consente al server di ricevere più rapidamente messaggi di dimensioni maggiori senza applicare la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="a2405-205">Dati raccolti automaticamente dagli attributi `Authorize` applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="a2405-206">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="a2405-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="a2405-207">32 KB</span></span> | <span data-ttu-id="a2405-208">Numero massimo di byte inviati dall'app che il server memorizza nel buffer prima di osservare la sovrapressione.</span><span class="sxs-lookup"><span data-stu-id="a2405-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="a2405-209">L'aumento di questo valore consente al server di memorizzare nel buffer i messaggi di dimensioni maggiori più rapidamente senza attendere la sovrapressione, ma può aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="a2405-210">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="a2405-210">All Transports are enabled.</span></span> | <span data-ttu-id="a2405-211">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="a2405-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="a2405-212">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="a2405-212">See below.</span></span> | <span data-ttu-id="a2405-213">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="a2405-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="a2405-214">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="a2405-214">See below.</span></span> | <span data-ttu-id="a2405-215">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="a2405-216">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-216">Option</span></span> | <span data-ttu-id="a2405-217">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-217">Default Value</span></span> | <span data-ttu-id="a2405-218">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="a2405-219">32 KB</span><span class="sxs-lookup"><span data-stu-id="a2405-219">32 KB</span></span> | <span data-ttu-id="a2405-220">Numero massimo di byte ricevuti dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="a2405-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="a2405-221">L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="a2405-222">Dati raccolti automaticamente dagli attributi `Authorize` applicati alla classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="a2405-223">Elenco di oggetti [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) usati per determinare se un client è autorizzato a connettersi all'hub.</span><span class="sxs-lookup"><span data-stu-id="a2405-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="a2405-224">32 KB</span><span class="sxs-lookup"><span data-stu-id="a2405-224">32 KB</span></span> | <span data-ttu-id="a2405-225">Numero massimo di byte inviati dall'app che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="a2405-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="a2405-226">L'aumento di questo valore consente al server di inviare messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="a2405-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="a2405-227">Tutti i trasporti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="a2405-227">All Transports are enabled.</span></span> | <span data-ttu-id="a2405-228">Enumerazione di flag di bit di valori `HttpTransportType` che possono limitare i trasporti che un client può utilizzare per la connessione.</span><span class="sxs-lookup"><span data-stu-id="a2405-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="a2405-229">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="a2405-229">See below.</span></span> | <span data-ttu-id="a2405-230">Opzioni aggiuntive specifiche per il trasporto di polling lungo.</span><span class="sxs-lookup"><span data-stu-id="a2405-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="a2405-231">come illustrato più avanti.</span><span class="sxs-lookup"><span data-stu-id="a2405-231">See below.</span></span> | <span data-ttu-id="a2405-232">Opzioni aggiuntive specifiche per il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="a2405-233">Il trasporto di polling lungo dispone di opzioni aggiuntive che possono essere configurate utilizzando la proprietà `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="a2405-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="a2405-234">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-234">Option</span></span> | <span data-ttu-id="a2405-235">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-235">Default Value</span></span> | <span data-ttu-id="a2405-236">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="a2405-237">90 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-237">90 seconds</span></span> | <span data-ttu-id="a2405-238">Quantità massima di tempo di attesa del server per l'invio di un messaggio al client prima che venga terminata una singola richiesta di polling.</span><span class="sxs-lookup"><span data-stu-id="a2405-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="a2405-239">La riduzione di questo valore determina la frequenza con cui il client rilascia più richieste di polling.</span><span class="sxs-lookup"><span data-stu-id="a2405-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="a2405-240">Il trasporto WebSocket dispone di opzioni aggiuntive che possono essere configurate con la proprietà `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="a2405-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="a2405-241">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-241">Option</span></span> | <span data-ttu-id="a2405-242">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-242">Default Value</span></span> | <span data-ttu-id="a2405-243">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="a2405-244">5 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-244">5 seconds</span></span> | <span data-ttu-id="a2405-245">Dopo la chiusura del server, se il client non riesce a chiudersi entro questo intervallo di tempo, la connessione viene terminata.</span><span class="sxs-lookup"><span data-stu-id="a2405-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="a2405-246">Delegato che può essere utilizzato per impostare l'intestazione `Sec-WebSocket-Protocol` su un valore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="a2405-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="a2405-247">Il delegato riceve i valori richiesti dal client come input ed è previsto che restituisca il valore desiderato.</span><span class="sxs-lookup"><span data-stu-id="a2405-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="a2405-248">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="a2405-248">Configure client options</span></span>

<span data-ttu-id="a2405-249">Le opzioni client possono essere configurate nel tipo `HubConnectionBuilder`, disponibile nei client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a2405-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="a2405-250">È anche disponibile nel client Java, ma la sottoclasse `HttpHubConnectionBuilder` contiene le opzioni di configurazione del compilatore, oltre a `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="a2405-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="a2405-251">Configurare la registrazione</span><span class="sxs-lookup"><span data-stu-id="a2405-251">Configure logging</span></span>

<span data-ttu-id="a2405-252">La registrazione viene configurata nel client .NET utilizzando il metodo `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="a2405-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="a2405-253">I provider di registrazione e i filtri possono essere registrati nello stesso modo in cui si trovano nel server.</span><span class="sxs-lookup"><span data-stu-id="a2405-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="a2405-254">Per ulteriori informazioni, vedere la documentazione relativa all' [accesso ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="a2405-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="a2405-255">Per registrare i provider di registrazione, è necessario installare i pacchetti necessari.</span><span class="sxs-lookup"><span data-stu-id="a2405-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="a2405-256">Per un elenco completo, vedere la sezione [provider di registrazione incorporati](xref:fundamentals/logging/index#built-in-logging-providers) di docs.</span><span class="sxs-lookup"><span data-stu-id="a2405-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="a2405-257">Ad esempio, per abilitare la registrazione della console, installare il pacchetto NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="a2405-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="a2405-258">Chiamare il metodo di estensione `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="a2405-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="a2405-259">Nel client JavaScript esiste un metodo `configureLogging` simile.</span><span class="sxs-lookup"><span data-stu-id="a2405-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="a2405-260">Fornire un valore `LogLevel` che indica il livello minimo di messaggi di log da produrre.</span><span class="sxs-lookup"><span data-stu-id="a2405-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="a2405-261">I log vengono scritti nella finestra della console del browser.</span><span class="sxs-lookup"><span data-stu-id="a2405-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2405-262">Anziché un valore `LogLevel`, è anche possibile fornire un valore `string` che rappresenta il nome di un livello di log.</span><span class="sxs-lookup"><span data-stu-id="a2405-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="a2405-263">Questa operazione è utile quando si configura la registrazione di SignalR in ambienti in cui non si ha accesso alle costanti `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="a2405-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="a2405-264">Nella tabella seguente sono elencati i livelli di log disponibili.</span><span class="sxs-lookup"><span data-stu-id="a2405-264">The following table lists the available log levels.</span></span> <span data-ttu-id="a2405-265">Il valore fornito per `configureLogging` imposta il livello di registrazione **minimo** che verrà registrato.</span><span class="sxs-lookup"><span data-stu-id="a2405-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="a2405-266">Verranno registrati i messaggi registrati a questo livello **o i livelli elencati dopo tale operazione nella tabella**.</span><span class="sxs-lookup"><span data-stu-id="a2405-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="a2405-267">String</span><span class="sxs-lookup"><span data-stu-id="a2405-267">String</span></span>                      | <span data-ttu-id="a2405-268">LogLevel</span><span class="sxs-lookup"><span data-stu-id="a2405-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="a2405-269">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="a2405-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="a2405-270">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="a2405-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a2405-271">Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nel metodo `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="a2405-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="a2405-272">Per ulteriori informazioni sulla registrazione, vedere la [documentazione di diagnostica di SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="a2405-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="a2405-273">Il client Java SignalR usa la libreria [SLF4J](https://www.slf4j.org/) per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="a2405-274">Si tratta di un'API di registrazione di alto livello che consente agli utenti della libreria di scegliere una propria implementazione di registrazione specifica inserendo una dipendenza di registrazione specifica.</span><span class="sxs-lookup"><span data-stu-id="a2405-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="a2405-275">Il frammento di codice seguente illustra come usare `java.util.logging` con il client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="a2405-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="a2405-276">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger senza operazioni predefinito con il seguente messaggio di avviso:</span><span class="sxs-lookup"><span data-stu-id="a2405-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="a2405-277">Questa operazione può essere ignorata in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="a2405-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="a2405-278">Configurare i trasporti consentiti</span><span class="sxs-lookup"><span data-stu-id="a2405-278">Configure allowed transports</span></span>

<span data-ttu-id="a2405-279">I trasporti usati da SignalR possono essere configurati nella chiamata `WithUrl` (`withUrl` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a2405-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="a2405-280">È possibile utilizzare un operatore OR bit per bit dei valori di `HttpTransportType` per limitare il client in modo che utilizzi solo i trasporti specificati.</span><span class="sxs-lookup"><span data-stu-id="a2405-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="a2405-281">Tutti i trasporti sono abilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a2405-281">All transports are enabled by default.</span></span>

<span data-ttu-id="a2405-282">Ad esempio, per disabilitare il trasporto degli eventi inviati dal server, ma consentire i WebSocket e le connessioni di polling lungo:</span><span class="sxs-lookup"><span data-stu-id="a2405-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="a2405-283">Nel client JavaScript, i trasporti vengono configurati impostando il campo `transport` nell'oggetto options fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="a2405-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2405-284">In questa versione dei WebSocket client Java è l'unico trasporto disponibile.</span><span class="sxs-lookup"><span data-stu-id="a2405-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="a2405-285">Nel client Java il trasporto viene selezionato con il metodo `withTransport` sul `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a2405-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="a2405-286">Per impostazione predefinita, il client Java usa il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="a2405-287">Il client Java SignalR non supporta ancora il fallback del trasporto.</span><span class="sxs-lookup"><span data-stu-id="a2405-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="a2405-288">Configurare l'autenticazione della porta</span><span class="sxs-lookup"><span data-stu-id="a2405-288">Configure bearer authentication</span></span>

<span data-ttu-id="a2405-289">Per fornire i dati di autenticazione insieme alle richieste SignalR, usare l'opzione `AccessTokenProvider` (`accessTokenFactory` in JavaScript) per specificare una funzione che restituisce il token di accesso desiderato.</span><span class="sxs-lookup"><span data-stu-id="a2405-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="a2405-290">Nel client .NET questo token di accesso viene passato come token "autenticazione di connessione" HTTP (usando l'intestazione `Authorization` con un tipo di `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="a2405-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="a2405-291">Nel client JavaScript, il token di accesso viene usato come token di connessione, **ad eccezione** dei casi in cui le API del browser limitano la possibilità di applicare intestazioni (in particolare, nelle richieste di eventi inviati dal server e WebSocket).</span><span class="sxs-lookup"><span data-stu-id="a2405-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="a2405-292">In questi casi, il token di accesso viene fornito come valore stringa di query `access_token`.</span><span class="sxs-lookup"><span data-stu-id="a2405-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="a2405-293">Nel client .NET è possibile specificare l'opzione `AccessTokenProvider` usando il delegato options in `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="a2405-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="a2405-294">Nel client JavaScript, il token di accesso viene configurato impostando il campo `accessTokenFactory` nell'oggetto opzioni in `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="a2405-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="a2405-295">Nel client Java SignalR è possibile configurare un bearer token da usare per l'autenticazione fornendo una factory del token di accesso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="a2405-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="a2405-296">Usare [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire una [RxJava](https://github.com/ReactiveX/RxJava) [stringa a\<> singola](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="a2405-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="a2405-297">Con una chiamata a [Single. rinvia](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.</span><span class="sxs-lookup"><span data-stu-id="a2405-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="a2405-298">Configurare le opzioni di timeout e Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="a2405-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="a2405-299">Sono disponibili opzioni aggiuntive per la configurazione del comportamento di timeout e Keep-Alive nell'oggetto `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="a2405-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="a2405-300">.NET</span><span class="sxs-lookup"><span data-stu-id="a2405-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a2405-301">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-301">Option</span></span> | <span data-ttu-id="a2405-302">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-302">Default value</span></span> | <span data-ttu-id="a2405-303">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="a2405-304">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-305">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-305">Timeout for server activity.</span></span> <span data-ttu-id="a2405-306">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a2405-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a2405-307">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-308">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="a2405-309">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-309">15 seconds</span></span> | <span data-ttu-id="a2405-310">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="a2405-311">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a2405-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a2405-312">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-313">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a2405-314">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-314">15 seconds</span></span> | <span data-ttu-id="a2405-315">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a2405-316">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="a2405-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a2405-317">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="a2405-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="a2405-318">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="a2405-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a2405-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2405-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a2405-320">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-320">Option</span></span> | <span data-ttu-id="a2405-321">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-321">Default value</span></span> | <span data-ttu-id="a2405-322">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="a2405-323">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-324">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-324">Timeout for server activity.</span></span> <span data-ttu-id="a2405-325">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="a2405-326">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-327">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="a2405-328">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="a2405-329">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a2405-330">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="a2405-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a2405-331">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="a2405-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a2405-332">Java</span><span class="sxs-lookup"><span data-stu-id="a2405-332">Java</span></span>](#tab/java)

| <span data-ttu-id="a2405-333">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-333">Option</span></span> | <span data-ttu-id="a2405-334">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-334">Default value</span></span> | <span data-ttu-id="a2405-335">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="a2405-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="a2405-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="a2405-337">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-338">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-338">Timeout for server activity.</span></span> <span data-ttu-id="a2405-339">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="a2405-340">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-341">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="a2405-342">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-342">15 seconds</span></span> | <span data-ttu-id="a2405-343">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="a2405-344">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="a2405-345">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-346">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="a2405-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="a2405-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="a2405-348">15 secondi (15.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="a2405-349">Determina l'intervallo in corrispondenza del quale il client invia messaggi ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a2405-350">L'invio di messaggi dal client Reimposta il timer all'inizio dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="a2405-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a2405-351">Se il client non ha inviato un messaggio nel `ClientTimeoutInterval` impostato sul server, il server considera il client disconnesso.</span><span class="sxs-lookup"><span data-stu-id="a2405-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="a2405-352">.NET</span><span class="sxs-lookup"><span data-stu-id="a2405-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a2405-353">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-353">Option</span></span> | <span data-ttu-id="a2405-354">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-354">Default value</span></span> | <span data-ttu-id="a2405-355">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="a2405-356">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-357">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-357">Timeout for server activity.</span></span> <span data-ttu-id="a2405-358">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a2405-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a2405-359">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-360">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="a2405-361">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-361">15 seconds</span></span> | <span data-ttu-id="a2405-362">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="a2405-363">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `Closed` (`onclose` in JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a2405-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a2405-364">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-365">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="a2405-366">Nel client .NET i valori di timeout vengono specificati come valori `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="a2405-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a2405-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2405-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a2405-368">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-368">Option</span></span> | <span data-ttu-id="a2405-369">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-369">Default value</span></span> | <span data-ttu-id="a2405-370">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="a2405-371">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-372">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-372">Timeout for server activity.</span></span> <span data-ttu-id="a2405-373">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="a2405-374">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-375">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a2405-376">Java</span><span class="sxs-lookup"><span data-stu-id="a2405-376">Java</span></span>](#tab/java)

| <span data-ttu-id="a2405-377">Opzione</span><span class="sxs-lookup"><span data-stu-id="a2405-377">Option</span></span> | <span data-ttu-id="a2405-378">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-378">Default value</span></span> | <span data-ttu-id="a2405-379">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a2405-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="a2405-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="a2405-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="a2405-381">30 secondi (30.000 millisecondi)</span><span class="sxs-lookup"><span data-stu-id="a2405-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a2405-382">Timeout per l'attività del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-382">Timeout for server activity.</span></span> <span data-ttu-id="a2405-383">Se il server non ha inviato un messaggio in questo intervallo, il client considera il server disconnesso e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="a2405-384">Questo valore deve essere sufficientemente grande da consentire l'invio del messaggio ping dal server **e** la ricezione da parte del client entro l'intervallo di timeout.</span><span class="sxs-lookup"><span data-stu-id="a2405-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a2405-385">Il valore consigliato è un numero almeno il doppio del valore `KeepAliveInterval` del server, per consentire l'arrivo dei ping.</span><span class="sxs-lookup"><span data-stu-id="a2405-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="a2405-386">15 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-386">15 seconds</span></span> | <span data-ttu-id="a2405-387">Timeout per l'handshake iniziale del server.</span><span class="sxs-lookup"><span data-stu-id="a2405-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="a2405-388">Se il server non invia una risposta di handshake in questo intervallo, il client Annulla l'handshake e attiva l'evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="a2405-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="a2405-389">Si tratta di un'impostazione avanzata che deve essere modificata solo se si verificano errori di timeout dell'handshake a causa di una latenza di rete grave.</span><span class="sxs-lookup"><span data-stu-id="a2405-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a2405-390">Per informazioni dettagliate sul processo di handshake, vedere la [specifica del protocollo dell'hub SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a2405-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="a2405-391">Configurare opzioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a2405-391">Configure additional options</span></span>

<span data-ttu-id="a2405-392">È possibile configurare opzioni aggiuntive nel metodo `WithUrl` (`withUrl` in JavaScript) in `HubConnectionBuilder` o nelle varie API di configurazione di `HttpHubConnectionBuilder` nel client Java:</span><span class="sxs-lookup"><span data-stu-id="a2405-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="a2405-393">.NET</span><span class="sxs-lookup"><span data-stu-id="a2405-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a2405-394">Opzione .NET</span><span class="sxs-lookup"><span data-stu-id="a2405-394">.NET Option</span></span> |  <span data-ttu-id="a2405-395">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="a2405-395">Default value</span></span> | <span data-ttu-id="a2405-396">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="a2405-397">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="a2405-398">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a2405-399">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="a2405-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a2405-400">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="a2405-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="a2405-401">Empty</span><span class="sxs-lookup"><span data-stu-id="a2405-401">Empty</span></span> | <span data-ttu-id="a2405-402">Raccolta di certificati TLS da inviare per autenticare le richieste.</span><span class="sxs-lookup"><span data-stu-id="a2405-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="a2405-403">Empty</span><span class="sxs-lookup"><span data-stu-id="a2405-403">Empty</span></span> | <span data-ttu-id="a2405-404">Raccolta di cookie HTTP da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="a2405-405">Empty</span><span class="sxs-lookup"><span data-stu-id="a2405-405">Empty</span></span> | <span data-ttu-id="a2405-406">Credenziali da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="a2405-407">5 secondi</span><span class="sxs-lookup"><span data-stu-id="a2405-407">5 seconds</span></span> | <span data-ttu-id="a2405-408">Solo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-408">WebSockets only.</span></span> <span data-ttu-id="a2405-409">Quantità massima di tempo di attesa del client dopo la chiusura del server per confermare la richiesta di chiusura.</span><span class="sxs-lookup"><span data-stu-id="a2405-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="a2405-410">Se il server non riconosce la chiusura entro questo intervallo di tempo, il client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="a2405-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="a2405-411">Empty</span><span class="sxs-lookup"><span data-stu-id="a2405-411">Empty</span></span> | <span data-ttu-id="a2405-412">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="a2405-413">Delegato che può essere utilizzato per configurare o sostituire il `HttpMessageHandler` utilizzato per inviare le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="a2405-414">Non usato per le connessioni WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="a2405-415">Questo delegato deve restituire un valore non null e riceve il valore predefinito come parametro.</span><span class="sxs-lookup"><span data-stu-id="a2405-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="a2405-416">Modificare le impostazioni del valore predefinito e restituirlo oppure restituire una nuova istanza di `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="a2405-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="a2405-417">**Quando si sostituisce il gestore, assicurarsi di copiare le impostazioni che si desidera gestire dal gestore fornito. in caso contrario, le opzioni configurate (ad esempio cookie e intestazioni) non verranno applicate al nuovo gestore.**</span><span class="sxs-lookup"><span data-stu-id="a2405-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="a2405-418">Proxy HTTP da usare per l'invio di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="a2405-419">Impostare questo valore booleano per inviare le credenziali predefinite per le richieste HTTP e WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a2405-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="a2405-420">Questo consente l'uso dell'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a2405-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="a2405-421">Delegato che può essere usato per configurare opzioni WebSocket aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="a2405-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="a2405-422">Riceve un'istanza di [Metodo ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) che può essere utilizzata per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="a2405-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a2405-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2405-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a2405-424">Opzione JavaScript</span><span class="sxs-lookup"><span data-stu-id="a2405-424">JavaScript Option</span></span> | <span data-ttu-id="a2405-425">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-425">Default Value</span></span> | <span data-ttu-id="a2405-426">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="a2405-427">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="a2405-428">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a2405-429">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="a2405-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a2405-430">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="a2405-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a2405-431">Java</span><span class="sxs-lookup"><span data-stu-id="a2405-431">Java</span></span>](#tab/java)

| <span data-ttu-id="a2405-432">Opzione Java</span><span class="sxs-lookup"><span data-stu-id="a2405-432">Java Option</span></span> | <span data-ttu-id="a2405-433">Default Value</span><span class="sxs-lookup"><span data-stu-id="a2405-433">Default Value</span></span> | <span data-ttu-id="a2405-434">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="a2405-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="a2405-435">Funzione che restituisce una stringa fornita come token di autenticazione di un portatore nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="a2405-436">Impostare questa impostazione su `true` per ignorare il passaggio di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="a2405-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a2405-437">**Supportato solo quando il trasporto WebSocket è l'unico trasporto abilitato**.</span><span class="sxs-lookup"><span data-stu-id="a2405-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a2405-438">Questa impostazione non può essere abilitata quando si usa il servizio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="a2405-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="a2405-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="a2405-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="a2405-440">Empty</span><span class="sxs-lookup"><span data-stu-id="a2405-440">Empty</span></span> | <span data-ttu-id="a2405-441">Mappa di intestazioni HTTP aggiuntive da inviare con ogni richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a2405-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="a2405-442">Nel client .NET queste opzioni possono essere modificate dal delegato options fornito a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="a2405-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="a2405-443">Nel client JavaScript queste opzioni possono essere fornite in un oggetto JavaScript fornito a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="a2405-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="a2405-444">Nel client Java queste opzioni possono essere configurate con i metodi del `HttpHubConnectionBuilder` restituiti dal `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="a2405-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="a2405-445">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a2405-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
