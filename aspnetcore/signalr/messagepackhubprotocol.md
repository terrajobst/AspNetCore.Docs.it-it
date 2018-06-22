---
title: Utilizzare il protocollo di MessagePack Hub SignalR per ASP.NET Core
author: rachelappel
description: Aggiungere protocollo MessagePack Hub SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274989"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Utilizzare il protocollo di MessagePack Hub SignalR per ASP.NET Core

Da [Brennan Conroy](https://github.com/BrennanConroy)

Questo articolo si presuppone il lettore ha familiarità con gli argomenti inclusi in [iniziare a](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Che cos'è MessagePack?

[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binaria è veloce e compact. È utile per la larghezza di banda e le prestazioni rappresentano un problema perché crea messaggi più piccoli rispetto al [JSON](https://www.json.org/). Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano i log e le tracce di rete, a meno che i byte vengono passati attraverso un parser MessagePack. SignalR include il supporto incorporato per il formato MessagePack e fornisce le API per il client e server da utilizzare.

## <a name="configure-messagepack-on-the-server"></a>Configurare MessagePack nel server

Per abilitare il protocollo di Hub MessagePack nel server, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto dell'app. Nel file Startup.cs aggiungere `AddMessagePackProtocol` per il `AddSignalR` chiamata per abilitare il supporto di MessagePack nel server.

> [!NOTE]
> JSON è abilitata per impostazione predefinita. Aggiunta di MessagePack Abilita il supporto per JSON e MessagePack i client.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Per personalizzare il modo MessagePack deve formattare i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione di opzioni. Nel delegato, il `FormatterResolvers` proprietà può essere utilizzata per configurare le opzioni di serializzazione MessagePack. Per ulteriori informazioni sul funzionamento delle funzioni i resolver, visitare la raccolta MessagePack al [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Gli attributi possono essere utilizzati per gli oggetti che si desidera serializzare per definire la modalità deve essere gestite.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>Configurare MessagePack nel client

### <a name="net-client"></a>Client .NET

Per abilitare MessagePack nel Client di .NET, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto e chiamare `AddMessagePackProtocol` su `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Ciò `AddMessagePackProtocol` chiamare accetta un delegato per la configurazione di opzioni come il server.

### <a name="javascript-client"></a>Client JavaScript

Supporto MessagePack per il client Javascript viene fornito dal `@aspnet/signalr-protocol-msgpack` pacchetto NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore del modulo di JavaScript o importato nel browser facendo riferimento il *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  file. In un browser il `msgpack5` libreria è necessario inoltre fare riferimento. Utilizzare un `<script>` tag per creare un riferimento. La libreria è reperibile in *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Quando si utilizza il `<script>` elemento, l'ordine è importante. Se *signalr-protocollo-msgpack.js* viene fatto riferimento prima *msgpack5.js*, si verifica un errore durante il tentativo di connettersi con MessagePack. *SignalR.js* è inoltre necessaria prima *signalr-protocollo-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Aggiunta `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` per il `HubConnectionBuilder` verrà configurato il client per usare il protocollo MessagePack quando ci si connette a un server.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> In questo momento, non esistono alcuna opzione di configurazione per il protocollo MessagePack sul client JavaScript.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
