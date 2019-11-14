---
title: Differenze tra SignalR e ASP.NET Core SignalR
author: bradygaster
description: Differenze tra SignalR e ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963715"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="1e570-103">Differenze tra ASP.NET SignalR e ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="1e570-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="1e570-104">ASP.NET Core SignalR non è compatibile con i client o i server per SignalRASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e570-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="1e570-105">Questo articolo descrive in dettaglio le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="1e570-106">Come identificare la versione di SignalR</span><span class="sxs-lookup"><span data-stu-id="1e570-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="1e570-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1e570-107">ASP.NET SignalR</span></span> | <span data-ttu-id="1e570-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="1e570-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="1e570-109">Pacchetto NuGet server</span><span class="sxs-lookup"><span data-stu-id="1e570-109">Server NuGet Package</span></span> | <span data-ttu-id="1e570-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="1e570-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="1e570-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="1e570-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="1e570-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="1e570-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="1e570-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="1e570-113">(.NET Framework)</span></span> |
| <span data-ttu-id="1e570-114">Pacchetti NuGet client</span><span class="sxs-lookup"><span data-stu-id="1e570-114">Client NuGet Packages</span></span> | <span data-ttu-id="1e570-115">[Microsoft. AspNet.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="1e570-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="1e570-116">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="1e570-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="1e570-117">[Microsoft. AspNetCore.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="1e570-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="1e570-118">Pacchetto NPM client</span><span class="sxs-lookup"><span data-stu-id="1e570-118">Client npm Package</span></span> | [<span data-ttu-id="1e570-119">SignalR</span><span class="sxs-lookup"><span data-stu-id="1e570-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="1e570-120">Client Java</span><span class="sxs-lookup"><span data-stu-id="1e570-120">Java Client</span></span> | <span data-ttu-id="1e570-121">[Repository GitHub](https://github.com/SignalR/java-client) (deprecato)</span><span class="sxs-lookup"><span data-stu-id="1e570-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="1e570-122">Pacchetto Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="1e570-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="1e570-123">Tipo di app Server</span><span class="sxs-lookup"><span data-stu-id="1e570-123">Server App Type</span></span> | <span data-ttu-id="1e570-124">ASP.NET (System. Web) o OWIN Self-host</span><span class="sxs-lookup"><span data-stu-id="1e570-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="1e570-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e570-125">ASP.NET Core</span></span> |
| <span data-ttu-id="1e570-126">Piattaforme server supportate</span><span class="sxs-lookup"><span data-stu-id="1e570-126">Supported Server Platforms</span></span> | <span data-ttu-id="1e570-127">.NET Framework 4,5 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1e570-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="1e570-128">.NET Framework 4.6.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1e570-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="1e570-129">.NET Core 2,1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1e570-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="1e570-130">Differenze tra le funzionalità</span><span class="sxs-lookup"><span data-stu-id="1e570-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="1e570-131">Riconnessioni automatiche</span><span class="sxs-lookup"><span data-stu-id="1e570-131">Automatic reconnects</span></span>

<span data-ttu-id="1e570-132">Le riconnessioni automatiche non sono supportate in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="1e570-133">Se il client è disconnesso, l'utente deve avviare in modo esplicito una nuova connessione se desidera riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="1e570-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="1e570-134">In ASP.NET SignalRSignalR tenta di riconnettersi al server se la connessione viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="1e570-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="1e570-135">Supporto del protocollo</span><span class="sxs-lookup"><span data-stu-id="1e570-135">Protocol support</span></span>

<span data-ttu-id="1e570-136">ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato su [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="1e570-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="1e570-137">Inoltre, è possibile creare protocolli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="1e570-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="1e570-138">Trasporti</span><span class="sxs-lookup"><span data-stu-id="1e570-138">Transports</span></span>

<span data-ttu-id="1e570-139">Il trasporto con frame Forever non è supportato in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="1e570-140">Differenze sul server</span><span class="sxs-lookup"><span data-stu-id="1e570-140">Differences on the server</span></span>

<span data-ttu-id="1e570-141">Le librerie ASP.NET Core SignalR sul lato server sono incluse nel pacchetto del [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) che fa parte del modello di **applicazione Web ASP.NET Core** per i progetti Razor e MVC.</span><span class="sxs-lookup"><span data-stu-id="1e570-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="1e570-142">ASP.NET Core SignalR è un middleware ASP.NET Core, quindi deve essere configurato chiamando [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1e570-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1e570-143">Per configurare il routing, mappare le route agli hub all'interno della chiamata al metodo [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1e570-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="1e570-144">Per configurare il routing, mappare le route agli hub all'interno della chiamata al metodo [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1e570-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="1e570-145">Sessioni permanenti</span><span class="sxs-lookup"><span data-stu-id="1e570-145">Sticky sessions</span></span>

<span data-ttu-id="1e570-146">Il modello di scalabilità orizzontale per ASP.NET SignalR consente ai client di riconnettersi e inviare messaggi a qualsiasi server della farm.</span><span class="sxs-lookup"><span data-stu-id="1e570-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="1e570-147">In ASP.NET Core SignalR, il client deve interagire con lo stesso server per la durata della connessione.</span><span class="sxs-lookup"><span data-stu-id="1e570-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="1e570-148">Per la scalabilità orizzontale con Redis, significa che sono necessarie sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="1e570-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="1e570-149">Per la scalabilità orizzontale tramite il [servizio SignalR di Azure](/azure/azure-signalr/), non sono necessarie sessioni permanenti perché il servizio gestisce le connessioni ai client.</span><span class="sxs-lookup"><span data-stu-id="1e570-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="1e570-150">Hub singolo per connessione</span><span class="sxs-lookup"><span data-stu-id="1e570-150">Single hub per connection</span></span>

<span data-ttu-id="1e570-151">In ASP.NET Core SignalRil modello di connessione è stato semplificato.</span><span class="sxs-lookup"><span data-stu-id="1e570-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="1e570-152">Le connessioni vengono effettuate direttamente in un singolo hub, anziché una singola connessione usata per condividere l'accesso a più hub.</span><span class="sxs-lookup"><span data-stu-id="1e570-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="1e570-153">Flusso</span><span class="sxs-lookup"><span data-stu-id="1e570-153">Streaming</span></span>

<span data-ttu-id="1e570-154">ASP.NET Core SignalR ora supporta il [flusso dei dati](xref:signalr/streaming) dall'hub al client.</span><span class="sxs-lookup"><span data-stu-id="1e570-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="1e570-155">Stato</span><span class="sxs-lookup"><span data-stu-id="1e570-155">State</span></span>

<span data-ttu-id="1e570-156">La possibilità di passare uno stato arbitrario tra i client e l'hub (spesso denominati HubState) è stata rimossa, oltre al supporto per i messaggi di stato.</span><span class="sxs-lookup"><span data-stu-id="1e570-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="1e570-157">Al momento non è presente alcuna controparte dei proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="1e570-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="1e570-158">Rimozione di PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="1e570-158">PersistentConnection removal</span></span>

<span data-ttu-id="1e570-159">In ASP.NET Core SignalR, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="1e570-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="1e570-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="1e570-160">GlobalHost</span></span>

<span data-ttu-id="1e570-161">ASP.NET Core dispone DI INSERIMENTO DI dipendenze (DI) incorporato nel Framework.</span><span class="sxs-lookup"><span data-stu-id="1e570-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="1e570-162">I servizi possono usare l'inserimento DI dipendenze per accedere a [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="1e570-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="1e570-163">L'oggetto `GlobalHost` usato in ASP.NET SignalR per ottenere un `HubContext` non esiste in ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="1e570-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="1e570-164">HubPipeline</span></span>

<span data-ttu-id="1e570-165">ASP.NET Core SignalR non dispone del supporto per i moduli di `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="1e570-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="1e570-166">Differenze sul client</span><span class="sxs-lookup"><span data-stu-id="1e570-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="1e570-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="1e570-167">TypeScript</span></span>

<span data-ttu-id="1e570-168">Il client di SignalR ASP.NET Core è scritto in [typescript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="1e570-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="1e570-169">È possibile scrivere in JavaScript o TypeScript quando si usa il [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="1e570-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="1e570-170">Il client JavaScript è ospitato in [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="1e570-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="1e570-171">Nelle versioni precedenti il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e570-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="1e570-172">Per le versioni principali, il pacchetto NPM di [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e570-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="1e570-173">Questo pacchetto non è incluso nel modello di **applicazione Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="1e570-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="1e570-174">Usare NPM per ottenere e installare il pacchetto NPM `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="1e570-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="1e570-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="1e570-175">jQuery</span></span>

<span data-ttu-id="1e570-176">La dipendenza da jQuery è stata rimossa, tuttavia i progetti possono comunque usare jQuery.</span><span class="sxs-lookup"><span data-stu-id="1e570-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="1e570-177">Supporto di Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="1e570-177">Internet Explorer support</span></span>

<span data-ttu-id="1e570-178">ASP.NET Core SignalR richiede Microsoft Internet Explorer 11 o versione successiva (ASP.NET SignalR supporta Microsoft Internet Explorer 8 e versioni successive).</span><span class="sxs-lookup"><span data-stu-id="1e570-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="1e570-179">Sintassi del metodo client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e570-179">JavaScript client method syntax</span></span>

<span data-ttu-id="1e570-180">La sintassi JavaScript è cambiata rispetto alla versione precedente di SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="1e570-181">Anziché usare l'oggetto `$connection`, creare una connessione usando l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="1e570-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="1e570-182">Usare il metodo [on](/javascript/api/@aspnet/signalr/HubConnection#on) per specificare i metodi client che l'hub può chiamare.</span><span class="sxs-lookup"><span data-stu-id="1e570-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="1e570-183">Dopo aver creato il metodo client, avviare la connessione dell'hub.</span><span class="sxs-lookup"><span data-stu-id="1e570-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="1e570-184">Concatenare un metodo [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) per registrare o gestire gli errori.</span><span class="sxs-lookup"><span data-stu-id="1e570-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="1e570-185">Proxy Hub</span><span class="sxs-lookup"><span data-stu-id="1e570-185">Hub proxies</span></span>

<span data-ttu-id="1e570-186">I proxy Hub non vengono più generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1e570-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="1e570-187">Al contrario, il nome del metodo viene passato nell'API [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) sotto forma di stringa.</span><span class="sxs-lookup"><span data-stu-id="1e570-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="1e570-188">.NET e altri client</span><span class="sxs-lookup"><span data-stu-id="1e570-188">.NET and other clients</span></span>

<span data-ttu-id="1e570-189">Il pacchetto NuGet `Microsoft.AspNetCore.SignalR.Client` contiene le librerie client .NET per ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e570-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="1e570-190">Usare [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.</span><span class="sxs-lookup"><span data-stu-id="1e570-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="1e570-191">Differenze di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="1e570-191">Scaleout differences</span></span>

<span data-ttu-id="1e570-192">ASP.NET SignalR supporta SQL Server e Redis.</span><span class="sxs-lookup"><span data-stu-id="1e570-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="1e570-193">ASP.NET Core SignalR supporta il servizio SignalR di Azure e Redis.</span><span class="sxs-lookup"><span data-stu-id="1e570-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="1e570-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1e570-194">ASP.NET</span></span>

* <span data-ttu-id="1e570-195">[scalabilità orizzontale di SignalR con il bus di servizio di Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="1e570-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="1e570-196">[scalabilità orizzontale di SignalR con Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="1e570-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="1e570-197">[SignalR la scalabilità orizzontale con SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="1e570-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="1e570-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e570-198">ASP.NET Core</span></span>

* <span data-ttu-id="1e570-199">[Servizio SignalR di Azure](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="1e570-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="1e570-200">Backplane Redis</span><span class="sxs-lookup"><span data-stu-id="1e570-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="1e570-201">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1e570-201">Additional resources</span></span>

* [<span data-ttu-id="1e570-202">Hub</span><span class="sxs-lookup"><span data-stu-id="1e570-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1e570-203">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e570-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="1e570-204">Client .NET</span><span class="sxs-lookup"><span data-stu-id="1e570-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="1e570-205">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="1e570-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
