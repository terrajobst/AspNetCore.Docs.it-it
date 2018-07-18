---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: tdykstra
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095008"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="39e4a-103">Differenze tra SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="39e4a-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="39e4a-104">ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="39e4a-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="39e4a-105">Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="39e4a-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="39e4a-106">Differenze di funzionalità</span><span class="sxs-lookup"><span data-stu-id="39e4a-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="39e4a-107">Connessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="39e4a-107">Automatic reconnects</span></span>

<span data-ttu-id="39e4a-108">Le connessioni automatica non sono più supportate.</span><span class="sxs-lookup"><span data-stu-id="39e4a-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="39e4a-109">SignalR tentato in precedenza, riconnettersi al server se la connessione è stata eliminata.</span><span class="sxs-lookup"><span data-stu-id="39e4a-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="39e4a-110">Se il client viene disconnesso ora l'utente deve avviare una nuova connessione in modo esplicito se si vuole riconnettere.</span><span class="sxs-lookup"><span data-stu-id="39e4a-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="39e4a-111">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="39e4a-111">Protocol support</span></span>

<span data-ttu-id="39e4a-112">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="39e4a-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="39e4a-113">Inoltre, è possibile creare i protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="39e4a-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="39e4a-114">Comprendere le differenze tra il server</span><span class="sxs-lookup"><span data-stu-id="39e4a-114">Differences on the server</span></span>

<span data-ttu-id="39e4a-115">Le librerie sul lato server di SignalR sono incluse nel `Microsoft.AspNetCore.App` che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per i progetti sia Razor MVC.</span><span class="sxs-lookup"><span data-stu-id="39e4a-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="39e4a-116">SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="39e4a-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="39e4a-117">Per configurare il routing, eseguire il mapping di route per gli hub all'interno di `UseSignalR` chiamata al metodo il `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="39e4a-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="39e4a-118">Sessioni permanenti ora richiesto</span><span class="sxs-lookup"><span data-stu-id="39e4a-118">Sticky sessions now required</span></span>

<span data-ttu-id="39e4a-119">A causa di scalabilità orizzontale come ha funzionato nelle versioni precedenti di SignalR, i client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm.</span><span class="sxs-lookup"><span data-stu-id="39e4a-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="39e4a-120">A causa di modifiche per il modello di scalabilità orizzontale, nonché non sono supportate le connessioni, ciò non è più supportata.</span><span class="sxs-lookup"><span data-stu-id="39e4a-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="39e4a-121">A questo punto, dopo che il client si connette al server deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="39e4a-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="39e4a-122">Hub singolo per ogni connessione</span><span class="sxs-lookup"><span data-stu-id="39e4a-122">Single hub per connection</span></span>

<span data-ttu-id="39e4a-123">In ASP.NET Core SignalR, il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="39e4a-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="39e4a-124">Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="39e4a-125">Flusso</span><span class="sxs-lookup"><span data-stu-id="39e4a-125">Streaming</span></span>

<span data-ttu-id="39e4a-126">Supporta ora SignalR [dati in streaming](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="39e4a-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="39e4a-127">Stato</span><span class="sxs-lookup"><span data-stu-id="39e4a-127">State</span></span>

<span data-ttu-id="39e4a-128">La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="39e4a-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="39e4a-129">Nel momento in cui non vi è alcun equivalente di proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="39e4a-130">Comprendere le differenze tra il client</span><span class="sxs-lookup"><span data-stu-id="39e4a-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="39e4a-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="39e4a-131">TypeScript</span></span>

<span data-ttu-id="39e4a-132">La versione di ASP.NET Core di SignalR è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="39e4a-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="39e4a-133">È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="39e4a-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="39e4a-134">Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="39e4a-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="39e4a-135">Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39e4a-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="39e4a-136">Per le versioni di base, il [ @aspnet/signalr pacchetto npm](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39e4a-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="39e4a-137">Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="39e4a-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="39e4a-138">Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="39e4a-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="39e4a-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="39e4a-139">jQuery</span></span>

<span data-ttu-id="39e4a-140">La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="39e4a-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="39e4a-141">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="39e4a-141">JavaScript client method syntax</span></span>

<span data-ttu-id="39e4a-142">La sintassi JavaScript è stato modificato dalla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="39e4a-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="39e4a-143">Invece di usare la `$connection` dell'oggetto, creare una connessione usando il `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="39e4a-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="39e4a-144">Usare `connection.on` per specificare metodi di client può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="39e4a-145">Dopo aver creato il metodo di client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="39e4a-146">Catena un `catch` metodo per accedere o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="39e4a-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="39e4a-147">Proxy di hub</span><span class="sxs-lookup"><span data-stu-id="39e4a-147">Hub proxies</span></span>

<span data-ttu-id="39e4a-148">Vengono generati non più automaticamente i proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="39e4a-149">Il nome del metodo viene invece passato nella `invoke` API sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="39e4a-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="39e4a-150">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="39e4a-150">.NET and other clients</span></span>

<span data-ttu-id="39e4a-151">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="39e4a-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="39e4a-152">Usare il `HubConnectionBuilder` per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="39e4a-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="39e4a-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="39e4a-153">Additional resources</span></span>

* [<span data-ttu-id="39e4a-154">Hub</span><span class="sxs-lookup"><span data-stu-id="39e4a-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="39e4a-155">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="39e4a-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="39e4a-156">Client .NET</span><span class="sxs-lookup"><span data-stu-id="39e4a-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="39e4a-157">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="39e4a-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
