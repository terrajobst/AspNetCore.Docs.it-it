---
title: Differenze tra SignalR e dei componenti di base di ASP.NET SignalR
author: rachelappel
description: Differenze tra SignalR e dei componenti di base di ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090064"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="3006f-103">Differenze tra SignalR e dei componenti di base di ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="3006f-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="3006f-104">SignalR ASP.NET Core non è compatibile con client e server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="3006f-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="3006f-105">In questo articolo illustra in dettaglio le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="3006f-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="3006f-106">Differenze di funzionalità</span><span class="sxs-lookup"><span data-stu-id="3006f-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="3006f-107">Riconnessioni automatico</span><span class="sxs-lookup"><span data-stu-id="3006f-107">Automatic reconnects</span></span>

<span data-ttu-id="3006f-108">Riconnessioni automatico non sono più supportate.</span><span class="sxs-lookup"><span data-stu-id="3006f-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="3006f-109">SignalR tentato in precedenza, riconnettersi al server se la connessione è stata eliminata.</span><span class="sxs-lookup"><span data-stu-id="3006f-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="3006f-110">Se il client è disconnesso l'utente deve avviare una nuova connessione in modo esplicito se desiderano ristabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="3006f-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="3006f-111">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="3006f-111">Protocol support</span></span>

<span data-ttu-id="3006f-112">ASP.NET SignalR Core supporta JSON, nonché un nuovo protocollo binario in base alle [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="3006f-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="3006f-113">Inoltre, è possibile creare protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="3006f-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="3006f-114">Differenze nel server</span><span class="sxs-lookup"><span data-stu-id="3006f-114">Differences on the server</span></span>

<span data-ttu-id="3006f-115">Le librerie sul lato server di SignalR sono inclusi nel `Microsoft.AspNetCore.App` che fa parte del pacchetto il **applicazione Web di ASP.NET Core** modello per i progetti sia Razor e MVC.</span><span class="sxs-lookup"><span data-stu-id="3006f-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="3006f-116">SignalR è un middleware di ASP.NET Core, pertanto deve essere configurata tramite la chiamata `AddSignalR` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3006f-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="3006f-117">Per configurare il routing, eseguire il mapping delle route agli hub all'interno di `UseSignalR` chiamata al metodo di `Startup.Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="3006f-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="3006f-118">Sessioni permanenti ora richiesto</span><span class="sxs-lookup"><span data-stu-id="3006f-118">Sticky sessions now required</span></span>

<span data-ttu-id="3006f-119">A causa di scalabilità orizzontale come funziona nelle versioni precedenti di SignalR client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm.</span><span class="sxs-lookup"><span data-stu-id="3006f-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="3006f-120">A causa di modifiche per il modello di scalabilità orizzontale, così come non supporta riconnessioni, ciò non è più supportata.</span><span class="sxs-lookup"><span data-stu-id="3006f-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="3006f-121">A questo punto, dopo che il client si connette al server deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="3006f-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="3006f-122">Singolo hub per ogni connessione</span><span class="sxs-lookup"><span data-stu-id="3006f-122">Single hub per connection</span></span>

<span data-ttu-id="3006f-123">In ASP.NET SignalR Core, il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="3006f-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="3006f-124">Le connessioni vengono effettuate direttamente a un singolo hub, anziché una singola connessione usato per condividere l'accesso agli hub più.</span><span class="sxs-lookup"><span data-stu-id="3006f-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="3006f-125">Flusso</span><span class="sxs-lookup"><span data-stu-id="3006f-125">Streaming</span></span>

<span data-ttu-id="3006f-126">Supporta ora SignalR [flusso di dati](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="3006f-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="3006f-127">Stato</span><span class="sxs-lookup"><span data-stu-id="3006f-127">State</span></span>

<span data-ttu-id="3006f-128">La possibilità di passare uno stato arbitrario tra client e l'hub (spesso chiamati HubState) è stato rimosso, oltre al supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="3006f-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="3006f-129">Al momento non sono alcun equivalente di proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="3006f-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="3006f-130">Differenze nel client</span><span class="sxs-lookup"><span data-stu-id="3006f-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="3006f-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="3006f-131">TypeScript</span></span>

<span data-ttu-id="3006f-132">La versione di ASP.NET Core di SignalR è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="3006f-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="3006f-133">È possibile scrivere in JavaScript o TypeScript quando si utilizza il [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="3006f-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="3006f-134">Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="3006f-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="3006f-135">Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3006f-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="3006f-136">Per le versioni dei componenti di base, il [ @aspnet/signalr pacchetto npm](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3006f-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="3006f-137">Questo pacchetto non è incluso nel **applicazione Web di ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="3006f-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="3006f-138">Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="3006f-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="3006f-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="3006f-139">jQuery</span></span>

<span data-ttu-id="3006f-140">La dipendenza da jQuery è stato rimosso, tuttavia, i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="3006f-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="3006f-141">Sintassi del metodo di client JavaScript</span><span class="sxs-lookup"><span data-stu-id="3006f-141">JavaScript client method syntax</span></span>

<span data-ttu-id="3006f-142">La sintassi JavaScript è stato modificato dalla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3006f-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="3006f-143">Invece di usare il `$connection` dell'oggetto, creare una connessione utilizzando il `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="3006f-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="3006f-144">Utilizzare `connection.on` per specificare i metodi di client che può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="3006f-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="3006f-145">Dopo aver creato il metodo di client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="3006f-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="3006f-146">Catena un `catch` metodo per accedere o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="3006f-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="3006f-147">Proxy dell'hub</span><span class="sxs-lookup"><span data-stu-id="3006f-147">Hub proxies</span></span>

<span data-ttu-id="3006f-148">Vengono generati non più automaticamente i proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="3006f-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="3006f-149">Al contrario, il nome del metodo viene passato il `invoke` API sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="3006f-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="3006f-150">.NET e gli altri client</span><span class="sxs-lookup"><span data-stu-id="3006f-150">.NET and other clients</span></span>

<span data-ttu-id="3006f-151">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET SignalR Core.</span><span class="sxs-lookup"><span data-stu-id="3006f-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="3006f-152">Utilizzare il `HubConnectionBuilder` per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="3006f-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="3006f-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3006f-153">Additional resources</span></span>

* [<span data-ttu-id="3006f-154">Hub</span><span class="sxs-lookup"><span data-stu-id="3006f-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3006f-155">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="3006f-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3006f-156">Client .NET</span><span class="sxs-lookup"><span data-stu-id="3006f-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3006f-157">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="3006f-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
