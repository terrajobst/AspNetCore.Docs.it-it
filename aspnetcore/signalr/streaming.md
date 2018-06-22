---
title: Utilizzare il flusso in ASP.NET SignalR Core
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275850"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="a7d21-102">Utilizzare il flusso in ASP.NET SignalR Core</span><span class="sxs-lookup"><span data-stu-id="a7d21-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="a7d21-103">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="a7d21-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="a7d21-104">SignalR ASP.NET Core supporta streaming valori restituiti dei metodi di server.</span><span class="sxs-lookup"><span data-stu-id="a7d21-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="a7d21-105">Ciò è utile per scenari in cui frammenti di dati saranno quello nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="a7d21-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="a7d21-106">Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, piuttosto che in attesa di tutti i dati diventano disponibili.</span><span class="sxs-lookup"><span data-stu-id="a7d21-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="a7d21-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7d21-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="a7d21-108">Impostare l'hub</span><span class="sxs-lookup"><span data-stu-id="a7d21-108">Set up the hub</span></span>

<span data-ttu-id="a7d21-109">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="a7d21-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="a7d21-110">Di seguito è riportato un esempio che illustra le nozioni di base del flusso di dati al client.</span><span class="sxs-lookup"><span data-stu-id="a7d21-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="a7d21-111">Ogni volta che un oggetto viene scritto il `ChannelReader` quell'oggetto viene inviato immediatamente al client.</span><span class="sxs-lookup"><span data-stu-id="a7d21-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="a7d21-112">Al termine, il `ChannelReader` sia completata per indicare al client il flusso è chiuso.</span><span class="sxs-lookup"><span data-stu-id="a7d21-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="a7d21-113">Scrivere il `ChannelReader` su un thread in background e restituire il `ChannelReader` appena possibile.</span><span class="sxs-lookup"><span data-stu-id="a7d21-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="a7d21-114">Altre chiamate dell'hub verranno bloccate finché un `ChannelReader` viene restituito.</span><span class="sxs-lookup"><span data-stu-id="a7d21-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="a7d21-115">Client .NET</span><span class="sxs-lookup"><span data-stu-id="a7d21-115">.NET client</span></span>

<span data-ttu-id="a7d21-116">Il `StreamAsChannelAsync` metodo su `HubConnection` viene utilizzato per richiamare un metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="a7d21-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="a7d21-117">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7d21-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="a7d21-118">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="a7d21-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="a7d21-119">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="a7d21-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="a7d21-120">Per leggere i dati, un modello comune consiste nel ciclo sugli `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati.</span><span class="sxs-lookup"><span data-stu-id="a7d21-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="a7d21-121">Il ciclo terminerà quando il flusso è stato chiuso dal server oppure il token di annullamento passato a `StreamAsChannelAsync` viene annullata.</span><span class="sxs-lookup"><span data-stu-id="a7d21-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

## <a name="javascript-client"></a><span data-ttu-id="a7d21-122">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7d21-122">JavaScript client</span></span>

<span data-ttu-id="a7d21-123">I client JavaScript chiamare metodi streaming su hub utilizzando `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="a7d21-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="a7d21-124">Il `stream` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="a7d21-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="a7d21-125">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a7d21-125">The name of the hub method.</span></span> <span data-ttu-id="a7d21-126">Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.</span><span class="sxs-lookup"><span data-stu-id="a7d21-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="a7d21-127">Argomenti definiti del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="a7d21-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="a7d21-128">Nell'esempio seguente, gli argomenti sono: un conteggio del numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="a7d21-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="a7d21-129">`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` metodo.</span><span class="sxs-lookup"><span data-stu-id="a7d21-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="a7d21-130">Passare un `IStreamSubscriber` al `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere notifiche dal `stream` chiamata.</span><span class="sxs-lookup"><span data-stu-id="a7d21-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="a7d21-131">Per terminare il flusso dalla chiamata al client il `dispose` metodo sul `ISubscription` restituito dal `subscribe` metodo.</span><span class="sxs-lookup"><span data-stu-id="a7d21-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="a7d21-132">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="a7d21-132">Related resources</span></span>

* [<span data-ttu-id="a7d21-133">Hub</span><span class="sxs-lookup"><span data-stu-id="a7d21-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a7d21-134">Client .NET</span><span class="sxs-lookup"><span data-stu-id="a7d21-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a7d21-135">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7d21-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a7d21-136">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="a7d21-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)