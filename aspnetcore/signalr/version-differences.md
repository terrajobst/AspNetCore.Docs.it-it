---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: tdykstra
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 8f07647959b6ef815eed599703bdb1bfb446572f
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505752"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="c99bd-103">Differenze tra ASP.NET SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="c99bd-104">ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="c99bd-105">Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="c99bd-106">Come identificare la versione di SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="c99bd-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-107">ASP.NET SignalR</span></span> | <span data-ttu-id="c99bd-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="c99bd-109">Pacchetto NuGet server</span><span class="sxs-lookup"><span data-stu-id="c99bd-109">Server NuGet Package</span></span> | [<span data-ttu-id="c99bd-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="c99bd-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c99bd-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="c99bd-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="c99bd-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="c99bd-113">Pacchetti NuGet del client</span><span class="sxs-lookup"><span data-stu-id="c99bd-113">Client NuGet Packages</span></span> | [<span data-ttu-id="c99bd-114">ASPNET</span><span class="sxs-lookup"><span data-stu-id="c99bd-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="c99bd-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="c99bd-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="c99bd-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="c99bd-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="c99bd-117">Client npm, pacchetti</span><span class="sxs-lookup"><span data-stu-id="c99bd-117">Client npm Package</span></span> | [<span data-ttu-id="c99bd-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="c99bd-119">Tipo di App server</span><span class="sxs-lookup"><span data-stu-id="c99bd-119">Server App Type</span></span> | <span data-ttu-id="c99bd-120">ASP.NET (System. Web) o Self-hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="c99bd-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="c99bd-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c99bd-121">ASP.NET Core</span></span> |
| <span data-ttu-id="c99bd-122">Nelle piattaforme Server supportati</span><span class="sxs-lookup"><span data-stu-id="c99bd-122">Supported Server Platforms</span></span> | <span data-ttu-id="c99bd-123">.NET framework 4.5 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c99bd-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="c99bd-124">.NET Framework 4.6.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="c99bd-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="c99bd-125">.NET core 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="c99bd-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="c99bd-126">Differenze di funzionalità</span><span class="sxs-lookup"><span data-stu-id="c99bd-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="c99bd-127">Connessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="c99bd-127">Automatic reconnects</span></span>

<span data-ttu-id="c99bd-128">Le connessioni automatica non sono supportate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="c99bd-129">Se il client viene disconnesso, l'utente deve iniziare in modo esplicito una nuova connessione se si vuole riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="c99bd-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="c99bd-130">In ASP.NET SignalR, SignalR tenta di riconnettersi al server se la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="c99bd-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="c99bd-131">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="c99bd-131">Protocol support</span></span>

<span data-ttu-id="c99bd-132">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="c99bd-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="c99bd-133">Inoltre, è possibile creare i protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c99bd-133">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="c99bd-134">Trasporti</span><span class="sxs-lookup"><span data-stu-id="c99bd-134">Transports</span></span>

<span data-ttu-id="c99bd-135">Il trasporto Forever Frame non è supportato in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-135">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="c99bd-136">Comprendere le differenze tra il server</span><span class="sxs-lookup"><span data-stu-id="c99bd-136">Differences on the server</span></span>

<span data-ttu-id="c99bd-137">Le librerie sul lato server ASP.NET Core SignalR sono incluse nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per Razor e MVC progetti.</span><span class="sxs-lookup"><span data-stu-id="c99bd-137">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="c99bd-138">ASP.NET Core SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c99bd-138">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="c99bd-139">Per configurare il routing, eseguire il mapping di route per gli hub all'interno di [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chiamata al metodo il `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c99bd-139">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="c99bd-140">Sessioni permanenti</span><span class="sxs-lookup"><span data-stu-id="c99bd-140">Sticky sessions</span></span>

<span data-ttu-id="c99bd-141">Il modello di scalabilità orizzontale ASP.NET SignalR consente ai client di riconnettersi e inviare messaggi a qualsiasi server nella farm.</span><span class="sxs-lookup"><span data-stu-id="c99bd-141">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="c99bd-142">In ASP.NET Core SignalR, il client deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="c99bd-142">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="c99bd-143">Per la scalabilità orizzontale con Redis, che significa che le sessioni permanenti sono necessari.</span><span class="sxs-lookup"><span data-stu-id="c99bd-143">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="c99bd-144">Per l'uso di scalabilità orizzontale [servizio Azure SignalR](/azure/azure-signalr/), le sessioni permanenti non sono necessarie perché il servizio gestisce le connessioni ai client.</span><span class="sxs-lookup"><span data-stu-id="c99bd-144">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="c99bd-145">Hub singolo per ogni connessione</span><span class="sxs-lookup"><span data-stu-id="c99bd-145">Single hub per connection</span></span>

<span data-ttu-id="c99bd-146">In ASP.NET Core SignalR, il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="c99bd-146">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="c99bd-147">Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-147">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="c99bd-148">Flusso</span><span class="sxs-lookup"><span data-stu-id="c99bd-148">Streaming</span></span>

<span data-ttu-id="c99bd-149">ASP.NET Core SignalR ora supporta [dati in streaming](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="c99bd-149">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="c99bd-150">Stato</span><span class="sxs-lookup"><span data-stu-id="c99bd-150">State</span></span>

<span data-ttu-id="c99bd-151">La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="c99bd-151">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="c99bd-152">Nel momento in cui non vi è alcun equivalente di proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-152">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="globalhost"></a><span data-ttu-id="c99bd-153">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="c99bd-153">GlobalHost</span></span>

<span data-ttu-id="c99bd-154">ASP.NET Core con inserimento di dipendenze () incorporato in framework.</span><span class="sxs-lookup"><span data-stu-id="c99bd-154">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="c99bd-155">I servizi possono utilizzare l'inserimento delle dipendenze per accedere la [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="c99bd-155">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="c99bd-156">Il `GlobalHost` oggetto che viene utilizzato in ASP.NET SignalR per ottenere un `HubContext` non esiste in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-156">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="c99bd-157">A HubPipeline</span><span class="sxs-lookup"><span data-stu-id="c99bd-157">HubPipeline</span></span>

<span data-ttu-id="c99bd-158">ASP.NET Core SignalR non dispone di supporto per `HubPipeline` moduli.</span><span class="sxs-lookup"><span data-stu-id="c99bd-158">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="c99bd-159">Comprendere le differenze tra il client</span><span class="sxs-lookup"><span data-stu-id="c99bd-159">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="c99bd-160">TypeScript</span><span class="sxs-lookup"><span data-stu-id="c99bd-160">TypeScript</span></span>

<span data-ttu-id="c99bd-161">Il client di ASP.NET Core SignalR è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="c99bd-161">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="c99bd-162">È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="c99bd-162">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="c99bd-163">Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="c99bd-163">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="c99bd-164">Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c99bd-164">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="c99bd-165">Per le versioni di base, il [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacchetto npm contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c99bd-165">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="c99bd-166">Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="c99bd-166">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c99bd-167">Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="c99bd-167">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="c99bd-168">jQuery</span><span class="sxs-lookup"><span data-stu-id="c99bd-168">jQuery</span></span>

<span data-ttu-id="c99bd-169">La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="c99bd-169">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="c99bd-170">Supporto di Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c99bd-170">Internet Explorer support</span></span>

<span data-ttu-id="c99bd-171">ASP.NET Core SignalR richiede Microsoft Internet Explorer 11 o versione successiva (ASP.NET SignalR supportato Microsoft Internet Explorer 8 e versioni successive).</span><span class="sxs-lookup"><span data-stu-id="c99bd-171">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="c99bd-172">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c99bd-172">JavaScript client method syntax</span></span>

<span data-ttu-id="c99bd-173">La sintassi JavaScript è stato modificato dalla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-173">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="c99bd-174">Invece di usare la `$connection` dell'oggetto, creare una connessione usando la [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="c99bd-174">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="c99bd-175">Usare la [su](/javascript/api/@aspnet/signalr/HubConnection#on) metodo per specificare metodi di client può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-175">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="c99bd-176">Dopo aver creato il metodo di client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-176">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="c99bd-177">Catena di una [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodo per accedere o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="c99bd-177">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="c99bd-178">Proxy di hub</span><span class="sxs-lookup"><span data-stu-id="c99bd-178">Hub proxies</span></span>

<span data-ttu-id="c99bd-179">Vengono generati non più automaticamente i proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-179">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="c99bd-180">Il nome del metodo viene invece passato nella [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="c99bd-180">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="c99bd-181">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="c99bd-181">.NET and other clients</span></span>

<span data-ttu-id="c99bd-182">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c99bd-182">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="c99bd-183">Usare la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="c99bd-183">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="c99bd-184">Differenze di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="c99bd-184">Scaleout differences</span></span>

<span data-ttu-id="c99bd-185">ASP.NET SignalR supporta SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="c99bd-185">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="c99bd-186">ASP.NET Core SignalR supporta servizio Azure SignalR e Redis.</span><span class="sxs-lookup"><span data-stu-id="c99bd-186">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="c99bd-187">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c99bd-187">ASP.NET</span></span>

* [<span data-ttu-id="c99bd-188">Scalabilità orizzontale di SignalR con il Bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="c99bd-188">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="c99bd-189">Scalabilità orizzontale di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="c99bd-189">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="c99bd-190">Scalabilità orizzontale di SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="c99bd-190">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="c99bd-191">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c99bd-191">ASP.NET Core</span></span>

* [<span data-ttu-id="c99bd-192">Servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="c99bd-192">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="c99bd-193">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c99bd-193">Additional resources</span></span>

* [<span data-ttu-id="c99bd-194">Hub</span><span class="sxs-lookup"><span data-stu-id="c99bd-194">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c99bd-195">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c99bd-195">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c99bd-196">Client .NET</span><span class="sxs-lookup"><span data-stu-id="c99bd-196">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c99bd-197">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="c99bd-197">Supported platforms</span></span>](xref:signalr/supported-platforms)
