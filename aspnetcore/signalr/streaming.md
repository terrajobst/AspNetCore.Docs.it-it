---
title: Usare lo streaming in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come restituire flussi di valori da metodi dell'hub sul server e utilizzare i flussi con i client .NET e JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 7c176e3f21ffca7b97d9d3c2e8861032f22587b8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264299"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="ad8d5-103">Usare lo streaming in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ad8d5-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ad8d5-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ad8d5-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="ad8d5-105">ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="ad8d5-106">Ciò è utile per scenari in cui risulterà frammenti di dati nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="ad8d5-107">Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="ad8d5-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ad8d5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="ad8d5-109">Configurare l'hub</span><span class="sxs-lookup"><span data-stu-id="ad8d5-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ad8d5-110">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ad8d5-111">Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ad8d5-112">In ASP.NET Core 3.0 o versione successiva, i metodi dell'hub di streaming può restituire `IAsyncEnumerable<T>` oltre a `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="ad8d5-113">Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere un metodo iteratore asincrono del metodo dell'hub come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="ad8d5-114">Metodi iteratori async hub possono accettare un `CancellationToken` parametro che verrà attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ad8d5-115">I metodi iterator Async evitare facilmente i problemi comuni con i canali, ad esempio non restituisce il `ChannelReader` iniziale in modo o metodo esistente senza completare la `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="ad8d5-116">L'esempio seguente illustra le nozioni di base dei dati al client utilizzando i canali di streaming.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="ad8d5-117">Ogni volta che un oggetto viene scritto il `ChannelWriter` quell'oggetto viene inviato immediatamente al client.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="ad8d5-118">Al termine, il `ChannelWriter` sia completata per indicare al client il flusso è chiuso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="ad8d5-119">Scrivere il `ChannelWriter` su un thread in background e restituire il `ChannelReader` appena possibile.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="ad8d5-120">Altre chiamate dell'hub verranno bloccate fino a un `ChannelReader` viene restituito.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="ad8d5-121">Eseguire il wrapping la logica in una `try ... catch` e completare il `Channel` nel catch e all'esterno di catch in modo da assicurarsi che l'hub di chiamata al metodo viene completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="ad8d5-122">In ASP.NET Core 2.2 o versioni successive, i metodi dell'hub di streaming può accettare un `CancellationToken` parametro che verrà attivato quando il client annulla la sottoscrizione dal flusso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ad8d5-123">Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="ad8d5-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ad8d5-124">.NET client</span></span>

<span data-ttu-id="ad8d5-125">Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="ad8d5-126">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ad8d5-127">Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ad8d5-128">Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="ad8d5-129">Per leggere i dati, un modello comune consiste nell'eseguire un ciclo nei `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="ad8d5-130">Il ciclo termina quando il server ha chiuso il flusso o il token di annullamento passato a `StreamAsChannelAsync` viene annullata.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="ad8d5-131">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ad8d5-131">JavaScript client</span></span>

<span data-ttu-id="ad8d5-132">I client JavaScript chiamano metodi streaming in hub usando `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="ad8d5-133">Il `stream` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="ad8d5-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="ad8d5-134">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-134">The name of the hub method.</span></span> <span data-ttu-id="ad8d5-135">Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="ad8d5-136">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="ad8d5-137">Nell'esempio seguente, gli argomenti sono: un conteggio per il numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="ad8d5-138">`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ad8d5-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="ad8d5-139">Passare un `IStreamSubscriber` a `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere le notifiche di `stream` chiamata.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ad8d5-140">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ad8d5-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ad8d5-141">Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ad8d5-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="ad8d5-142">Chiamare questo metodo genererà il `CancellationToken` parametro del metodo dell'Hub (se è stata specificata una) deve essere annullata.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="java-client"></a><span data-ttu-id="ad8d5-143">Client Java</span><span class="sxs-lookup"><span data-stu-id="ad8d5-143">Java client</span></span>

<span data-ttu-id="ad8d5-144">Il client Java di SignalR utilizza i `stream` metodo per richiamare i metodi di streaming.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="ad8d5-145">Accetta tre o più argomenti:</span><span class="sxs-lookup"><span data-stu-id="ad8d5-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="ad8d5-146">Il tipo previsto degli elementi di flusso</span><span class="sxs-lookup"><span data-stu-id="ad8d5-146">The expected type of the stream items</span></span>
* <span data-ttu-id="ad8d5-147">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-147">The name of the hub method.</span></span>
* <span data-ttu-id="ad8d5-148">Argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-148">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="ad8d5-149">Il `stream` metodo su `HubConnection` restituisce un oggetto osservabile del tipo di elemento flusso.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="ad8d5-150">Il tipo Observable `subscribe` metodo viene usata per definire le `onNext`, `onError` e `onCompleted` gestori.</span><span class="sxs-lookup"><span data-stu-id="ad8d5-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="ad8d5-151">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="ad8d5-151">Related resources</span></span>

* [<span data-ttu-id="ad8d5-152">Hub</span><span class="sxs-lookup"><span data-stu-id="ad8d5-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ad8d5-153">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ad8d5-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ad8d5-154">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ad8d5-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ad8d5-155">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="ad8d5-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
