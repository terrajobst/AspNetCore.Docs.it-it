---
title: Usare lo streaming in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come trasmettere dati tra il client e server.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: 8f39fdfa45766b5bbec572970f009abefefdc419
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897198"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="bff96-103">Usare lo streaming in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bff96-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="bff96-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="bff96-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bff96-105">ASP.NET Core SignalR supporta lo streaming dal client al server e dal server al client.</span><span class="sxs-lookup"><span data-stu-id="bff96-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="bff96-106">Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="bff96-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="bff96-107">Durante lo streaming, ogni frammento viene inviato al client o server, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="bff96-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bff96-108">ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server.</span><span class="sxs-lookup"><span data-stu-id="bff96-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="bff96-109">Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="bff96-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="bff96-110">Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="bff96-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="bff96-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bff96-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="bff96-112">Configurare un hub per lo streaming</span><span class="sxs-lookup"><span data-stu-id="bff96-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bff96-113">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="bff96-113">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bff96-114">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un <xref:System.Threading.Channels.ChannelReader%601> o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="bff96-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="bff96-115">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="bff96-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bff96-116">I metodi dell'hub di streaming può restituire `IAsyncEnumerable<T>` oltre a `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="bff96-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="bff96-117">Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere un metodo iteratore asincrono del metodo dell'hub come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="bff96-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="bff96-118">Metodi iteratori async hub possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="bff96-119">Metodi iteratori Async evitare problemi comuni con i canali, ad esempio per non restituire la `ChannelReader` iniziale in modo o metodo esistente senza completare la <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="bff96-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="bff96-120">L'esempio seguente illustra le nozioni di base dei dati al client utilizzando i canali di streaming.</span><span class="sxs-lookup"><span data-stu-id="bff96-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="bff96-121">Ogni volta che un oggetto viene scritto il <xref:System.Threading.Channels.ChannelWriter%601>, l'oggetto viene inviato immediatamente al client.</span><span class="sxs-lookup"><span data-stu-id="bff96-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="bff96-122">Al termine, il `ChannelWriter` sia completata per indicare al client il flusso è chiuso.</span><span class="sxs-lookup"><span data-stu-id="bff96-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="bff96-123">Scrivere il `ChannelWriter<T>` su un thread in background e restituire il `ChannelReader` appena possibile.</span><span class="sxs-lookup"><span data-stu-id="bff96-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="bff96-124">Altre chiamate dell'hub sono bloccate fino a quando un `ChannelReader` viene restituito.</span><span class="sxs-lookup"><span data-stu-id="bff96-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="bff96-125">Eseguire il wrapping per la logica in un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="bff96-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="bff96-126">Completare la `Channel` nella `catch` esterno il `catch` per assicurarsi che l'hub di chiamata al metodo viene completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="bff96-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="bff96-127">I metodi dell'hub di streaming server a client possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="bff96-128">Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="bff96-129">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="bff96-129">Client-to-server streaming</span></span>

<span data-ttu-id="bff96-130">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso client-server quando si accetta uno o più <xref:System.Threading.Channels.ChannelReader`1>s.</span><span class="sxs-lookup"><span data-stu-id="bff96-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more <xref:System.Threading.Channels.ChannelReader`1>s.</span></span> <span data-ttu-id="bff96-131">L'esempio seguente illustra le nozioni di base di leggere i dati di streaming inviati dal client.</span><span class="sxs-lookup"><span data-stu-id="bff96-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="bff96-132">Ogni volta che il client scrive per la <xref:System.Threading.Channels.ChannelWriter`1>, i dati vengono scritti nel `ChannelReader` nel server che sta leggendo il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff96-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter`1>, the data is written into the `ChannelReader` on the server that the hub method is reading from.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="bff96-133">Client .NET</span><span class="sxs-lookup"><span data-stu-id="bff96-133">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="bff96-134">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="bff96-134">Server-to-client streaming</span></span>

<span data-ttu-id="bff96-135">Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming server a client.</span><span class="sxs-lookup"><span data-stu-id="bff96-135">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="bff96-136">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="bff96-136">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="bff96-137">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="bff96-137">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="bff96-138">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="bff96-138">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
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

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="bff96-139">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="bff96-139">Client-to-server streaming</span></span>

<span data-ttu-id="bff96-140">Per richiamare un metodo dell'hub sul flusso client-server dal client .NET, creare un `Channel` e passare il `ChannelReader` come argomento al `SendAsync`, `InvokeAsync`, o `StreamAsChannelAsync`, a seconda del metodo dell'hub richiamato.</span><span class="sxs-lookup"><span data-stu-id="bff96-140">To invoke a client-to-server streaming hub method from the .NET client, create a `Channel` and pass the `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="bff96-141">Ogni volta che i dati vengono scritti i `ChannelWriter`, il metodo dell'hub sul server riceve un nuovo elemento con i dati dal client.</span><span class="sxs-lookup"><span data-stu-id="bff96-141">Whenever data is written to the `ChannelWriter`, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="bff96-142">Per terminare il flusso, completare il canale con `channel.Writer.Complete()`.</span><span class="sxs-lookup"><span data-stu-id="bff96-142">To end the stream, complete the channel with `channel.Writer.Complete()`.</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="bff96-143">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bff96-143">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="bff96-144">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="bff96-144">Server-to-client streaming</span></span>

<span data-ttu-id="bff96-145">I client JavaScript chiamano i metodi di streaming server a client sull'hub con `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="bff96-145">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="bff96-146">Il `stream` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="bff96-146">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="bff96-147">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff96-147">The name of the hub method.</span></span> <span data-ttu-id="bff96-148">Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.</span><span class="sxs-lookup"><span data-stu-id="bff96-148">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="bff96-149">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff96-149">Arguments defined in the hub method.</span></span> <span data-ttu-id="bff96-150">Nell'esempio seguente, gli argomenti sono un conteggio per il numero di elementi del flusso di ricezione e il ritardo tra gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-150">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="bff96-151">`connection.stream` Restituisce un `IStreamResult`, che contiene un `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bff96-151">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="bff96-152">Passare un `IStreamSubscriber` al `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere notifiche dal `stream` chiamata.</span><span class="sxs-lookup"><span data-stu-id="bff96-152">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="bff96-153">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bff96-153">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="bff96-154">Chiamando questo metodo, l'annullamento del `CancellationToken` parametro del metodo dell'Hub, se è stato specificato uno.</span><span class="sxs-lookup"><span data-stu-id="bff96-154">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="bff96-155">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bff96-155">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="bff96-156">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="bff96-156">Client-to-server streaming</span></span>

<span data-ttu-id="bff96-157">I client JavaScript chiamano metodi flusso client-server sugli hub passando un `Subject` come argomento al `send`, `invoke`, o `stream`, a seconda del metodo dell'hub richiamato.</span><span class="sxs-lookup"><span data-stu-id="bff96-157">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="bff96-158">Il `Subject` è una classe che è simile a un `Subject`.</span><span class="sxs-lookup"><span data-stu-id="bff96-158">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="bff96-159">Ad esempio in RxJS, è possibile usare la [soggetto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe da tale libreria.</span><span class="sxs-lookup"><span data-stu-id="bff96-159">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="bff96-160">La chiamata `subject.next(item)` con un elemento scrive l'elemento nel flusso e del metodo dell'hub riceve l'elemento nel server.</span><span class="sxs-lookup"><span data-stu-id="bff96-160">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="bff96-161">Per terminare il flusso, chiamare `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="bff96-161">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="bff96-162">Client Java</span><span class="sxs-lookup"><span data-stu-id="bff96-162">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="bff96-163">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="bff96-163">Server-to-client streaming</span></span>

<span data-ttu-id="bff96-164">Il client Java di SignalR utilizza i `stream` metodo per richiamare i metodi di streaming.</span><span class="sxs-lookup"><span data-stu-id="bff96-164">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="bff96-165">`stream` accetta tre o più argomenti:</span><span class="sxs-lookup"><span data-stu-id="bff96-165">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="bff96-166">Il tipo previsto degli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-166">The expected type of the stream items.</span></span>
* <span data-ttu-id="bff96-167">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff96-167">The name of the hub method.</span></span>
* <span data-ttu-id="bff96-168">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="bff96-168">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="bff96-169">Il `stream` metodo su `HubConnection` restituisce un oggetto osservabile del tipo di elemento flusso.</span><span class="sxs-lookup"><span data-stu-id="bff96-169">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="bff96-170">Il tipo Observable `subscribe` metodo è dove `onNext`, `onError` e `onCompleted` sono stati definiti gestori.</span><span class="sxs-lookup"><span data-stu-id="bff96-170">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="bff96-171">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bff96-171">Additional resources</span></span>

* [<span data-ttu-id="bff96-172">Hub</span><span class="sxs-lookup"><span data-stu-id="bff96-172">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="bff96-173">Client .NET</span><span class="sxs-lookup"><span data-stu-id="bff96-173">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="bff96-174">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bff96-174">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="bff96-175">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="bff96-175">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
