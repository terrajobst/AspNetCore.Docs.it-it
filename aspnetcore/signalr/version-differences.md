---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: tdykstra
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: fb10d6e62ff28128e6e9e5dcef55e44f25de8ad0
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425120"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="8bbb0-103">Differenze tra ASP.NET SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="8bbb0-104">ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="8bbb0-105">Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="8bbb0-106">Come identificare la versione di SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="8bbb0-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-107">ASP.NET SignalR</span></span> | <span data-ttu-id="8bbb0-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="8bbb0-109">Pacchetto NuGet server</span><span class="sxs-lookup"><span data-stu-id="8bbb0-109">Server NuGet Package</span></span> | [<span data-ttu-id="8bbb0-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="8bbb0-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8bbb0-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="8bbb0-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="8bbb0-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="8bbb0-113">Pacchetti NuGet del client</span><span class="sxs-lookup"><span data-stu-id="8bbb0-113">Client NuGet Packages</span></span> | [<span data-ttu-id="8bbb0-114">ASPNET</span><span class="sxs-lookup"><span data-stu-id="8bbb0-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="8bbb0-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="8bbb0-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="8bbb0-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="8bbb0-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="8bbb0-117">Client npm, pacchetti</span><span class="sxs-lookup"><span data-stu-id="8bbb0-117">Client npm Package</span></span> | [<span data-ttu-id="8bbb0-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="8bbb0-119">Client Java</span><span class="sxs-lookup"><span data-stu-id="8bbb0-119">Java Client</span></span> | <span data-ttu-id="8bbb0-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecata)</span><span class="sxs-lookup"><span data-stu-id="8bbb0-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="8bbb0-121">Pacchetto Maven [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="8bbb0-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="8bbb0-122">Tipo di App server</span><span class="sxs-lookup"><span data-stu-id="8bbb0-122">Server App Type</span></span> | <span data-ttu-id="8bbb0-123">ASP.NET (System. Web) o Self-hosting OWIN</span><span class="sxs-lookup"><span data-stu-id="8bbb0-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="8bbb0-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8bbb0-124">ASP.NET Core</span></span> |
| <span data-ttu-id="8bbb0-125">Nelle piattaforme Server supportati</span><span class="sxs-lookup"><span data-stu-id="8bbb0-125">Supported Server Platforms</span></span> | <span data-ttu-id="8bbb0-126">.NET framework 4.5 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="8bbb0-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="8bbb0-127">.NET Framework 4.6.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="8bbb0-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="8bbb0-128">.NET core 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="8bbb0-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="8bbb0-129">Differenze di funzionalità</span><span class="sxs-lookup"><span data-stu-id="8bbb0-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="8bbb0-130">Connessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="8bbb0-130">Automatic reconnects</span></span>

<span data-ttu-id="8bbb0-131">Le connessioni automatica non sono supportate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="8bbb0-132">Se il client viene disconnesso, l'utente deve iniziare in modo esplicito una nuova connessione se si vuole riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="8bbb0-133">In ASP.NET SignalR, SignalR tenta di riconnettersi al server se la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="8bbb0-134">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="8bbb0-134">Protocol support</span></span>

<span data-ttu-id="8bbb0-135">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="8bbb0-136">Inoltre, è possibile creare i protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="8bbb0-137">Trasporti</span><span class="sxs-lookup"><span data-stu-id="8bbb0-137">Transports</span></span>

<span data-ttu-id="8bbb0-138">Il trasporto Forever Frame non è supportato in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="8bbb0-139">Comprendere le differenze tra il server</span><span class="sxs-lookup"><span data-stu-id="8bbb0-139">Differences on the server</span></span>

<span data-ttu-id="8bbb0-140">Le librerie sul lato server ASP.NET Core SignalR sono incluse nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per Razor e MVC progetti.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="8bbb0-141">ASP.NET Core SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="8bbb0-142">Per configurare il routing, eseguire il mapping di route per gli hub all'interno di [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chiamata al metodo il `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-142">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="8bbb0-143">Sessioni permanenti</span><span class="sxs-lookup"><span data-stu-id="8bbb0-143">Sticky sessions</span></span>

<span data-ttu-id="8bbb0-144">Il modello di scalabilità orizzontale ASP.NET SignalR consente ai client di riconnettersi e inviare messaggi a qualsiasi server nella farm.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-144">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="8bbb0-145">In ASP.NET Core SignalR, il client deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-145">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="8bbb0-146">Per la scalabilità orizzontale con Redis, che significa che le sessioni permanenti sono necessari.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-146">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="8bbb0-147">Per l'uso di scalabilità orizzontale [servizio Azure SignalR](/azure/azure-signalr/), le sessioni permanenti non sono necessarie perché il servizio gestisce le connessioni ai client.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-147">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="8bbb0-148">Hub singolo per ogni connessione</span><span class="sxs-lookup"><span data-stu-id="8bbb0-148">Single hub per connection</span></span>

<span data-ttu-id="8bbb0-149">In ASP.NET Core SignalR, il modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-149">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="8bbb0-150">Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-150">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="8bbb0-151">Flusso</span><span class="sxs-lookup"><span data-stu-id="8bbb0-151">Streaming</span></span>

<span data-ttu-id="8bbb0-152">ASP.NET Core SignalR ora supporta [dati in streaming](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-152">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="8bbb0-153">Stato</span><span class="sxs-lookup"><span data-stu-id="8bbb0-153">State</span></span>

<span data-ttu-id="8bbb0-154">La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-154">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="8bbb0-155">Nel momento in cui non vi è alcun equivalente di proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-155">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="8bbb0-156">Rimozione PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="8bbb0-156">PersistentConnection removal</span></span>

<span data-ttu-id="8bbb0-157">In ASP.NET Core SignalR, il [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) classe è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-157">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span> 

### <a name="globalhost"></a><span data-ttu-id="8bbb0-158">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="8bbb0-158">GlobalHost</span></span>

<span data-ttu-id="8bbb0-159">ASP.NET Core con inserimento di dipendenze () incorporato in framework.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-159">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="8bbb0-160">I servizi possono utilizzare l'inserimento delle dipendenze per accedere la [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-160">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="8bbb0-161">Il `GlobalHost` oggetto che viene utilizzato in ASP.NET SignalR per ottenere un `HubContext` non esiste in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-161">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="8bbb0-162">A HubPipeline</span><span class="sxs-lookup"><span data-stu-id="8bbb0-162">HubPipeline</span></span>

<span data-ttu-id="8bbb0-163">ASP.NET Core SignalR non dispone di supporto per `HubPipeline` moduli.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-163">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="8bbb0-164">Comprendere le differenze tra il client</span><span class="sxs-lookup"><span data-stu-id="8bbb0-164">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="8bbb0-165">TypeScript</span><span class="sxs-lookup"><span data-stu-id="8bbb0-165">TypeScript</span></span>

<span data-ttu-id="8bbb0-166">Il client di ASP.NET Core SignalR è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-166">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="8bbb0-167">È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-167">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="8bbb0-168">Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8bbb0-168">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="8bbb0-169">Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-169">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="8bbb0-170">Per le versioni di base, il [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacchetto npm contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-170">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="8bbb0-171">Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-171">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8bbb0-172">Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-172">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="8bbb0-173">jQuery</span><span class="sxs-lookup"><span data-stu-id="8bbb0-173">jQuery</span></span>

<span data-ttu-id="8bbb0-174">La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-174">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="8bbb0-175">Supporto di Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8bbb0-175">Internet Explorer support</span></span>

<span data-ttu-id="8bbb0-176">ASP.NET Core SignalR richiede Microsoft Internet Explorer 11 o versione successiva (ASP.NET SignalR supportato Microsoft Internet Explorer 8 e versioni successive).</span><span class="sxs-lookup"><span data-stu-id="8bbb0-176">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="8bbb0-177">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8bbb0-177">JavaScript client method syntax</span></span>

<span data-ttu-id="8bbb0-178">La sintassi JavaScript è stato modificato dalla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-178">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="8bbb0-179">Invece di usare la `$connection` dell'oggetto, creare una connessione usando la [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-179">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="8bbb0-180">Usare la [su](/javascript/api/@aspnet/signalr/HubConnection#on) metodo per specificare metodi di client può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-180">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="8bbb0-181">Dopo aver creato il metodo di client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-181">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="8bbb0-182">Catena di una [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodo per accedere o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-182">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="8bbb0-183">Proxy di hub</span><span class="sxs-lookup"><span data-stu-id="8bbb0-183">Hub proxies</span></span>

<span data-ttu-id="8bbb0-184">Vengono generati non più automaticamente i proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-184">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="8bbb0-185">Il nome del metodo viene invece passato nella [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-185">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="8bbb0-186">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="8bbb0-186">.NET and other clients</span></span>

<span data-ttu-id="8bbb0-187">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-187">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="8bbb0-188">Usare la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-188">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="8bbb0-189">Differenze di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="8bbb0-189">Scaleout differences</span></span>

<span data-ttu-id="8bbb0-190">ASP.NET SignalR supporta SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-190">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="8bbb0-191">ASP.NET Core SignalR supporta servizio Azure SignalR e Redis.</span><span class="sxs-lookup"><span data-stu-id="8bbb0-191">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="8bbb0-192">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8bbb0-192">ASP.NET</span></span>

* [<span data-ttu-id="8bbb0-193">Scalabilità orizzontale di SignalR con il Bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="8bbb0-193">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="8bbb0-194">Scalabilità orizzontale di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="8bbb0-194">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="8bbb0-195">Scalabilità orizzontale di SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="8bbb0-195">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="8bbb0-196">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8bbb0-196">ASP.NET Core</span></span>

* [<span data-ttu-id="8bbb0-197">Servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="8bbb0-197">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="8bbb0-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8bbb0-198">Additional resources</span></span>

* [<span data-ttu-id="8bbb0-199">Hub</span><span class="sxs-lookup"><span data-stu-id="8bbb0-199">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8bbb0-200">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8bbb0-200">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8bbb0-201">Client .NET</span><span class="sxs-lookup"><span data-stu-id="8bbb0-201">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="8bbb0-202">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="8bbb0-202">Supported platforms</span></span>](xref:signalr/supported-platforms)
