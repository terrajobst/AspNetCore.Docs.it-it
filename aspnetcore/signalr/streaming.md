---
title: Usare lo streaming in ASP.NET Core SignalR
author: bradygaster
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: ade2d6fb6e799d53ff3aaa69c641d0088acdee95
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837403"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="ff3de-102">Usare lo streaming in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ff3de-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ff3de-103">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ff3de-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="ff3de-104">ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server.</span><span class="sxs-lookup"><span data-stu-id="ff3de-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="ff3de-105">Ciò è utile per scenari in cui risulterà frammenti di dati nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="ff3de-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="ff3de-106">Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="ff3de-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="ff3de-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ff3de-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="ff3de-108">Configurare l'hub</span><span class="sxs-lookup"><span data-stu-id="ff3de-108">Set up the hub</span></span>

<span data-ttu-id="ff3de-109">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="ff3de-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="ff3de-110">Di seguito è un esempio che illustra le nozioni di base del flusso di dati al client.</span><span class="sxs-lookup"><span data-stu-id="ff3de-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="ff3de-111">Ogni volta che un oggetto viene scritto il `ChannelReader` quell'oggetto viene inviato immediatamente al client.</span><span class="sxs-lookup"><span data-stu-id="ff3de-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="ff3de-112">Al termine, il `ChannelReader` sia completata per indicare al client il flusso è chiuso.</span><span class="sxs-lookup"><span data-stu-id="ff3de-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="ff3de-113">Scrivere il `ChannelReader` su un thread in background e restituire il `ChannelReader` appena possibile.</span><span class="sxs-lookup"><span data-stu-id="ff3de-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="ff3de-114">Altre chiamate dell'hub verranno bloccate fino a un `ChannelReader` viene restituito.</span><span class="sxs-lookup"><span data-stu-id="ff3de-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="ff3de-115">Eseguire il wrapping la logica in una `try ... catch` e completare il `Channel` nel catch e all'esterno di catch in modo da assicurarsi che l'hub di chiamata al metodo viene completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="ff3de-115">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="ff3de-116">In ASP.NET Core 2.2 o versioni successive, i metodi dell'Hub di streaming può accettare un `CancellationToken` parametro che verrà attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="ff3de-116">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ff3de-117">Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.</span><span class="sxs-lookup"><span data-stu-id="ff3de-117">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="ff3de-118">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ff3de-118">.NET client</span></span>

<span data-ttu-id="ff3de-119">Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="ff3de-119">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="ff3de-120">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="ff3de-120">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ff3de-121">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="ff3de-121">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ff3de-122">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="ff3de-122">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="ff3de-123">Per leggere i dati, un modello comune consiste nell'eseguire un ciclo nei `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati.</span><span class="sxs-lookup"><span data-stu-id="ff3de-123">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="ff3de-124">Il ciclo termina quando il server ha chiuso il flusso o il token di annullamento passato a `StreamAsChannelAsync` viene annullata.</span><span class="sxs-lookup"><span data-stu-id="ff3de-124">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

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

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="ff3de-125">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ff3de-125">JavaScript client</span></span>

<span data-ttu-id="ff3de-126">I client JavaScript chiamano metodi streaming in hub usando `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="ff3de-126">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="ff3de-127">Il `stream` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="ff3de-127">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="ff3de-128">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ff3de-128">The name of the hub method.</span></span> <span data-ttu-id="ff3de-129">Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.</span><span class="sxs-lookup"><span data-stu-id="ff3de-129">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="ff3de-130">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ff3de-130">Arguments defined in the hub method.</span></span> <span data-ttu-id="ff3de-131">Nell'esempio seguente, gli argomenti sono: un conteggio per il numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="ff3de-131">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="ff3de-132">`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ff3de-132">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="ff3de-133">Passare un `IStreamSubscriber` a `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere le notifiche di `stream` chiamata.</span><span class="sxs-lookup"><span data-stu-id="ff3de-133">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ff3de-134">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ff3de-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ff3de-135">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ff3de-135">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="ff3de-136">Chiamare questo metodo genererà il `CancellationToken` parametro del metodo dell'Hub (se è stata specificata una) deve essere annullata.</span><span class="sxs-lookup"><span data-stu-id="ff3de-136">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="ff3de-137">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="ff3de-137">Related resources</span></span>

* [<span data-ttu-id="ff3de-138">Hub</span><span class="sxs-lookup"><span data-stu-id="ff3de-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ff3de-139">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ff3de-139">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ff3de-140">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ff3de-140">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ff3de-141">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="ff3de-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
