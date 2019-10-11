---
title: Usare il protocollo dell'hub MessagePack in SignalR per ASP.NET Core
author: bradygaster
description: Aggiungere il protocollo dell'hub MessagePack a ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/08/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: fe09b646eba5ae15cbd9e568b276aaf7763e4b1b
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165399"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Usare il protocollo dell'hub MessagePack in SignalR per ASP.NET Core

Di [Brennan Conroy](https://github.com/BrennanConroy)

Questo articolo presuppone che il lettore abbia familiarità con gli argomenti trattati in [Introduzione](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>Che cos'è MessagePack?

[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binario veloce e compatto. È utile quando le prestazioni e la larghezza di banda rappresentano un problema perché crea messaggi più piccoli rispetto a [JSON](https://www.json.org/). Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano le tracce di rete e i log, a meno che i byte non vengano passati tramite un parser MessagePack. SignalR dispone del supporto incorporato per il formato MessagePack e fornisce le API per il client e il server da usare.

## <a name="configure-messagepack-on-the-server"></a>Configurare MessagePack nel server

Per abilitare il protocollo dell'hub MessagePack nel server, installare il pacchetto `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` nell'app. Nel file Startup.cs aggiungere `AddMessagePackProtocol` alla chiamata `AddSignalR` per abilitare il supporto di MessagePack nel server.

> [!NOTE]
> JSON è abilitato per impostazione predefinita. L'aggiunta di MessagePack Abilita il supporto per i client JSON e MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Per personalizzare il modo in cui MessagePack formatterà i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni. In tale delegato è possibile utilizzare la proprietà `FormatterResolvers` per configurare le opzioni di serializzazione MessagePack. Per ulteriori informazioni sul funzionamento dei resolver, visitare la libreria MessagePack in [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Gli attributi possono essere utilizzati per gli oggetti che si desidera serializzare per definire il modo in cui devono essere gestiti.

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

> [!NOTE]
> JSON è abilitato per impostazione predefinita per i client supportati. I client possono supportare un solo protocollo. L'aggiunta del supporto di MessagePack sostituirà tutti i protocolli configurati in precedenza.

### <a name="net-client"></a>Client .NET

Per abilitare MessagePack nel client .NET, installare il pacchetto `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` e chiamare `AddMessagePackProtocol` in `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Questa chiamata `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni esattamente come il server.

### <a name="javascript-client"></a>Client JavaScript

::: moniker range=">= aspnetcore-3.0"

Il supporto di MessagePack per il client JavaScript viene fornito dal pacchetto NPM `@microsoft/signalr-protocol-msgpack`. Installare il pacchetto eseguendo il comando seguente in una shell dei comandi:

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Il supporto di MessagePack per il client JavaScript viene fornito dal pacchetto NPM `@aspnet/signalr-protocol-msgpack`. Installare il pacchetto eseguendo il comando seguente in una shell dei comandi:

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

Dopo l'installazione del pacchetto NPM, il modulo può essere usato direttamente tramite un caricatore di moduli JavaScript o importato nel browser facendo riferimento al file seguente:

::: moniker range=">= aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

In un browser è necessario fare riferimento anche alla libreria `msgpack5`. Usare un tag `<script>` per creare un riferimento. La libreria si trova in *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Quando si usa l'elemento `<script>`, l'ordine è importante. Se si fa riferimento a *SignalR-Protocol-msgpack. js* prima di *msgpack5. js*, si verifica un errore durante il tentativo di connessione con MessagePack. *SignalR. js* è necessario anche prima di *SignalR-Protocol-msgpack. js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Se si aggiunge `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` al `HubConnectionBuilder`, il client utilizzerà il protocollo MessagePack per la connessione a un server.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> Al momento non sono disponibili opzioni di configurazione per il protocollo MessagePack nel client JavaScript.

## <a name="messagepack-quirks"></a>Peculiarità di MessagePack

Quando si usa il protocollo dell'hub MessagePack, è necessario tenere presenti alcuni aspetti.

### <a name="messagepack-is-case-sensitive"></a>MessagePack fa distinzione tra maiuscole e minuscole

Il protocollo MessagePack fa distinzione tra maiuscole e minuscole. Si consideri, ad C# esempio, la classe seguente:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Quando si invia dal client JavaScript, è necessario usare i nomi di proprietà `PascalCased`, perché la combinazione di C# maiuscole e minuscole deve corrispondere esattamente alla classe. Esempio:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

L' C# utilizzo dei nomi `camelCased` non verrà associato correttamente alla classe. Per aggirare questo problema, è possibile usare l'attributo `Key` per specificare un nome diverso per la proprietà MessagePack. Per ulteriori informazioni, vedere [la documentazione di MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime. Kind non viene mantenuto durante la serializzazione/deserializzazione

Il protocollo MessagePack non fornisce un modo per codificare il valore `Kind` di un `DateTime`. Di conseguenza, quando si deserializza una data, il protocollo dell'hub MessagePack presuppone che la data in ingresso sia in formato UTC. Se si usano valori `DateTime` nell'ora locale, è consigliabile eseguire la conversione in formato UTC prima di inviarli. Convertirli dall'ora UTC all'ora locale quando vengono ricevuti.

Per altre informazioni su questa limitazione, vedere il problema relativo a GitHub [/SignalR # 2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime. MinValue non è supportato da MessagePack in JavaScript

La libreria [msgpack5](https://github.com/mcollina/msgpack5) usata dal client JavaScript SignalR non supporta il tipo `timestamp96` in MessagePack. Questo tipo viene usato per codificare i valori di data molto grandi (molto presto in passato o molto lontano in futuro). Il valore di `DateTime.MinValue` è `January 1, 0001` che deve essere codificato in un valore di `timestamp96`. Per questo motivo, l'invio di `DateTime.MinValue` a un client JavaScript non è supportato. Quando il client JavaScript riceve `DateTime.MinValue`, viene generato l'errore seguente:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

In genere, `DateTime.MinValue` viene usato per codificare un valore "Missing" o `null`. Se è necessario codificare il valore in MessagePack, usare un valore Nullable `DateTime` (`DateTime?`) o codificare un valore `bool` separato che indichi se la data è presente.

Per altre informazioni su questa limitazione, vedere il problema relativo a GitHub [/SignalR # 2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Supporto di MessagePack nell'ambiente di compilazione "Ahead of Time"

La libreria [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) utilizzata dal client e dal server .NET utilizza la generazione del codice per ottimizzare la serializzazione. Di conseguenza, non è supportato per impostazione predefinita negli ambienti che usano la compilazione "Ahead of Time" (ad esempio, Novell iOS o Unity). È possibile usare MessagePack in questi ambienti mediante la "pre-generazione" del codice del serializzatore/deserializzatore. Per ulteriori informazioni, vedere [la documentazione di MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Una volta creati in precedenza i serializzatori, è possibile registrarli usando il delegato di configurazione passato a `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>I controlli del tipo sono più severi in MessagePack

Il protocollo dell'hub JSON eseguirà le conversioni dei tipi durante la deserializzazione. Se, ad esempio, l'oggetto in ingresso ha un valore della proprietà che è un numero (`{ foo: 42 }`), ma la proprietà nella classe .NET è di tipo `string`, il valore verrà convertito. Tuttavia, MessagePack non esegue questa conversione e genererà un'eccezione che può essere visualizzata nei log lato server (e nella console):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Per altre informazioni su questa limitazione, vedere il problema relativo a GitHub [/SignalR # 2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
