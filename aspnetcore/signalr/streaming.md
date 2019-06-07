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
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usare lo streaming in ASP.NET Core SignalR

Da [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR supporta lo streaming dal client al server e dal server al client. Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo. Durante lo streaming, ogni frammento viene inviato al client o server, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server. Ciò è utile per scenari in cui frammenti di dati arrivano nel corso del tempo. Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

::: moniker-end

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Configurare un hub per lo streaming

::: moniker range=">= aspnetcore-3.0"

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, o `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un <xref:System.Threading.Channels.ChannelReader%601> o un `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Streaming server a client

::: moniker range=">= aspnetcore-3.0"

I metodi dell'hub di streaming può restituire `IAsyncEnumerable<T>` oltre a `ChannelReader<T>`. Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere un metodo iteratore asincrono del metodo dell'hub come illustrato nell'esempio seguente. Metodi iteratori async hub possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso. Metodi iteratori Async evitare problemi comuni con i canali, ad esempio per non restituire la `ChannelReader` iniziale in modo o metodo esistente senza completare la <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

L'esempio seguente illustra le nozioni di base dei dati al client utilizzando i canali di streaming. Ogni volta che un oggetto viene scritto il <xref:System.Threading.Channels.ChannelWriter%601>, l'oggetto viene inviato immediatamente al client. Al termine, il `ChannelWriter` sia completata per indicare al client il flusso è chiuso.

> [!NOTE]
> Scrivere il `ChannelWriter<T>` su un thread in background e restituire il `ChannelReader` appena possibile. Altre chiamate dell'hub sono bloccate fino a quando un `ChannelReader` viene restituito.
>
> Eseguire il wrapping per la logica in un `try ... catch`. Completare la `Channel` nella `catch` esterno il `catch` per assicurarsi che l'hub di chiamata al metodo viene completata correttamente.

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

I metodi dell'hub di streaming server a client possono accettare un `CancellationToken` parametro che viene attivato quando il client annulla la sottoscrizione dal flusso. Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Client-server di streaming

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso client-server quando si accetta uno o più oggetti di tipo <xref:System.Threading.Channels.ChannelReader%601> o <xref:System.Collections.Generic.IAsyncEnumerable%601>. L'esempio seguente illustra le nozioni di base di leggere i dati di streaming inviati dal client. Ogni volta che il client scrive per la <xref:System.Threading.Channels.ChannelWriter%601>, i dati vengono scritti nel `ChannelReader` sul server da cui sta leggendo il metodo dell'hub.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Un <xref:System.Collections.Generic.IAsyncEnumerable%601> segue versione del metodo.

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

## <a name="net-client"></a>Client .NET

### <a name="server-to-client-streaming"></a>Streaming server a client


::: moniker range=">= aspnetcore-3.0"

Il `StreamAsync` e `StreamAsChannelAsync` metodi su `HubConnection` vengono utilizzati per richiamare i metodi di streaming server a client. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub `StreamAsync` o `StreamAsChannelAsync`. Il parametro generico sul `StreamAsync<T>` e `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Un oggetto di tipo `IAsyncEnumerable<T>` o `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.

Oggetto `StreamAsync` esempio che restituisce `IAsyncEnumerable<int>`:

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

Un oggetto corrispondente `StreamAsChannelAsync` esempio che restituisce `ChannelReader<int>`:

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

Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming server a client. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.

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

Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming server a client. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client.

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

### <a name="client-to-server-streaming"></a>Client-server di streaming

Esistono due modi per richiamare un metodo dell'hub sul flusso client-server dal client .NET. È possibile passare un `IAsyncEnumerable<T>` o un `ChannelReader` come argomento al `SendAsync`, `InvokeAsync`, o `StreamAsChannelAsync`, a seconda del metodo dell'hub richiamato.

Ogni volta che i dati vengono scritti i `IAsyncEnumerable` o `ChannelWriter` dell'oggetto, il metodo dell'hub sul server riceve un nuovo elemento con i dati dal client.

Se si usa un `IAsyncEnumerable` dell'oggetto, il flusso termina dopo il metodo che restituisce elementi di flusso viene chiuso.

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

O se si usa un' `ChannelWriter`, completare il canale con `channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Client JavaScript

### <a name="server-to-client-streaming"></a>Streaming server a client

I client JavaScript chiamano i metodi di streaming server a client sull'hub con `connection.stream`. Il `stream` metodo accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.
* Argomenti definiti nel metodo dell'hub. Nell'esempio seguente, gli argomenti sono un conteggio per il numero di elementi del flusso di ricezione e il ritardo tra gli elementi di flusso.

`connection.stream` Restituisce un `IStreamResult`, che contiene un `subscribe` (metodo). Passare un `IStreamSubscriber` al `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere notifiche dal `stream` chiamata.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo). Chiamando questo metodo, l'annullamento del `CancellationToken` parametro del metodo dell'Hub, se è stato specificato uno.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Client-server di streaming

I client JavaScript chiamano metodi flusso client-server sugli hub passando un `Subject` come argomento al `send`, `invoke`, o `stream`, a seconda del metodo dell'hub richiamato. Il `Subject` è una classe che è simile a un `Subject`. Ad esempio in RxJS, è possibile usare la [soggetto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe da tale libreria.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

La chiamata `subject.next(item)` con un elemento scrive l'elemento nel flusso e del metodo dell'hub riceve l'elemento nel server.

Per terminare il flusso, chiamare `subject.complete()`.

## <a name="java-client"></a>Client Java

### <a name="server-to-client-streaming"></a>Streaming server a client

Il client Java di SignalR utilizza i `stream` metodo per richiamare i metodi di streaming. `stream` accetta tre o più argomenti:

* Il tipo previsto degli elementi di flusso.
* Il nome del metodo dell'hub.
* Argomenti definiti nel metodo dell'hub.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

Il `stream` metodo su `HubConnection` restituisce un oggetto osservabile del tipo di elemento flusso. Il tipo Observable `subscribe` metodo è dove `onNext`, `onError` e `onCompleted` sono stati definiti gestori.

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
