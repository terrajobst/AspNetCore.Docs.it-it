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
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="3439d-103">Utilizzare il protocollo di MessagePack Hub SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3439d-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="3439d-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3439d-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="3439d-105">Questo articolo si presuppone il lettore ha familiarità con gli argomenti inclusi in [iniziare a](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="3439d-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="3439d-106">Che cos'è MessagePack?</span><span class="sxs-lookup"><span data-stu-id="3439d-106">What is MessagePack?</span></span>

<span data-ttu-id="3439d-107">[MessagePack](https://msgpack.org/index.html) è un formato di serializzazione binaria è veloce e compact.</span><span class="sxs-lookup"><span data-stu-id="3439d-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="3439d-108">È utile per la larghezza di banda e le prestazioni rappresentano un problema perché crea messaggi più piccoli rispetto al [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="3439d-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="3439d-109">Poiché si tratta di un formato binario, i messaggi sono illeggibili quando si esaminano i log e le tracce di rete, a meno che i byte vengono passati attraverso un parser MessagePack.</span><span class="sxs-lookup"><span data-stu-id="3439d-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="3439d-110">SignalR include il supporto incorporato per il formato MessagePack e fornisce le API per il client e server da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="3439d-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="3439d-111">Configurare MessagePack nel server</span><span class="sxs-lookup"><span data-stu-id="3439d-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="3439d-112">Per abilitare il protocollo di Hub MessagePack nel server, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="3439d-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="3439d-113">Nel file Startup.cs aggiungere `AddMessagePackProtocol` per il `AddSignalR` chiamata per abilitare il supporto di MessagePack nel server.</span><span class="sxs-lookup"><span data-stu-id="3439d-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="3439d-114">JSON è abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3439d-114">JSON is enabled by default.</span></span> <span data-ttu-id="3439d-115">Aggiunta di MessagePack Abilita il supporto per JSON e MessagePack i client.</span><span class="sxs-lookup"><span data-stu-id="3439d-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="3439d-116">Per personalizzare il modo MessagePack deve formattare i dati, `AddMessagePackProtocol` accetta un delegato per la configurazione di opzioni.</span><span class="sxs-lookup"><span data-stu-id="3439d-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="3439d-117">Nel delegato, il `FormatterResolvers` proprietà può essere utilizzata per configurare le opzioni di serializzazione MessagePack.</span><span class="sxs-lookup"><span data-stu-id="3439d-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="3439d-118">Per ulteriori informazioni sul funzionamento delle funzioni i resolver, visitare la raccolta MessagePack al [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="3439d-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="3439d-119">Gli attributi possono essere utilizzati per gli oggetti che si desidera serializzare per definire la modalità deve essere gestite.</span><span class="sxs-lookup"><span data-stu-id="3439d-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="3439d-120">Configurare MessagePack nel client</span><span class="sxs-lookup"><span data-stu-id="3439d-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="3439d-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="3439d-121">.NET client</span></span>

<span data-ttu-id="3439d-122">Per abilitare MessagePack nel Client di .NET, installare il `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacchetto e chiamare `AddMessagePackProtocol` su `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3439d-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="3439d-123">Ciò `AddMessagePackProtocol` chiamare accetta un delegato per la configurazione di opzioni come il server.</span><span class="sxs-lookup"><span data-stu-id="3439d-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="3439d-124">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="3439d-124">JavaScript client</span></span>

<span data-ttu-id="3439d-125">Supporto MessagePack per il client Javascript viene fornito dal `@aspnet/signalr-protocol-msgpack` pacchetto NPM.</span><span class="sxs-lookup"><span data-stu-id="3439d-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="3439d-126">Dopo aver installato il pacchetto npm, il modulo può essere utilizzato direttamente tramite un caricatore del modulo di JavaScript o importato nel browser facendo riferimento il *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  file.</span><span class="sxs-lookup"><span data-stu-id="3439d-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="3439d-127">In un browser il `msgpack5` libreria è necessario inoltre fare riferimento.</span><span class="sxs-lookup"><span data-stu-id="3439d-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="3439d-128">Utilizzare un `<script>` tag per creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="3439d-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="3439d-129">La libreria è reperibile in *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="3439d-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="3439d-130">Quando si utilizza il `<script>` elemento, l'ordine è importante.</span><span class="sxs-lookup"><span data-stu-id="3439d-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="3439d-131">Se *signalr-protocollo-msgpack.js* viene fatto riferimento prima *msgpack5.js*, si verifica un errore durante il tentativo di connettersi con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="3439d-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="3439d-132">*SignalR.js* è inoltre necessaria prima *signalr-protocollo-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="3439d-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="3439d-133">Aggiunta `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` per il `HubConnectionBuilder` verrà configurato il client per usare il protocollo MessagePack quando ci si connette a un server.</span><span class="sxs-lookup"><span data-stu-id="3439d-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="3439d-134">In questo momento, non esistono alcuna opzione di configurazione per il protocollo MessagePack sul client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3439d-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3439d-135">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="3439d-135">Related resources</span></span>

* [<span data-ttu-id="3439d-136">Introduzione</span><span class="sxs-lookup"><span data-stu-id="3439d-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="3439d-137">Client .NET</span><span class="sxs-lookup"><span data-stu-id="3439d-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3439d-138">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="3439d-138">JavaScript client</span></span>](xref:signalr/javascript-client)
