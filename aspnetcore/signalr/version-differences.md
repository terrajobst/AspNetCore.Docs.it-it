---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: bradygaster
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746544"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="4f2fc-103">Differenze tra ASP.NET SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="4f2fc-104">ASP.NET Core SignalR non è compatibile con i client o i server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="4f2fc-105">Questo articolo descrive in dettaglio le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="4f2fc-106">Come identificare la versione di SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="4f2fc-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-107">ASP.NET SignalR</span></span> | <span data-ttu-id="4f2fc-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="4f2fc-109">Pacchetto NuGet server</span><span class="sxs-lookup"><span data-stu-id="4f2fc-109">Server NuGet Package</span></span> | [<span data-ttu-id="4f2fc-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="4f2fc-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="4f2fc-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="4f2fc-112">[Microsoft. AspNetCore. SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="4f2fc-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="4f2fc-113">Pacchetti NuGet client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-113">Client NuGet Packages</span></span> | [<span data-ttu-id="4f2fc-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="4f2fc-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="4f2fc-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="4f2fc-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="4f2fc-117">Pacchetto NPM client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-117">Client npm Package</span></span> | [<span data-ttu-id="4f2fc-118">signalr</span><span class="sxs-lookup"><span data-stu-id="4f2fc-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="4f2fc-119">Client Java</span><span class="sxs-lookup"><span data-stu-id="4f2fc-119">Java Client</span></span> | <span data-ttu-id="4f2fc-120">[Repository GitHub](https://github.com/SignalR/java-client) deprecato</span><span class="sxs-lookup"><span data-stu-id="4f2fc-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="4f2fc-121">Pacchetto Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="4f2fc-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="4f2fc-122">Tipo di app Server</span><span class="sxs-lookup"><span data-stu-id="4f2fc-122">Server App Type</span></span> | <span data-ttu-id="4f2fc-123">ASP.NET (System. Web) o OWIN Self-host</span><span class="sxs-lookup"><span data-stu-id="4f2fc-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="4f2fc-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f2fc-124">ASP.NET Core</span></span> |
| <span data-ttu-id="4f2fc-125">Piattaforme server supportate</span><span class="sxs-lookup"><span data-stu-id="4f2fc-125">Supported Server Platforms</span></span> | <span data-ttu-id="4f2fc-126">.NET Framework 4,5 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="4f2fc-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="4f2fc-127">.NET Framework 4.6.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="4f2fc-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="4f2fc-128">.NET Core 2,1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="4f2fc-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="4f2fc-129">Differenze tra le funzionalità</span><span class="sxs-lookup"><span data-stu-id="4f2fc-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="4f2fc-130">Riconnessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="4f2fc-130">Automatic reconnects</span></span>

<span data-ttu-id="4f2fc-131">Le riconnessioni automatiche non sono supportate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="4f2fc-132">Se il client è disconnesso, l'utente deve avviare in modo esplicito una nuova connessione se desidera riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="4f2fc-133">In ASP.NET SignalR, SignalR tenta di riconnettersi al server se la connessione viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="4f2fc-134">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="4f2fc-134">Protocol support</span></span>

<span data-ttu-id="4f2fc-135">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato su [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="4f2fc-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="4f2fc-136">Inoltre, è possibile creare protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="4f2fc-137">Trasporti</span><span class="sxs-lookup"><span data-stu-id="4f2fc-137">Transports</span></span>

<span data-ttu-id="4f2fc-138">Il trasporto con frame Forever non è supportato in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="4f2fc-139">Differenze sul server</span><span class="sxs-lookup"><span data-stu-id="4f2fc-139">Differences on the server</span></span>

<span data-ttu-id="4f2fc-140">Le librerie sul lato server di ASP.NET Core SignalR sono incluse nel pacchetto del [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) che fa parte del modello di **applicazione Web ASP.NET Core** per i progetti Razor e MVC.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="4f2fc-141">ASP.NET Core SignalR è un middleware ASP.NET Core, quindi deve essere configurato chiamando [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4f2fc-142">Per configurare il routing, mappare le route agli Hub all'interno della chiamata al metodo [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="4f2fc-143">Per configurare il routing, mappare le route agli Hub all'interno della chiamata al metodo [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="4f2fc-144">Sessioni permanenti</span><span class="sxs-lookup"><span data-stu-id="4f2fc-144">Sticky sessions</span></span>

<span data-ttu-id="4f2fc-145">Il modello di scalabilità orizzontale per ASP.NET SignalR consente ai client di riconnettersi e inviare messaggi a qualsiasi server della farm.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="4f2fc-146">In ASP.NET Core SignalR il client deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="4f2fc-147">Per la scalabilità orizzontale con Redis, significa che sono necessarie sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="4f2fc-148">Per la scalabilità con il [servizio Azure SignalR](/azure/azure-signalr/), non sono necessarie sessioni permanenti perché il servizio gestisce le connessioni ai client.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="4f2fc-149">Hub singolo per connessione</span><span class="sxs-lookup"><span data-stu-id="4f2fc-149">Single hub per connection</span></span>

<span data-ttu-id="4f2fc-150">In ASP.NET Core SignalR il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="4f2fc-151">Le connessioni vengono effettuate direttamente in un singolo hub, anziché una singola connessione usata per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="4f2fc-152">Flusso</span><span class="sxs-lookup"><span data-stu-id="4f2fc-152">Streaming</span></span>

<span data-ttu-id="4f2fc-153">ASP.NET Core SignalR ora supporta lo [streaming dei dati](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="4f2fc-154">Stato</span><span class="sxs-lookup"><span data-stu-id="4f2fc-154">State</span></span>

<span data-ttu-id="4f2fc-155">La possibilità di passare uno stato arbitrario tra i client e l'hub (spesso denominati HubState) è stata rimossa, oltre al supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="4f2fc-156">Al momento non è presente alcuna controparte dei proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="4f2fc-157">Rimozione di PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="4f2fc-157">PersistentConnection removal</span></span>

<span data-ttu-id="4f2fc-158">In ASP.NET Core SignalR la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="4f2fc-159">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="4f2fc-159">GlobalHost</span></span>

<span data-ttu-id="4f2fc-160">ASP.NET Core dispone DI INSERIMENTO DI dipendenze (DI) incorporato nel Framework.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="4f2fc-161">I servizi possono usare l'inserimento DI dipendenze per accedere a [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="4f2fc-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="4f2fc-162">L' `GlobalHost` oggetto usato in ASP.NET SignalR per ottenere un `HubContext` valore non esiste in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="4f2fc-163">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="4f2fc-163">HubPipeline</span></span>

<span data-ttu-id="4f2fc-164">ASP.NET Core SignalR non dispone del supporto `HubPipeline` per i moduli.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="4f2fc-165">Differenze sul client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="4f2fc-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="4f2fc-166">TypeScript</span></span>

<span data-ttu-id="4f2fc-167">Il client ASP.NET Core SignalR è scritto in [typescript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="4f2fc-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="4f2fc-168">È possibile scrivere in JavaScript o TypeScript quando si usa il [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="4f2fc-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="4f2fc-169">Il client JavaScript è ospitato in [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="4f2fc-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="4f2fc-170">Nelle versioni precedenti il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="4f2fc-171">Per le versioni principali, il [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) pacchetto NPM contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="4f2fc-172">Questo pacchetto non è incluso nel modello di **applicazione Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="4f2fc-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4f2fc-173">Usare NPM per ottenere e installare il `@aspnet/signalr` pacchetto NPM.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="4f2fc-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="4f2fc-174">jQuery</span></span>

<span data-ttu-id="4f2fc-175">La dipendenza da jQuery è stata rimossa, tuttavia i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="4f2fc-176">Supporto di Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="4f2fc-176">Internet Explorer support</span></span>

<span data-ttu-id="4f2fc-177">ASP.NET Core SignalR richiede Microsoft Internet Explorer 11 o versione successiva (ASP.NET SignalR supporta Microsoft Internet Explorer 8 e versioni successive).</span><span class="sxs-lookup"><span data-stu-id="4f2fc-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="4f2fc-178">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4f2fc-178">JavaScript client method syntax</span></span>

<span data-ttu-id="4f2fc-179">La sintassi di JavaScript è cambiata rispetto alla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="4f2fc-180">Anziché usare l' `$connection` oggetto, creare una connessione usando l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="4f2fc-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="4f2fc-181">Usare il metodo [on](/javascript/api/@aspnet/signalr/HubConnection#on) per specificare i metodi client che l'hub può chiamare.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="4f2fc-182">Dopo aver creato il metodo client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="4f2fc-183">Concatenare un metodo [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) per registrare o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="4f2fc-184">Proxy Hub</span><span class="sxs-lookup"><span data-stu-id="4f2fc-184">Hub proxies</span></span>

<span data-ttu-id="4f2fc-185">I proxy Hub non vengono più generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="4f2fc-186">Al contrario, il nome del metodo viene passato nell'API [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="4f2fc-187">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="4f2fc-187">.NET and other clients</span></span>

<span data-ttu-id="4f2fc-188">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="4f2fc-189">Usare [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="4f2fc-190">Differenze di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="4f2fc-190">Scaleout differences</span></span>

<span data-ttu-id="4f2fc-191">ASP.NET SignalR supporta SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="4f2fc-192">ASP.NET Core SignalR supporta il servizio Azure SignalR e Redis.</span><span class="sxs-lookup"><span data-stu-id="4f2fc-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="4f2fc-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f2fc-193">ASP.NET</span></span>

* [<span data-ttu-id="4f2fc-194">Scalabilità orizzontale di SignalR con il bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="4f2fc-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="4f2fc-195">Scalabilità orizzontale di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="4f2fc-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="4f2fc-196">Scalabilità orizzontale di SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="4f2fc-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="4f2fc-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f2fc-197">ASP.NET Core</span></span>

* [<span data-ttu-id="4f2fc-198">Servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="4f2fc-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="4f2fc-199">Backplane Redis</span><span class="sxs-lookup"><span data-stu-id="4f2fc-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="4f2fc-200">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4f2fc-200">Additional resources</span></span>

* [<span data-ttu-id="4f2fc-201">Hub</span><span class="sxs-lookup"><span data-stu-id="4f2fc-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4f2fc-202">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4f2fc-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="4f2fc-203">Client .NET</span><span class="sxs-lookup"><span data-stu-id="4f2fc-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4f2fc-204">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="4f2fc-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
