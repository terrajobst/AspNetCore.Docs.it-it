---
title: Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core
author: tdykstra
description: Aggiungere il protocollo Hub MessagePack per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: c04834b0d395d08782b51b56e79badba078a5b91
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794837"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core

Da [Brennan Conroy](https://github.com/BrennanConroy)

Questo articolo presuppone che il lettore abbia familiarità con gli argomenti trattati in [iniziare a usare](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Che cos'è MessagePack?

[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binaria che è veloce e compatto. È utile quando le prestazioni e della larghezza di banda rappresentano un problema perché crea messaggi più piccoli rispetto alla [JSON](https://www.json.org/). Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano i log e tracce di rete, a meno che i byte vengono passati tramite un parser MessagePack. SignalR include supporto predefinito per il formato MessagePack e fornisce le API per il client e server da usare.

## <a name="configure-messagepack-on-the-server"></a>Configurare MessagePack nel server

Per abilitare il protocollo Hub MessagePack sul server, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto nell'app. Nel file Startup.cs aggiungere `AddMessagePackProtocol` per il `AddSignalR` chiamata per abilitare il supporto MessagePack nel server.

> [!NOTE]
> JSON è abilitato per impostazione predefinita. Aggiunta di MessagePack Abilita il supporto per JSON e MessagePack client.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Per personalizzare il modo MessagePack deve formattare i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni. Nel delegato, di `FormatterResolvers` proprietà può essere utilizzata per configurare le opzioni di serializzazione MessagePack. Per altre informazioni sul funzionamento di sistemi di risoluzione, visitare la libreria MessagePack all'indirizzo [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Gli attributi possono essere utilizzati per gli oggetti da serializzare per definire come deve essere gestite.

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

## <a name="configure-messagepack-on-the-client"></a>Configurare MessagePack sul client

> [!NOTE]
> JSON è abilitato per impostazione predefinita per i client supportati. I client possono supportare solo un singolo protocollo. Aggiunta di supporto MessagePack qualsiasi sostituirà precedentemente configurati i protocolli.

### <a name="net-client"></a>Client .NET

Per abilitare MessagePack nel Client .NET, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto e chiamare `AddMessagePackProtocol` sul `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Ciò `AddMessagePackProtocol` chiamata accetta un delegato per la configurazione di opzioni come il server.

### <a name="javascript-client"></a>Client JavaScript

Il supporto per il client Javascript MessagePack avviene tramite la `@aspnet/signalr-protocol-msgpack` pacchetto NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore di modulo JavaScript o importato nel browser facendo il *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file. In un browser il `msgpack5` libreria è necessario inoltre fare riferimento. Usare un `<script>` tag per creare un riferimento. La libreria reperibili *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Quando si usa il `<script>` elemento, l'ordine è importante. Se *signalr-protocol-msgpack.js* fa riferimento prima *msgpack5.js*, si verifica un errore quando provano a connettersi con MessagePack. *SignalR.js* è inoltre necessaria prima *signalr-protocol-msgpack.js*.

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
> A questo punto, non esistono alcuna opzione di configurazione per il protocollo MessagePack nel client JavaScript.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
