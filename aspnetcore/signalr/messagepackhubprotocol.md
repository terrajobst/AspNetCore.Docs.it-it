---
title: Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core
author: tdykstra
description: Aggiungere il protocollo Hub MessagePack per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094995"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="50c3c-103">Usare il protocollo Hub MessagePack in SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50c3c-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="50c3c-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="50c3c-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="50c3c-105">Questo articolo presuppone che il lettore abbia familiarità con gli argomenti trattati in [iniziare a usare](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="50c3c-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="50c3c-106">Che cos'è MessagePack?</span><span class="sxs-lookup"><span data-stu-id="50c3c-106">What is MessagePack?</span></span>

<span data-ttu-id="50c3c-107">[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binaria che è veloce e compatto.</span><span class="sxs-lookup"><span data-stu-id="50c3c-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="50c3c-108">È utile quando le prestazioni e della larghezza di banda rappresentano un problema perché crea messaggi più piccoli rispetto alla [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="50c3c-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="50c3c-109">Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano i log e tracce di rete, a meno che i byte vengono passati tramite un parser MessagePack.</span><span class="sxs-lookup"><span data-stu-id="50c3c-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="50c3c-110">SignalR include supporto predefinito per il formato MessagePack e fornisce le API per il client e server da usare.</span><span class="sxs-lookup"><span data-stu-id="50c3c-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="50c3c-111">Configurare MessagePack nel server</span><span class="sxs-lookup"><span data-stu-id="50c3c-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="50c3c-112">Per abilitare il protocollo Hub MessagePack sul server, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto nell'app.</span><span class="sxs-lookup"><span data-stu-id="50c3c-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="50c3c-113">Nel file Startup.cs aggiungere `AddMessagePackProtocol` per il `AddSignalR` chiamata per abilitare il supporto MessagePack nel server.</span><span class="sxs-lookup"><span data-stu-id="50c3c-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="50c3c-114">JSON è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="50c3c-114">JSON is enabled by default.</span></span> <span data-ttu-id="50c3c-115">Aggiunta di MessagePack Abilita il supporto per JSON e MessagePack client.</span><span class="sxs-lookup"><span data-stu-id="50c3c-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="50c3c-116">Per personalizzare il modo MessagePack deve formattare i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="50c3c-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="50c3c-117">Nel delegato, di `FormatterResolvers` proprietà può essere utilizzata per configurare le opzioni di serializzazione MessagePack.</span><span class="sxs-lookup"><span data-stu-id="50c3c-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="50c3c-118">Per altre informazioni sul funzionamento di sistemi di risoluzione, visitare la libreria MessagePack all'indirizzo [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="50c3c-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="50c3c-119">Gli attributi possono essere utilizzati per gli oggetti da serializzare per definire come deve essere gestite.</span><span class="sxs-lookup"><span data-stu-id="50c3c-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="50c3c-120">Configurare MessagePack sul client</span><span class="sxs-lookup"><span data-stu-id="50c3c-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="50c3c-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="50c3c-121">.NET client</span></span>

<span data-ttu-id="50c3c-122">Per abilitare MessagePack nel Client .NET, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto e chiamare `AddMessagePackProtocol` sul `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="50c3c-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="50c3c-123">Ciò `AddMessagePackProtocol` chiamata accetta un delegato per la configurazione di opzioni come il server.</span><span class="sxs-lookup"><span data-stu-id="50c3c-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="50c3c-124">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="50c3c-124">JavaScript client</span></span>

<span data-ttu-id="50c3c-125">Il supporto per il client Javascript MessagePack avviene tramite la `@aspnet/signalr-protocol-msgpack` pacchetto NPM.</span><span class="sxs-lookup"><span data-stu-id="50c3c-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="50c3c-126">Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore di modulo JavaScript o importato nel browser facendo il *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  file.</span><span class="sxs-lookup"><span data-stu-id="50c3c-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="50c3c-127">In un browser il `msgpack5` libreria è necessario inoltre fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="50c3c-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="50c3c-128">Usare un `<script>` tag per creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="50c3c-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="50c3c-129">La libreria reperibili *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="50c3c-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="50c3c-130">Quando si usa il `<script>` elemento, l'ordine è importante.</span><span class="sxs-lookup"><span data-stu-id="50c3c-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="50c3c-131">Se *signalr-protocol-msgpack.js* fa riferimento prima *msgpack5.js*, si verifica un errore quando provano a connettersi con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="50c3c-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="50c3c-132">*SignalR.js* è inoltre necessaria prima *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="50c3c-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="50c3c-133">Aggiunta `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` per il `HubConnectionBuilder` verrà configurato il client per usare il protocollo MessagePack quando ci si connette a un server.</span><span class="sxs-lookup"><span data-stu-id="50c3c-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="50c3c-134">A questo punto, non esistono alcuna opzione di configurazione per il protocollo MessagePack nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="50c3c-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="50c3c-135">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="50c3c-135">Related resources</span></span>

* [<span data-ttu-id="50c3c-136">Introduzione</span><span class="sxs-lookup"><span data-stu-id="50c3c-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="50c3c-137">Client .NET</span><span class="sxs-lookup"><span data-stu-id="50c3c-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="50c3c-138">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="50c3c-138">JavaScript client</span></span>](xref:signalr/javascript-client)
