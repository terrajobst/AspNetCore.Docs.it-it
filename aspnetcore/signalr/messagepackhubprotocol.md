---
title: Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core
author: bradygaster
description: Aggiungere il protocollo Hub MessagePack per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896518"
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

Il supporto per il client JavaScript MessagePack avviene tramite la `@aspnet/signalr-protocol-msgpack` pacchetto npm.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore di modulo JavaScript o importato nel browser facendo il *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file. In un browser, il `msgpack5` libreria è necessario inoltre fare riferimento. Usare un `<script>` tag per creare un riferimento. La libreria reperibili *node_modules\msgpack5\dist\msgpack5.js*.

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

## <a name="messagepack-quirks"></a>MessagePack quirks

Esistono alcuni problemi da tenere presenti quando si usa il protocollo Hub MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack è distinzione maiuscole/minuscole

Il protocollo MessagePack fa distinzione maiuscole/minuscole. Ad esempio, tenere presente quanto segue C# classe:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Quando vengono inviati dal client JavaScript, è necessario utilizzare `PascalCased` i nomi delle proprietà, poiché le maiuscole e minuscole devono corrispondere il C# classe esattamente. Ad esempio:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Usando `camelCased` per non associare correttamente i nomi di C# classe. È possibile risolvere il problema usando il `Key` attributo per specificare un nome diverso per la proprietà MessagePack. Per altre informazioni, vedere [la documentazione MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime. Kind non viene mantenuto durante la serializzazione/deserializzazione

Il protocollo MessagePack non fornisce un modo per codificare il `Kind` valore di un `DateTime`. Di conseguenza, durante la deserializzazione di una data, il protocollo Hub MessagePack presuppone che la data in ingresso è in formato UTC. Se si lavora con `DateTime` valori nell'ora locale, si consiglia di conversione in formato UTC prima di inviarli. Convertirli rispetto all'ora UTC nell'ora locale quando si ricevono.

Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>MinValue non è supportato da MessagePack in JavaScript

Il [msgpack5](https://github.com/mcollina/msgpack5) libreria usata dal client SignalR JavaScript non supporta il `timestamp96` tipo in MessagePack. Questo tipo viene utilizzato per codificare i valori delle date di dimensioni molto grandi (in molto presto in passato o a un momento molto lontano nel futuro). Il valore di `DateTime.MinValue` viene `January 1, 0001` che deve essere codificato un `timestamp96` valore. A causa di questa operazione, l'invio `DateTime.MinValue` per JavaScript client non è supportato. Quando si `DateTime.MinValue` viene ricevuto dal client JavaScript, viene generato l'errore seguente:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

In genere `DateTime.MinValue` viene usato per codificare un "missing" o `null` valore. Se è necessario codificare il valore in MessagePack, usare un valore nullable `DateTime` valore (`DateTime?`) o codificare un oggetto separato `bool` valore che indica se la data è presente.

Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Supporto MessagePack nell'ambiente di compilazione "ahead of time"

Il [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) libreria usata dal client .NET e server utilizza la generazione di codice per ottimizzare la serializzazione. Di conseguenza, non è supportato per impostazione predefinita in ambienti che utilizzano la compilazione "ahead of time" (ad esempio iOS di Xamarin e Unity). È possibile usare MessagePack in questi ambienti generando"pre-" il codice del serializzatore/deserializzatore. Per altre informazioni, vedere [la documentazione MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Dopo aver pre-generato i serializzatori, è possibile registrarli utilizzando il delegato di configurazione passato a `AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>Controlli di tipo sono più rigorose in MessagePack

Il protocollo Hub JSON eseguirà le conversioni di tipo durante la deserializzazione. Ad esempio, se l'oggetto in ingresso ha un valore della proprietà che è un numero (`{ foo: 42 }`) ma la proprietà sulla classe .NET è di tipo `string`, il valore verrà convertito. Tuttavia, MessagePack non esegue questa conversione e verrà generata un'eccezione che può essere visualizzata nel file di log lato server (e nella console):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
