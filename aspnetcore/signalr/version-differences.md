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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Differenze tra SignalR e dei componenti di base di ASP.NET SignalR

SignalR ASP.NET Core non è compatibile con client e server per ASP.NET SignalR. In questo articolo illustra in dettaglio le funzionalità che sono state rimosse o modificate in ASP.NET Core SignalR.

## <a name="feature-differences"></a>Differenze di funzionalità

### <a name="automatic-reconnects"></a>Riconnessioni automatico

Riconnessioni automatico non sono più supportate. SignalR tentato in precedenza, riconnettersi al server se la connessione è stata eliminata. Se il client è disconnesso l'utente deve avviare una nuova connessione in modo esplicito se desiderano ristabilire la connessione.

### <a name="protocol-support"></a>Supporto del protocollo

ASP.NET SignalR Core supporta JSON, nonché un nuovo protocollo binario in base alle [MessagePack](xref:signalr/messagepackhubprotocol). Inoltre, è possibile creare protocolli personalizzati.

## <a name="differences-on-the-server"></a>Differenze nel server

Le librerie sul lato server di SignalR sono inclusi nel `Microsoft.AspNetCore.App` che fa parte del pacchetto il **applicazione Web di ASP.NET Core** modello per i progetti sia Razor e MVC.

SignalR è un middleware di ASP.NET Core, pertanto deve essere configurata tramite la chiamata `AddSignalR` in `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Per configurare il routing, eseguire il mapping delle route agli hub all'interno di `UseSignalR` chiamata al metodo di `Startup.Configure` metodo.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessioni permanenti ora richiesto

A causa di scalabilità orizzontale come funziona nelle versioni precedenti di SignalR client è stato possibile riconnettersi e inviare messaggi a qualsiasi server nella farm. A causa di modifiche per il modello di scalabilità orizzontale, così come non supporta riconnessioni, ciò non è più supportata. A questo punto, dopo che il client si connette al server deve interagire con lo stesso server per la durata della connessione.

### <a name="single-hub-per-connection"></a>Singolo hub per ogni connessione

In ASP.NET SignalR Core, il modello di connessione è stato semplificato. Le connessioni vengono effettuate direttamente a un singolo hub, anziché una singola connessione usato per condividere l'accesso agli hub più.

### <a name="streaming"></a>Flusso

Supporta ora SignalR [flusso di dati](xref:signalr/streaming) dall'hub al client.

### <a name="state"></a>Stato

La possibilità di passare uno stato arbitrario tra client e l'hub (spesso chiamati HubState) è stato rimosso, oltre al supporto per i messaggi di stato. Al momento non sono alcun equivalente di proxy dell'hub.

## <a name="differences-on-the-client"></a>Differenze nel client

### <a name="typescript"></a>TypeScript

La versione di ASP.NET Core di SignalR è scritto in [TypeScript](https://www.typescriptlang.org/). È possibile scrivere in JavaScript o TypeScript quando si utilizza il [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Il client JavaScript è ospitato in [npm](https://www.npmjs.com/)

Nelle versioni precedenti, il client JavaScript è stato ottenuto tramite un pacchetto NuGet in Visual Studio. Per le versioni dei componenti di base, il [ @aspnet/signalr pacchetto npm](https://www.npmjs.com/package/@aspnet/signalr) contiene le librerie JavaScript. Questo pacchetto non è incluso nel **applicazione Web di ASP.NET Core** modello. Usare npm per ottenere e installare il `@aspnet/signalr` pacchetto npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dipendenza da jQuery è stato rimosso, tuttavia, i progetti possono comunque usare jQuery.

### <a name="javascript-client-method-syntax"></a>Sintassi del metodo di client JavaScript

La sintassi JavaScript è stato modificato dalla versione precedente di SignalR. Invece di usare il `$connection` dell'oggetto, creare una connessione utilizzando il `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Utilizzare `connection.on` per specificare i metodi di client che può chiamare l'hub.

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

### <a name="hub-proxies"></a>Proxy dell'hub

Vengono generati non più automaticamente i proxy dell'hub. Al contrario, il nome del metodo viene passato il `invoke` API sotto forma di stringa.

### <a name="net-and-other-clients"></a>.NET e gli altri client

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto NuGet contiene le librerie client .NET per ASP.NET SignalR Core.

Utilizzare il `HubConnectionBuilder` per creare e compilare un'istanza di una connessione a un hub.

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
