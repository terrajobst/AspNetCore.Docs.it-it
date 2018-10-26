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
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Differenze tra ASP.NET SignalR e ASP.NET Core SignalR

ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR. Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Come identificare la versione di SignalR

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Pacchetto NuGet server | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Pacchetti NuGet del client | [ASPNET](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Client npm, pacchetti | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Tipo di App server | ASP.NET (System. Web) o Self-hosting OWIN | ASP.NET Core |
| Nelle piattaforme Server supportati | .NET framework 4.5 o versione successiva | .NET Framework 4.6.1 o versioni successive<br>.NET core 2.1 o versioni successive |

## <a name="feature-differences"></a>Differenze di funzionalità

### <a name="automatic-reconnects"></a>Connessioni automatiche

Le connessioni automatica non sono supportate in ASP.NET Core SignalR. Se il client viene disconnesso, l'utente deve iniziare in modo esplicito una nuova connessione se si vuole riconnettersi. In ASP.NET SignalR, SignalR tenta di riconnettersi al server se la connessione viene interrotta. 

### <a name="protocol-support"></a>Supporto del protocollo

ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol). Inoltre, è possibile creare i protocolli personalizzati.

## <a name="differences-on-the-server"></a>Comprendere le differenze tra il server

Le librerie sul lato server ASP.NET Core SignalR sono incluse nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per Razor e MVC progetti.

ASP.NET Core SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Per configurare il routing, eseguire il mapping di route per gli hub all'interno di [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chiamata al metodo il `Startup.Configure` (metodo).

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessioni permanenti ora richiesto

A causa di scalabilità orizzontale come ha funzionato in ASP.NET SignalR, i client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm. A causa di modifiche per il modello di scalabilità orizzontale, nonché non sono supportate le connessioni, ciò non è più supportata. Dopo che il client si connette al server, deve interagire con lo stesso server per la durata della connessione.

### <a name="single-hub-per-connection"></a>Hub singolo per ogni connessione

In ASP.NET Core SignalR, il modello di connessione è stato semplificato. Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.

### <a name="streaming"></a>Flusso

ASP.NET Core SignalR ora supporta [dati in streaming](xref:signalr/streaming) dall'hub al client.

### <a name="state"></a>State

La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato. Nel momento in cui non vi è alcun equivalente di proxy dell'hub.

## <a name="differences-on-the-client"></a>Comprendere le differenze tra il client

### <a name="typescript"></a>TypeScript

Il client di ASP.NET Core SignalR è scritto in [TypeScript](https://www.typescriptlang.org/). È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)

Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio. Per le versioni di base, il [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacchetto npm contiene le librerie JavaScript. Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello. Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.

### <a name="javascript-client-method-syntax"></a>Sintassi del metodo client JavaScript

La sintassi JavaScript è stato modificato dalla versione precedente di SignalR. Invece di usare la `$connection` dell'oggetto, creare una connessione usando la [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Usare la [su](/javascript/api/@aspnet/signalr/HubConnection#on) metodo per specificare metodi di client può chiamare l'hub.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Dopo aver creato il metodo di client, avviare la connessione dell'hub. Catena di una [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodo per accedere o gestire gli errori.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy di hub

Vengono generati non più automaticamente i proxy dell'hub. Il nome del metodo viene invece passato nella [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sotto forma di stringa.

### <a name="net-and-other-clients"></a>.NET e altri client

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.

Usare la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Differenze di scalabilità orizzontale

ASP.NET SignalR supporta SQL Server e Redis. ASP.NET Core SignalR supporta servizio Azure SignalR e Redis.

### <a name="aspnet"></a>ASP.NET

* [Scalabilità orizzontale di SignalR con il Bus di servizio di Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Scalabilità orizzontale di SignalR con Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Scalabilità orizzontale di SignalR con SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Servizio Azure SignalR](/azure/azure-signalr/)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Piattaforme supportate](xref:signalr/supported-platforms)
