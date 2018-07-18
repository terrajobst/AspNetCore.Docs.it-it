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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Differenze tra SignalR e ASP.NET Core SignalR

ASP.NET Core SignalR non è compatibile con client o server per ASP.NET SignalR. Questo articolo illustra le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.

## <a name="feature-differences"></a>Differenze di funzionalità

### <a name="automatic-reconnects"></a>Connessioni automatiche

Le connessioni automatica non sono più supportate. SignalR tentato in precedenza, riconnettersi al server se la connessione è stata eliminata. Se il client viene disconnesso ora l'utente deve avviare una nuova connessione in modo esplicito se si vuole riconnettere.

### <a name="protocol-support"></a>Supporto del protocollo

ASP.NET Core SignalR supporta JSON, nonché un nuovo protocollo binario basato sul [MessagePack](xref:signalr/messagepackhubprotocol). Inoltre, è possibile creare i protocolli personalizzati.

## <a name="differences-on-the-server"></a>Comprendere le differenze tra il server

Le librerie sul lato server di SignalR sono incluse nel `Microsoft.AspNetCore.App` che fa parte del pacchetto il **applicazione Web ASP.NET Core** modello per i progetti sia Razor MVC.

SignalR è un middleware di ASP.NET Core, pertanto deve essere configurato tramite la chiamata `AddSignalR` in `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Per configurare il routing, eseguire il mapping di route per gli hub all'interno di `UseSignalR` chiamata al metodo il `Startup.Configure` (metodo).

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessioni permanenti ora richiesto

A causa di scalabilità orizzontale come ha funzionato nelle versioni precedenti di SignalR, i client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm. A causa di modifiche per il modello di scalabilità orizzontale, nonché non sono supportate le connessioni, ciò non è più supportata. A questo punto, dopo che il client si connette al server deve interagire con lo stesso server per la durata della connessione.

### <a name="single-hub-per-connection"></a>Hub singolo per ogni connessione

In ASP.NET Core SignalR, il modello di connessione è stato semplificato. Le connessioni vengono effettuate direttamente da un singolo hub, anziché una singola connessione utilizzato per condividere l'accesso a più hub.

### <a name="streaming"></a>Flusso

Supporta ora SignalR [dati in streaming](xref:signalr/streaming) dall'hub al client.

### <a name="state"></a>Stato

La possibilità di passare informazioni sullo stato arbitrarie tra client e l'hub (spesso chiamati HubState) è stato rimosso, nonché il supporto per i messaggi di stato. Nel momento in cui non vi è alcun equivalente di proxy dell'hub.

## <a name="differences-on-the-client"></a>Comprendere le differenze tra il client

### <a name="typescript"></a>TypeScript

La versione di ASP.NET Core di SignalR è scritto in [TypeScript](https://www.typescriptlang.org/). È possibile scrivere in JavaScript o TypeScript quando si usa la [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)

Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio. Per le versioni di base, il [ @aspnet/signalr pacchetto npm](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript. Questo pacchetto non è incluso nel **applicazione Web ASP.NET Core** modello. Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dipendenza da jQuery è stato rimosso, ma i progetti possono comunque usare jQuery.

### <a name="javascript-client-method-syntax"></a>Sintassi del metodo client JavaScript

La sintassi JavaScript è stato modificato dalla versione precedente di SignalR. Invece di usare la `$connection` dell'oggetto, creare una connessione usando il `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Usare `connection.on` per specificare metodi di client può chiamare l'hub.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Dopo aver creato il metodo di client, avviare la connessione dell'hub. Catena un `catch` metodo per accedere o gestire gli errori.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxy di hub

Vengono generati non più automaticamente i proxy dell'hub. Il nome del metodo viene invece passato nella `invoke` API sotto forma di stringa.

### <a name="net-and-other-clients"></a>.NET e altri client

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET Core SignalR.

Usare il `HubConnectionBuilder` per creare e compilare un'istanza di una connessione a un hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Piattaforme supportate](xref:signalr/supported-platforms)
