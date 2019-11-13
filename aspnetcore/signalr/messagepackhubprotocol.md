---
title: Usare il protocollo dell'hub MessagePack in SignalR per ASP.NET Core
author: bradygaster
description: Aggiungere il protocollo dell'hub MessagePack al SignalRASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: cd052a97db1e20d6c7aa00f47cf6a7d01a9bc305
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963758"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="d8c89-103">Usare il protocollo dell'hub MessagePack in SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8c89-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="d8c89-104">Di [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="d8c89-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="d8c89-105">Questo articolo presuppone che il lettore abbia familiarità con gli argomenti trattati in [Introduzione](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="d8c89-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="d8c89-106">Che cos'è MessagePack?</span><span class="sxs-lookup"><span data-stu-id="d8c89-106">What is MessagePack?</span></span>

<span data-ttu-id="d8c89-107">[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binario veloce e compatto.</span><span class="sxs-lookup"><span data-stu-id="d8c89-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="d8c89-108">È utile quando le prestazioni e la larghezza di banda rappresentano un problema perché crea messaggi più piccoli rispetto a [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="d8c89-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="d8c89-109">Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano le tracce di rete e i log, a meno che i byte non vengano passati tramite un parser MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="d8c89-110"> dispone del supporto incorporato per il formato MessagePack e fornisce le API per il client e il server da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="d8c89-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="d8c89-111">Configurare MessagePack nel server</span><span class="sxs-lookup"><span data-stu-id="d8c89-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="d8c89-112">Per abilitare il protocollo dell'hub MessagePack nel server, installare il pacchetto di `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` nell'app.</span><span class="sxs-lookup"><span data-stu-id="d8c89-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="d8c89-113">Nel file Startup.cs aggiungere `AddMessagePackProtocol` alla chiamata `AddSignalR` per abilitare il supporto di MessagePack nel server.</span><span class="sxs-lookup"><span data-stu-id="d8c89-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="d8c89-114">JSON è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d8c89-114">JSON is enabled by default.</span></span> <span data-ttu-id="d8c89-115">L'aggiunta di MessagePack Abilita il supporto per i client JSON e MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="d8c89-116">Per personalizzare il modo in cui MessagePack formatterà i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="d8c89-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="d8c89-117">In tale delegato è possibile utilizzare la proprietà `FormatterResolvers` per configurare le opzioni di serializzazione MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="d8c89-118">Per ulteriori informazioni sul funzionamento dei resolver, visitare la libreria MessagePack in [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="d8c89-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="d8c89-119">Gli attributi possono essere utilizzati per gli oggetti che si desidera serializzare per definire il modo in cui devono essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="d8c89-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="d8c89-120">Configurare MessagePack nel client</span><span class="sxs-lookup"><span data-stu-id="d8c89-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="d8c89-121">JSON è abilitato per impostazione predefinita per i client supportati.</span><span class="sxs-lookup"><span data-stu-id="d8c89-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="d8c89-122">I client possono supportare un solo protocollo.</span><span class="sxs-lookup"><span data-stu-id="d8c89-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="d8c89-123">L'aggiunta del supporto di MessagePack sostituirà tutti i protocolli configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d8c89-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="d8c89-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="d8c89-124">.NET client</span></span>

<span data-ttu-id="d8c89-125">Per abilitare MessagePack nel client .NET, installare il pacchetto di `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` e chiamare `AddMessagePackProtocol` in `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="d8c89-126">Questa chiamata `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni esattamente come il server.</span><span class="sxs-lookup"><span data-stu-id="d8c89-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="d8c89-127">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="d8c89-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d8c89-128">Il supporto di MessagePack per il client JavaScript viene fornito dal pacchetto NPM `@microsoft/signalr-protocol-msgpack`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="d8c89-129">Installare il pacchetto eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d8c89-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d8c89-130">Il supporto di MessagePack per il client JavaScript viene fornito dal pacchetto NPM `@aspnet/signalr-protocol-msgpack`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="d8c89-131">Installare il pacchetto eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d8c89-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="d8c89-132">Dopo l'installazione del pacchetto NPM, il modulo può essere usato direttamente tramite un caricatore di moduli JavaScript o importato nel browser facendo riferimento al file seguente:</span><span class="sxs-lookup"><span data-stu-id="d8c89-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d8c89-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="d8c89-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d8c89-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="d8c89-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="d8c89-135">In un browser è necessario fare riferimento anche alla libreria `msgpack5`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="d8c89-136">Usare un tag `<script>` per creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="d8c89-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="d8c89-137">La libreria si trova in *node_modules \msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="d8c89-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="d8c89-138">Quando si usa l'elemento `<script>`, l'ordine è importante.</span><span class="sxs-lookup"><span data-stu-id="d8c89-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="d8c89-139">Se si fa riferimento a *SignalR-Protocol-msgpack. js* prima di *msgpack5. js*, si verifica un errore durante il tentativo di connessione con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="d8c89-140">*SignalR. js* è necessario anche prima di *SignalR-Protocol-msgpack. js*.</span><span class="sxs-lookup"><span data-stu-id="d8c89-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="d8c89-141">Se si aggiunge `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` al `HubConnectionBuilder`, il client viene configurato per l'utilizzo del protocollo MessagePack durante la connessione a un server.</span><span class="sxs-lookup"><span data-stu-id="d8c89-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="d8c89-142">Al momento non sono disponibili opzioni di configurazione per il protocollo MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8c89-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="d8c89-143">Peculiarità di MessagePack</span><span class="sxs-lookup"><span data-stu-id="d8c89-143">MessagePack quirks</span></span>

<span data-ttu-id="d8c89-144">Quando si usa il protocollo dell'hub MessagePack, è necessario tenere presenti alcuni aspetti.</span><span class="sxs-lookup"><span data-stu-id="d8c89-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="d8c89-145">MessagePack fa distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="d8c89-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="d8c89-146">Il protocollo MessagePack fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d8c89-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="d8c89-147">Si consideri, ad C# esempio, la classe seguente:</span><span class="sxs-lookup"><span data-stu-id="d8c89-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="d8c89-148">Quando si invia dal client JavaScript, è necessario usare `PascalCased` nomi di proprietà, poiché la combinazione di maiuscole e minuscole deve corrispondere esattamente alla C# classe.</span><span class="sxs-lookup"><span data-stu-id="d8c89-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="d8c89-149">Esempio:</span><span class="sxs-lookup"><span data-stu-id="d8c89-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="d8c89-150">L' C# utilizzo di nomi di `camelCased` non verrà associato correttamente alla classe.</span><span class="sxs-lookup"><span data-stu-id="d8c89-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="d8c89-151">Per aggirare questo problema, è possibile usare l'attributo `Key` per specificare un nome diverso per la proprietà MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="d8c89-152">Per ulteriori informazioni, vedere [la documentazione di MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="d8c89-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="d8c89-153">DateTime. Kind non viene mantenuto durante la serializzazione/deserializzazione</span><span class="sxs-lookup"><span data-stu-id="d8c89-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="d8c89-154">Il protocollo MessagePack non fornisce un modo per codificare il valore `Kind` di un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="d8c89-155">Di conseguenza, quando si deserializza una data, il protocollo dell'hub MessagePack presuppone che la data in ingresso sia in formato UTC.</span><span class="sxs-lookup"><span data-stu-id="d8c89-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="d8c89-156">Se si usano `DateTime` valori nell'ora locale, è consigliabile eseguire la conversione in formato UTC prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="d8c89-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="d8c89-157">Convertirli dall'ora UTC all'ora locale quando vengono ricevuti.</span><span class="sxs-lookup"><span data-stu-id="d8c89-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="d8c89-158">Per ulteriori informazioni su questa limitazione, vedere la pagina relativa ai problemi di [#2632 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2632)di GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8c89-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="d8c89-159">DateTime. MinValue non è supportato da MessagePack in JavaScript</span><span class="sxs-lookup"><span data-stu-id="d8c89-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="d8c89-160">La libreria [msgpack5](https://github.com/mcollina/msgpack5) usata dal client JavaScript SignalR non supporta il tipo di `timestamp96` in MessagePack.</span><span class="sxs-lookup"><span data-stu-id="d8c89-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="d8c89-161">Questo tipo viene usato per codificare i valori di data molto grandi (molto presto in passato o molto lontano in futuro).</span><span class="sxs-lookup"><span data-stu-id="d8c89-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="d8c89-162">Il valore di `DateTime.MinValue` è `January 1, 0001` che deve essere codificato in un valore di `timestamp96`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="d8c89-163">Per questo motivo, l'invio di `DateTime.MinValue` a un client JavaScript non è supportato.</span><span class="sxs-lookup"><span data-stu-id="d8c89-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="d8c89-164">Quando `DateTime.MinValue` viene ricevuto dal client JavaScript, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="d8c89-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="d8c89-165">In genere, `DateTime.MinValue` viene usato per codificare un valore "Missing" o `null`.</span><span class="sxs-lookup"><span data-stu-id="d8c89-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="d8c89-166">Se è necessario codificare il valore in MessagePack, usare un valore di `DateTime` Nullable (`DateTime?`) o codificare un valore `bool` separato che indichi se la data è presente.</span><span class="sxs-lookup"><span data-stu-id="d8c89-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="d8c89-167">Per ulteriori informazioni su questa limitazione, vedere la pagina relativa ai problemi di [#2228 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2228)di GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8c89-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="d8c89-168">Supporto di MessagePack nell'ambiente di compilazione "Ahead of Time"</span><span class="sxs-lookup"><span data-stu-id="d8c89-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="d8c89-169">La libreria [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) utilizzata dal client e dal server .NET utilizza la generazione del codice per ottimizzare la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="d8c89-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="d8c89-170">Di conseguenza, non è supportato per impostazione predefinita negli ambienti che usano la compilazione "Ahead of Time" (ad esempio, Novell iOS o Unity).</span><span class="sxs-lookup"><span data-stu-id="d8c89-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="d8c89-171">È possibile usare MessagePack in questi ambienti mediante la "pre-generazione" del codice del serializzatore/deserializzatore.</span><span class="sxs-lookup"><span data-stu-id="d8c89-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="d8c89-172">Per ulteriori informazioni, vedere [la documentazione di MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="d8c89-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="d8c89-173">Dopo aver generato i serializzatori, è possibile registrarli usando il delegato di configurazione passato a `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="d8c89-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="d8c89-174">I controlli del tipo sono più severi in MessagePack</span><span class="sxs-lookup"><span data-stu-id="d8c89-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="d8c89-175">Il protocollo dell'hub JSON eseguirà le conversioni dei tipi durante la deserializzazione.</span><span class="sxs-lookup"><span data-stu-id="d8c89-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="d8c89-176">Se, ad esempio, l'oggetto in ingresso ha un valore della proprietà che è un numero (`{ foo: 42 }`), ma la proprietà nella classe .NET è di tipo `string`, il valore verrà convertito.</span><span class="sxs-lookup"><span data-stu-id="d8c89-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="d8c89-177">Tuttavia, MessagePack non esegue questa conversione e genererà un'eccezione che può essere visualizzata nei log lato server (e nella console):</span><span class="sxs-lookup"><span data-stu-id="d8c89-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="d8c89-178">Per ulteriori informazioni su questa limitazione, vedere la pagina relativa ai problemi di [#2937 ASPNET/SignalR](https://github.com/aspnet/SignalR/issues/2937)di GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8c89-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="d8c89-179">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="d8c89-179">Related resources</span></span>

* [<span data-ttu-id="d8c89-180">Introduzione</span><span class="sxs-lookup"><span data-stu-id="d8c89-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="d8c89-181">Client .NET</span><span class="sxs-lookup"><span data-stu-id="d8c89-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="d8c89-182">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="d8c89-182">JavaScript client</span></span>](xref:signalr/javascript-client)
