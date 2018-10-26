---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: tdykstra
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 4ac7952f26500285fc1c8f9453feb3ea8b33851a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089828"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="a0472-103">Differenze tra ASP.NET SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="a0472-104">ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0472-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="a0472-105">Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0472-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="a0472-106">Come identificare la versione di SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="a0472-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-107">ASP.NET SignalR</span></span> | <span data-ttu-id="a0472-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="a0472-109">Pacchetto NuGet server</span><span class="sxs-lookup"><span data-stu-id="a0472-109">Server NuGet Package</span></span> | [<span data-ttu-id="a0472-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="a0472-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="a0472-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="a0472-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="a0472-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="a0472-113">Pacchetti NuGet del client</span><span class="sxs-lookup"><span data-stu-id="a0472-113">Client NuGet Packages</span></span> | [<span data-ttu-id="a0472-114">ASPNET</span><span class="sxs-lookup"><span data-stu-id="a0472-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="a0472-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="a0472-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="a0472-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="a0472-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="a0472-117">Client npm, pacchetti</span><span class="sxs-lookup"><span data-stu-id="a0472-117">Client npm Package</span></span> | [<span data-ttu-id="a0472-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="a0472-119">Tipo di App server</span><span class="sxs-lookup"><span data-stu-id="a0472-119">Server App Type</span></span> | <span data-ttu-id="a0472-120">ASP.NET (System. Web) o Self-hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="a0472-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="a0472-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0472-121">ASP.NET Core</span></span> |
| <span data-ttu-id="a0472-122">Nelle piattaforme Server supportati</span><span class="sxs-lookup"><span data-stu-id="a0472-122">Supported Server Platforms</span></span> | <span data-ttu-id="a0472-123">.NET framework 4.5 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="a0472-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="a0472-124">.NET Framework 4.6.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="a0472-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="a0472-125">.NET core 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="a0472-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="a0472-126">Differenze di funzionalità</span><span class="sxs-lookup"><span data-stu-id="a0472-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="a0472-127">Connessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="a0472-127">Automatic reconnects</span></span>

<span data-ttu-id="a0472-128">Le connessioni automatica non sono supportate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0472-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="a0472-129">Se il client viene disconnesso, l'utente deve iniziare in modo esplicito una nuova connessione se si vuole riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="a0472-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="a0472-130">In ASP.NET SignalR, SignalR tenta di riconnettersi al server se la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="a0472-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="a0472-131">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="a0472-131">Protocol support</span></span>

<span data-ttu-id="a0472-132">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="a0472-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="a0472-133">Inoltre, è possibile creare i protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="a0472-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="a0472-134">Comprendere le differenze tra il server</span><span class="sxs-lookup"><span data-stu-id="a0472-134">Differences on the server</span></span>

<span data-ttu-id="a0472-135">Le librerie sul lato server ASP.NET Core SignalR sono incluse nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per Razor e MVC progetti.</span><span class="sxs-lookup"><span data-stu-id="a0472-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="a0472-136">ASP.NET Core SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a0472-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="a0472-137">Per configurare il routing, eseguire il mapping di route per gli hub all'interno di [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chiamata al metodo il `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="a0472-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="a0472-138">Sessioni permanenti ora richiesto</span><span class="sxs-lookup"><span data-stu-id="a0472-138">Sticky sessions now required</span></span>

<span data-ttu-id="a0472-139">A causa di scalabilità orizzontale come ha funzionato in ASP.NET SignalR, i client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm.</span><span class="sxs-lookup"><span data-stu-id="a0472-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="a0472-140">A causa di modifiche per il modello di scalabilità orizzontale, nonché non sono supportate le connessioni, ciò non è più supportata.</span><span class="sxs-lookup"><span data-stu-id="a0472-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="a0472-141">Dopo che il client si connette al server, deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="a0472-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="a0472-142">Hub singolo per ogni connessione</span><span class="sxs-lookup"><span data-stu-id="a0472-142">Single hub per connection</span></span>

<span data-ttu-id="a0472-143">In ASP.NET Core SignalR, il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="a0472-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="a0472-144">Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="a0472-145">Flusso</span><span class="sxs-lookup"><span data-stu-id="a0472-145">Streaming</span></span>

<span data-ttu-id="a0472-146">ASP.NET Core SignalR ora supporta [dati in streaming](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="a0472-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="a0472-147">State</span><span class="sxs-lookup"><span data-stu-id="a0472-147">State</span></span>

<span data-ttu-id="a0472-148">La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="a0472-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="a0472-149">Nel momento in cui non vi è alcun equivalente di proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="a0472-150">Comprendere le differenze tra il client</span><span class="sxs-lookup"><span data-stu-id="a0472-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="a0472-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="a0472-151">TypeScript</span></span>

<span data-ttu-id="a0472-152">Il client di ASP.NET Core SignalR è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="a0472-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="a0472-153">È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="a0472-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="a0472-154">Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a0472-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="a0472-155">Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0472-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="a0472-156">Per le versioni di base, il [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacchetto npm contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a0472-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="a0472-157">Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="a0472-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a0472-158">Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="a0472-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="a0472-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="a0472-159">jQuery</span></span>

<span data-ttu-id="a0472-160">La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="a0472-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="a0472-161">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a0472-161">JavaScript client method syntax</span></span>

<span data-ttu-id="a0472-162">La sintassi JavaScript è stato modificato dalla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0472-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="a0472-163">Invece di usare la `$connection` dell'oggetto, creare una connessione usando la [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="a0472-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="a0472-164">Usare la [su](/javascript/api/@aspnet/signalr/HubConnection#on) metodo per specificare metodi di client può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="a0472-165">Dopo aver creato il metodo di client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="a0472-166">Catena di una [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodo per accedere o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="a0472-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="a0472-167">Proxy di hub</span><span class="sxs-lookup"><span data-stu-id="a0472-167">Hub proxies</span></span>

<span data-ttu-id="a0472-168">Vengono generati non più automaticamente i proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="a0472-169">Il nome del metodo viene invece passato nella [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="a0472-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="a0472-170">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="a0472-170">.NET and other clients</span></span>

<span data-ttu-id="a0472-171">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0472-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="a0472-172">Usare la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="a0472-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="a0472-173">Differenze di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="a0472-173">Scaleout differences</span></span>

<span data-ttu-id="a0472-174">ASP.NET SignalR supporta SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="a0472-174">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="a0472-175">ASP.NET Core SignalR supporta servizio Azure SignalR e Redis.</span><span class="sxs-lookup"><span data-stu-id="a0472-175">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="a0472-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a0472-176">ASP.NET</span></span>

* [<span data-ttu-id="a0472-177">Scalabilità orizzontale di SignalR con il Bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="a0472-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="a0472-178">Scalabilità orizzontale di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="a0472-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="a0472-179">Scalabilità orizzontale di SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="a0472-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="a0472-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0472-180">ASP.NET Core</span></span>

* [<span data-ttu-id="a0472-181">Servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="a0472-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="a0472-182">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a0472-182">Additional resources</span></span>

* [<span data-ttu-id="a0472-183">Hub</span><span class="sxs-lookup"><span data-stu-id="a0472-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a0472-184">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a0472-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a0472-185">Client .NET</span><span class="sxs-lookup"><span data-stu-id="a0472-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a0472-186">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="a0472-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
