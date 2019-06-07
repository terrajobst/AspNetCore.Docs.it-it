---
title: Usare lo streaming in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come trasmettere dati tra il client e server.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: a75156f398e113393ddb891d16eec3f09de80c09
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750190"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="33d38-103">Usare lo streaming in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="33d38-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="33d38-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="33d38-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="33d38-105">ASP.NET Core SignalR supporta lo streaming dal client al server e dal server al client.</span><span class="sxs-lookup"><span data-stu-id="33d38-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="33d38-106">Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="33d38-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="33d38-107">Durante lo streaming, ogni frammento viene inviato al client o server, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="33d38-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="33d38-108">ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server.</span><span class="sxs-lookup"><span data-stu-id="33d38-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="33d38-109">Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="33d38-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="33d38-110">Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="33d38-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="33d38-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33d38-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="33d38-112">Configurare un hub per lo streaming</span><span class="sxs-lookup"><span data-stu-id="33d38-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="33d38-113">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="33d38-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="33d38-114">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un <xref:System.Threading.Channels.ChannelReader%601> o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="33d38-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="33d38-115">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="33d38-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="33d38-116">I metodi dell'hub di streaming può restituire `IAsyncEnumerable<T>` oltre a `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="33d38-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="33d38-117">Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere un metodo iteratore asincrono del metodo dell'hub come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="33d38-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="33d38-118">Metodi iteratori async hub possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="33d38-119">Metodi iteratori Async evitare problemi comuni con i canali, ad esempio per non restituire la `ChannelReader` iniziale in modo o metodo esistente senza completare la <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="33d38-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="33d38-120">L'esempio seguente illustra le nozioni di base dei dati al client utilizzando i canali di streaming.</span><span class="sxs-lookup"><span data-stu-id="33d38-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="33d38-121">Ogni volta che un oggetto viene scritto il <xref:System.Threading.Channels.ChannelWriter%601>, l'oggetto viene inviato immediatamente al client.</span><span class="sxs-lookup"><span data-stu-id="33d38-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="33d38-122">Al termine, il `ChannelWriter` sia completata per indicare al client il flusso è chiuso.</span><span class="sxs-lookup"><span data-stu-id="33d38-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="33d38-123">Scrivere il `ChannelWriter<T>` su un thread in background e restituire il `ChannelReader` appena possibile.</span><span class="sxs-lookup"><span data-stu-id="33d38-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="33d38-124">Altre chiamate dell'hub sono bloccate fino a quando un `ChannelReader` viene restituito.</span><span class="sxs-lookup"><span data-stu-id="33d38-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="33d38-125">Eseguire il wrapping per la logica in un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="33d38-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="33d38-126">Completare la `Channel` nella `catch` esterno il `catch` per assicurarsi che l'hub di chiamata al metodo viene completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="33d38-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="33d38-127">I metodi dell'hub di streaming server a client possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="33d38-128">Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="33d38-129">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="33d38-129">Client-to-server streaming</span></span>

<span data-ttu-id="33d38-130">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso client-server quando si accetta uno o più oggetti di tipo <xref:System.Threading.Channels.ChannelReader%601> o <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="33d38-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="33d38-131">L'esempio seguente illustra le nozioni di base di leggere i dati di streaming inviati dal client.</span><span class="sxs-lookup"><span data-stu-id="33d38-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="33d38-132">Ogni volta che il client scrive per la <xref:System.Threading.Channels.ChannelWriter%601>, i dati vengono scritti nel `ChannelReader` sul server da cui sta leggendo il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="33d38-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="33d38-133">Un <xref:System.Collections.Generic.IAsyncEnumerable%601> segue versione del metodo.</span><span class="sxs-lookup"><span data-stu-id="33d38-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<Stream> stream) 
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="33d38-134">Client .NET</span><span class="sxs-lookup"><span data-stu-id="33d38-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="33d38-135">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="33d38-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="33d38-136">Il `StreamAsync` e `StreamAsChannelAsync` metodi su `HubConnection` vengono utilizzati per richiamare i metodi di streaming server a client.</span><span class="sxs-lookup"><span data-stu-id="33d38-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="33d38-137">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub `StreamAsync` o `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="33d38-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="33d38-138">Il parametro generico sul `StreamAsync<T>` e `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="33d38-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="33d38-139">Un oggetto di tipo `IAsyncEnumerable<T>` o `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="33d38-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="33d38-140">Oggetto `StreamAsync` esempio che restituisce `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="33d38-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

<span data-ttu-id="33d38-141">Un oggetto corrispondente `StreamAsChannelAsync` esempio che restituisce `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="33d38-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="33d38-142">Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming server a client.</span><span class="sxs-lookup"><span data-stu-id="33d38-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="33d38-143">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="33d38-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="33d38-144">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="33d38-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="33d38-145">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="33d38-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="33d38-146">Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming server a client.</span><span class="sxs-lookup"><span data-stu-id="33d38-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="33d38-147">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="33d38-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="33d38-148">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="33d38-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="33d38-149">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="33d38-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="33d38-150">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="33d38-150">Client-to-server streaming</span></span>

<span data-ttu-id="33d38-151">Esistono due modi per richiamare un metodo dell'hub sul flusso client-server dal client .NET.</span><span class="sxs-lookup"><span data-stu-id="33d38-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="33d38-152">È possibile passare un `IAsyncEnumerable<T>` o un `ChannelReader` come argomento al `SendAsync`, `InvokeAsync`, o `StreamAsChannelAsync`, a seconda del metodo dell'hub richiamato.</span><span class="sxs-lookup"><span data-stu-id="33d38-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="33d38-153">Ogni volta che i dati vengono scritti i `IAsyncEnumerable` o `ChannelWriter` dell'oggetto, il metodo dell'hub sul server riceve un nuovo elemento con i dati dal client.</span><span class="sxs-lookup"><span data-stu-id="33d38-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="33d38-154">Se si usa un `IAsyncEnumerable` dell'oggetto, il flusso termina dopo il metodo che restituisce elementi di flusso viene chiuso.</span><span class="sxs-lookup"><span data-stu-id="33d38-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="33d38-155">O se si usa un' `ChannelWriter`, completare il canale con `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="33d38-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="33d38-156">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="33d38-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="33d38-157">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="33d38-157">Server-to-client streaming</span></span>

<span data-ttu-id="33d38-158">I client JavaScript chiamano i metodi di streaming server a client sull'hub con `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="33d38-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="33d38-159">Il `stream` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="33d38-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="33d38-160">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="33d38-160">The name of the hub method.</span></span> <span data-ttu-id="33d38-161">Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.</span><span class="sxs-lookup"><span data-stu-id="33d38-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="33d38-162">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="33d38-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="33d38-163">Nell'esempio seguente, gli argomenti sono un conteggio per il numero di elementi del flusso di ricezione e il ritardo tra gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="33d38-164">`connection.stream` Restituisce un `IStreamResult`, che contiene un `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="33d38-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="33d38-165">Passare un `IStreamSubscriber` al `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere notifiche dal `stream` chiamata.</span><span class="sxs-lookup"><span data-stu-id="33d38-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="33d38-166">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="33d38-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="33d38-167">Chiamando questo metodo, l'annullamento del `CancellationToken` parametro del metodo dell'Hub, se è stato specificato uno.</span><span class="sxs-lookup"><span data-stu-id="33d38-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="33d38-168">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="33d38-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="33d38-169">Client-server di streaming</span><span class="sxs-lookup"><span data-stu-id="33d38-169">Client-to-server streaming</span></span>

<span data-ttu-id="33d38-170">I client JavaScript chiamano metodi flusso client-server sugli hub passando un `Subject` come argomento al `send`, `invoke`, o `stream`, a seconda del metodo dell'hub richiamato.</span><span class="sxs-lookup"><span data-stu-id="33d38-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="33d38-171">Il `Subject` è una classe che è simile a un `Subject`.</span><span class="sxs-lookup"><span data-stu-id="33d38-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="33d38-172">Ad esempio in RxJS, è possibile usare la [soggetto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe da tale libreria.</span><span class="sxs-lookup"><span data-stu-id="33d38-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="33d38-173">La chiamata `subject.next(item)` con un elemento scrive l'elemento nel flusso e del metodo dell'hub riceve l'elemento nel server.</span><span class="sxs-lookup"><span data-stu-id="33d38-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="33d38-174">Per terminare il flusso, chiamare `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="33d38-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="33d38-175">Client Java</span><span class="sxs-lookup"><span data-stu-id="33d38-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="33d38-176">Streaming server a client</span><span class="sxs-lookup"><span data-stu-id="33d38-176">Server-to-client streaming</span></span>

<span data-ttu-id="33d38-177">Il client Java di SignalR utilizza i `stream` metodo per richiamare i metodi di streaming.</span><span class="sxs-lookup"><span data-stu-id="33d38-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="33d38-178">`stream` accetta tre o più argomenti:</span><span class="sxs-lookup"><span data-stu-id="33d38-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="33d38-179">Il tipo previsto degli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="33d38-180">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="33d38-180">The name of the hub method.</span></span>
* <span data-ttu-id="33d38-181">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="33d38-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="33d38-182">Il `stream` metodo su `HubConnection` restituisce un oggetto osservabile del tipo di elemento flusso.</span><span class="sxs-lookup"><span data-stu-id="33d38-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="33d38-183">Il tipo Observable `subscribe` metodo è dove `onNext`, `onError` e `onCompleted` sono stati definiti gestori.</span><span class="sxs-lookup"><span data-stu-id="33d38-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="33d38-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="33d38-184">Additional resources</span></span>

* [<span data-ttu-id="33d38-185">Hub</span><span class="sxs-lookup"><span data-stu-id="33d38-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="33d38-186">Client .NET</span><span class="sxs-lookup"><span data-stu-id="33d38-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="33d38-187">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="33d38-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="33d38-188">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="33d38-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
