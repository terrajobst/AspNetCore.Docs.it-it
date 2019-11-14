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
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a>Differenze tra ASP.NET SignalR e ASP.NET Core SignalR

ASP.NET Core SignalR non è compatibile con i client o i server per SignalRASP.NET. Questo articolo descrive in dettaglio le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.

## <a name="how-to-identify-the-opno-locsignalr-version"></a>Come identificare la versione di SignalR

|                      | SignalR ASP.NET | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Pacchetto NuGet server | [Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Pacchetti NuGet client | [Microsoft. AspNet.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Pacchetto NPM client | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Client Java | [Repository GitHub](https://github.com/SignalR/java-client) (deprecato)  | Pacchetto Maven [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Tipo di app Server | ASP.NET (System. Web) o OWIN Self-host | ASP.NET Core |
| Piattaforme server supportate | .NET Framework 4,5 o versione successiva | .NET Framework 4.6.1 o versioni successive<br>.NET Core 2,1 o versione successiva |

## <a name="feature-differences"></a>Differenze tra le funzionalità

### <a name="automatic-reconnects"></a>Riconnessioni automatiche

Le riconnessioni automatiche non sono supportate in ASP.NET Core SignalR. Se il client è disconnesso, l'utente deve avviare in modo esplicito una nuova connessione se desidera riconnettersi. In ASP.NET SignalRSignalR tenta di riconnettersi al server se la connessione viene eliminata.

### <a name="protocol-support"></a>Supporto del protocollo

ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato su [MessagePack](xref:signalr/messagepackhubprotocol). Inoltre, è possibile creare protocolli personalizzati.

### <a name="transports"></a>Trasporti

Il trasporto con frame Forever non è supportato in ASP.NET Core SignalR.

## <a name="differences-on-the-server"></a>Differenze sul server

Le librerie ASP.NET Core SignalR sul lato server sono incluse nel pacchetto del [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) che fa parte del modello di **applicazione Web ASP.NET Core** per i progetti Razor e MVC.

ASP.NET Core SignalR è un middleware ASP.NET Core, quindi deve essere configurato chiamando [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Per configurare il routing, mappare le route agli hub all'interno della chiamata al metodo [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) nel metodo `Startup.Configure`.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Per configurare il routing, mappare le route agli hub all'interno della chiamata al metodo [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) nel metodo `Startup.Configure`.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Sessioni permanenti

Il modello di scalabilità orizzontale per ASP.NET SignalR consente ai client di riconnettersi e inviare messaggi a qualsiasi server della farm. In ASP.NET Core SignalR, il client deve interagire con lo stesso server per la durata della connessione. Per la scalabilità orizzontale con Redis, significa che sono necessarie sessioni permanenti. Per la scalabilità orizzontale tramite il [servizio SignalR di Azure](/azure/azure-signalr/), non sono necessarie sessioni permanenti perché il servizio gestisce le connessioni ai client.

### <a name="single-hub-per-connection"></a>Hub singolo per connessione

In ASP.NET Core SignalRil modello di connessione è stato semplificato. Le connessioni vengono effettuate direttamente in un singolo hub, anziché una singola connessione usata per condividere l'accesso a più hub.

### <a name="streaming"></a>Flusso

ASP.NET Core SignalR ora supporta il [flusso dei dati](xref:signalr/streaming) dall'hub al client.

### <a name="state"></a>Stato

La possibilità di passare uno stato arbitrario tra i client e l'hub (spesso denominati HubState) è stata rimossa, oltre al supporto per i messaggi di stato. Al momento non è presente alcuna controparte dei proxy dell'hub.

### <a name="persistentconnection-removal"></a>Rimozione di PersistentConnection

In ASP.NET Core SignalR, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) è stata rimossa.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core dispone DI INSERIMENTO DI dipendenze (DI) incorporato nel Framework. I servizi possono usare l'inserimento DI dipendenze per accedere a [HubContext](xref:signalr/hubcontext). L'oggetto `GlobalHost` usato in ASP.NET SignalR per ottenere un `HubContext` non esiste in ASP.NET Core SignalR.

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR non dispone del supporto per i moduli di `HubPipeline`.

## <a name="differences-on-the-client"></a>Differenze sul client

### <a name="typescript"></a>TypeScript

Il client di SignalR ASP.NET Core è scritto in [typescript](https://www.typescriptlang.org/). È possibile scrivere in JavaScript o TypeScript quando si usa il [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Il client JavaScript è ospitato in [NPM](https://www.npmjs.com/)

Nelle versioni precedenti il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio. Per le versioni principali, il pacchetto NPM di [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript. Questo pacchetto non è incluso nel modello di **applicazione Web ASP.NET Core** . Usare NPM per ottenere e installare il pacchetto NPM `@aspnet/signalr`.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dipendenza da jQuery è stata rimossa, tuttavia i progetti possono comunque usare jQuery.

### <a name="internet-explorer-support"></a>Supporto di Internet Explorer

ASP.NET Core SignalR richiede Microsoft Internet Explorer 11 o versione successiva (ASP.NET SignalR supporta Microsoft Internet Explorer 8 e versioni successive).

### <a name="javascript-client-method-syntax"></a>Sintassi del metodo client JavaScript

La sintassi JavaScript è cambiata rispetto alla versione precedente di SignalR. Anziché usare l'oggetto `$connection`, creare una connessione usando l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Usare il metodo [on](/javascript/api/@aspnet/signalr/HubConnection#on) per specificare i metodi client che l'hub può chiamare.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Dopo aver creato il metodo client, avviare la connessione dell'hub. Concatenare un metodo [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) per registrare o gestire gli errori.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy Hub

I proxy Hub non vengono più generati automaticamente. Al contrario, il nome del metodo viene passato nell'API [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) sotto forma di stringa.

### <a name="net-and-other-clients"></a>.NET e altri client

Il pacchetto NuGet `Microsoft.AspNetCore.SignalR.Client` contiene le librerie client .NET per ASP.NET Core SignalR.

Usare [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) per creare e compilare un'istanza di una connessione a un hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Differenze di scalabilità orizzontale

ASP.NET SignalR supporta SQL Server e Redis. ASP.NET Core SignalR supporta il servizio SignalR di Azure e Redis.

### <a name="aspnet"></a>ASP.NET

* [scalabilità orizzontale di SignalR con il bus di servizio di Azure](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [scalabilità orizzontale di SignalR con Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR la scalabilità orizzontale con SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Servizio SignalR di Azure](/azure/azure-signalr/)
* [Backplane Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Piattaforme supportate](xref:signalr/supported-platforms)
