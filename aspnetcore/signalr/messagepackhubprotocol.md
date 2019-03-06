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
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400671"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="cbf4a-103">Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbf4a-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="cbf4a-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="cbf4a-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="cbf4a-105">Questo articolo presuppone che il lettore abbia familiarità con gli argomenti trattati in [iniziare a usare](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="cbf4a-106">Che cos'è MessagePack?</span><span class="sxs-lookup"><span data-stu-id="cbf4a-106">What is MessagePack?</span></span>

<span data-ttu-id="cbf4a-107">[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binaria che è veloce e compatto.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="cbf4a-108">È utile quando le prestazioni e della larghezza di banda rappresentano un problema perché crea messaggi più piccoli rispetto alla [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="cbf4a-109">Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano i log e tracce di rete, a meno che i byte vengono passati tramite un parser MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="cbf4a-110">SignalR include supporto predefinito per il formato MessagePack e fornisce le API per il client e server da usare.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="cbf4a-111">Configurare MessagePack nel server</span><span class="sxs-lookup"><span data-stu-id="cbf4a-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="cbf4a-112">Per abilitare il protocollo Hub MessagePack sul server, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto nell'app.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="cbf4a-113">Nel file Startup.cs aggiungere `AddMessagePackProtocol` per il `AddSignalR` chiamata per abilitare il supporto MessagePack nel server.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="cbf4a-114">JSON è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-114">JSON is enabled by default.</span></span> <span data-ttu-id="cbf4a-115">Aggiunta di MessagePack Abilita il supporto per JSON e MessagePack client.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="cbf4a-116">Per personalizzare il modo MessagePack deve formattare i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="cbf4a-117">Nel delegato, di `FormatterResolvers` proprietà può essere utilizzata per configurare le opzioni di serializzazione MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="cbf4a-118">Per altre informazioni sul funzionamento di sistemi di risoluzione, visitare la libreria MessagePack all'indirizzo [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="cbf4a-119">Gli attributi possono essere utilizzati per gli oggetti da serializzare per definire come deve essere gestite.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="cbf4a-120">Configurare MessagePack sul client</span><span class="sxs-lookup"><span data-stu-id="cbf4a-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="cbf4a-121">JSON è abilitato per impostazione predefinita per i client supportati.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="cbf4a-122">I client possono supportare solo un singolo protocollo.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="cbf4a-123">Aggiunta di supporto MessagePack qualsiasi sostituirà precedentemente configurati i protocolli.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="cbf4a-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="cbf4a-124">.NET client</span></span>

<span data-ttu-id="cbf4a-125">Per abilitare MessagePack nel Client .NET, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto e chiamare `AddMessagePackProtocol` sul `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="cbf4a-126">Ciò `AddMessagePackProtocol` chiamata accetta un delegato per la configurazione di opzioni come il server.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="cbf4a-127">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbf4a-127">JavaScript client</span></span>

<span data-ttu-id="cbf4a-128">Il supporto per il client JavaScript MessagePack avviene tramite la `@aspnet/signalr-protocol-msgpack` pacchetto npm.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="cbf4a-129">Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore di modulo JavaScript o importato nel browser facendo il *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="cbf4a-130">In un browser, il `msgpack5` libreria è necessario inoltre fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="cbf4a-131">Usare un `<script>` tag per creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="cbf4a-132">La libreria reperibili *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="cbf4a-133">Quando si usa il `<script>` elemento, l'ordine è importante.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="cbf4a-134">Se *signalr-protocol-msgpack.js* fa riferimento prima *msgpack5.js*, si verifica un errore quando provano a connettersi con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="cbf4a-135">*SignalR.js* è inoltre necessaria prima *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="cbf4a-136">Aggiunta `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` per il `HubConnectionBuilder` verrà configurato il client per usare il protocollo MessagePack quando ci si connette a un server.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="cbf4a-137">A questo punto, non esistono alcuna opzione di configurazione per il protocollo MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="cbf4a-138">MessagePack quirks</span><span class="sxs-lookup"><span data-stu-id="cbf4a-138">MessagePack quirks</span></span>

<span data-ttu-id="cbf4a-139">Esistono alcuni problemi da tenere presenti quando si usa il protocollo Hub MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="cbf4a-140">MessagePack è distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="cbf4a-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="cbf4a-141">Il protocollo MessagePack fa distinzione maiuscole/minuscole.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="cbf4a-142">Ad esempio, tenere presente quanto segue C# classe:</span><span class="sxs-lookup"><span data-stu-id="cbf4a-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="cbf4a-143">Quando vengono inviati dal client JavaScript, è necessario utilizzare `PascalCased` i nomi delle proprietà, poiché le maiuscole e minuscole devono corrispondere il C# classe esattamente.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="cbf4a-144">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cbf4a-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="cbf4a-145">Usando `camelCased` per non associare correttamente i nomi di C# classe.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="cbf4a-146">È possibile risolvere il problema usando il `Key` attributo per specificare un nome diverso per la proprietà MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="cbf4a-147">Per altre informazioni, vedere [la documentazione MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="cbf4a-148">DateTime. Kind non viene mantenuto durante la serializzazione/deserializzazione</span><span class="sxs-lookup"><span data-stu-id="cbf4a-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="cbf4a-149">Il protocollo MessagePack non fornisce un modo per codificare il `Kind` valore di un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="cbf4a-150">Di conseguenza, durante la deserializzazione di una data, il protocollo Hub MessagePack presuppone che la data in ingresso è in formato UTC.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="cbf4a-151">Se si lavora con `DateTime` valori nell'ora locale, si consiglia di conversione in formato UTC prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="cbf4a-152">Convertirli rispetto all'ora UTC nell'ora locale quando si ricevono.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="cbf4a-153">Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="cbf4a-154">MinValue non è supportato da MessagePack in JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbf4a-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="cbf4a-155">Il [msgpack5](https://github.com/mcollina/msgpack5) libreria usata dal client SignalR JavaScript non supporta il `timestamp96` tipo in MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="cbf4a-156">Questo tipo viene utilizzato per codificare i valori delle date di dimensioni molto grandi (in molto presto in passato o a un momento molto lontano nel futuro).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="cbf4a-157">Il valore di `DateTime.MinValue` viene `January 1, 0001` che deve essere codificato un `timestamp96` valore.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="cbf4a-158">A causa di questa operazione, l'invio `DateTime.MinValue` per JavaScript client non è supportato.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="cbf4a-159">Quando si `DateTime.MinValue` viene ricevuto dal client JavaScript, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="cbf4a-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="cbf4a-160">In genere `DateTime.MinValue` viene usato per codificare un "missing" o `null` valore.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="cbf4a-161">Se è necessario codificare il valore in MessagePack, usare un valore nullable `DateTime` valore (`DateTime?`) o codificare un oggetto separato `bool` valore che indica se la data è presente.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="cbf4a-162">Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="cbf4a-163">Supporto MessagePack nell'ambiente di compilazione "ahead of time"</span><span class="sxs-lookup"><span data-stu-id="cbf4a-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="cbf4a-164">Il [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) libreria usata dal client .NET e server utilizza la generazione di codice per ottimizzare la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="cbf4a-165">Di conseguenza, non è supportato per impostazione predefinita in ambienti che utilizzano la compilazione "ahead of time" (ad esempio iOS di Xamarin e Unity).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="cbf4a-166">È possibile usare MessagePack in questi ambienti generando"pre-" il codice del serializzatore/deserializzatore.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="cbf4a-167">Per altre informazioni, vedere [la documentazione MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="cbf4a-168">Dopo aver pre-generato i serializzatori, è possibile registrarli utilizzando il delegato di configurazione passato a `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="cbf4a-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="cbf4a-169">Controlli di tipo sono più rigorose in MessagePack</span><span class="sxs-lookup"><span data-stu-id="cbf4a-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="cbf4a-170">Il protocollo Hub JSON eseguirà le conversioni di tipo durante la deserializzazione.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="cbf4a-171">Ad esempio, se l'oggetto in ingresso ha un valore della proprietà che è un numero (`{ foo: 42 }`) ma la proprietà sulla classe .NET è di tipo `string`, il valore verrà convertito.</span><span class="sxs-lookup"><span data-stu-id="cbf4a-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="cbf4a-172">Tuttavia, MessagePack non esegue questa conversione e verrà generata un'eccezione che può essere visualizzata nel file di log lato server (e nella console):</span><span class="sxs-lookup"><span data-stu-id="cbf4a-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="cbf4a-173">Per altre informazioni su questa limitazione, vedere GitHub problema [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="cbf4a-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="cbf4a-174">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="cbf4a-174">Related resources</span></span>

* [<span data-ttu-id="cbf4a-175">Introduzione</span><span class="sxs-lookup"><span data-stu-id="cbf4a-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cbf4a-176">Client .NET</span><span class="sxs-lookup"><span data-stu-id="cbf4a-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="cbf4a-177">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbf4a-177">JavaScript client</span></span>](xref:signalr/javascript-client)
