---
title: Usare il flusso in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come trasmettere i dati tra il client e il server.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 21dd8180fe168f81ed68b01f02b81a6264d6e5a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667727"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>Usare il flusso in ASP.NET Core SignalR

Di [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR supporta lo streaming da client a server e da server a client. Questa operazione è utile per gli scenari in cui i frammenti di dati arrivano nel tempo. Quando si esegue il flusso, ogni frammento viene inviato al client o al server non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR supporta il flusso dei valori restituiti dei metodi del server. Questa operazione è utile per gli scenari in cui i frammenti di dati arrivano nel tempo. Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

::: moniker-end

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Configurare un hub per lo streaming

::: moniker range=">= aspnetcore-3.0"

Un metodo Hub diventa automaticamente un metodo dell'hub di streaming quando restituisce <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`o `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un metodo Hub diventa automaticamente un metodo dell'hub di streaming quando restituisce un <xref:System.Threading.Channels.ChannelReader%601> o una `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Streaming da server a client

::: moniker range=">= aspnetcore-3.0"

I metodi dell'hub di streaming possono restituire `IAsyncEnumerable<T>` oltre al `ChannelReader<T>`. Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere il metodo dell'hub un metodo iteratore asincrono come illustrato nell'esempio seguente. I metodi iteratori asincroni dell'hub possono accettare un `CancellationToken` parametro che viene attivato quando il client Annulla la sottoscrizione dal flusso. I metodi iterator asincroni evitano problemi comuni con i canali, ad esempio la mancata restituzione del `ChannelReader` abbastanza presto o l'uscita dal metodo senza completare l'<xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Nell'esempio seguente vengono illustrate le nozioni di base per il flusso di dati al client tramite canali. Ogni volta che un oggetto viene scritto nel <xref:System.Threading.Channels.ChannelWriter%601>, l'oggetto viene immediatamente inviato al client. Alla fine, il `ChannelWriter` viene completato per indicare al client che il flusso è chiuso.

> [!NOTE]
> Scrivere sul `ChannelWriter<T>` in un thread in background e restituire il `ChannelReader` il prima possibile. Altre chiamate all'hub vengono bloccate fino a quando non viene restituito un `ChannelReader`.
>
> Eseguire il wrapping della logica in un `try ... catch`. Completare il `Channel` nel `catch` e all'esterno del `catch` per assicurarsi che la chiamata al metodo Hub venga completata correttamente.

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

I metodi dell'hub di streaming da server a client possono accettare un `CancellationToken` parametro che viene attivato quando il client Annulla la sottoscrizione dal flusso. Usare questo token per arrestare l'operazione server e rilasciare tutte le risorse se il client si disconnette prima della fine del flusso.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Streaming da client a server

Un metodo Hub diventa automaticamente un metodo dell'hub di streaming da client a server quando accetta uno o più oggetti di tipo <xref:System.Threading.Channels.ChannelReader%601> o <xref:System.Collections.Generic.IAsyncEnumerable%601>. Nell'esempio seguente vengono illustrate le nozioni di base per la lettura dei dati di streaming inviati dal client. Ogni volta che il client scrive nel <xref:System.Threading.Channels.ChannelWriter%601>, i dati vengono scritti nel `ChannelReader` sul server da cui viene letto il metodo dell'hub.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Segue una versione <xref:System.Collections.Generic.IAsyncEnumerable%601> del metodo.

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a>Client .NET

### <a name="server-to-client-streaming"></a>Streaming da server a client


::: moniker range=">= aspnetcore-3.0"

I metodi `StreamAsync` e `StreamAsChannelAsync` su `HubConnection` vengono usati per richiamare i metodi di streaming da server a client. Passare il nome e gli argomenti del metodo Hub definiti nel metodo hub per `StreamAsync` o `StreamAsChannelAsync`. Il parametro generico in `StreamAsync<T>` e `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Viene restituito un oggetto di tipo `IAsyncEnumerable<T>` o `ChannelReader<T>` dalla chiamata del flusso e rappresenta il flusso nel client.

Un `StreamAsync` esempio che restituisce `IAsyncEnumerable<int>`:

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

Esempio di `StreamAsChannelAsync` corrispondente che restituisce `ChannelReader<int>`:

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

Il metodo `StreamAsChannelAsync` su `HubConnection` viene usato per richiamare un metodo di streaming da server a client. Passare il nome e gli argomenti del metodo Hub definiti nel metodo hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Una `ChannelReader<T>` viene restituita dalla chiamata del flusso e rappresenta il flusso nel client.

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

Il metodo `StreamAsChannelAsync` su `HubConnection` viene usato per richiamare un metodo di streaming da server a client. Passare il nome e gli argomenti del metodo Hub definiti nel metodo hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Una `ChannelReader<T>` viene restituita dalla chiamata del flusso e rappresenta il flusso nel client.

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

### <a name="client-to-server-streaming"></a>Streaming da client a server

Esistono due modi per richiamare un metodo dell'hub di streaming da client a server dal client .NET. È possibile passare un `IAsyncEnumerable<T>` o un `ChannelReader` come argomento per `SendAsync`, `InvokeAsync`o `StreamAsChannelAsync`, a seconda del metodo dell'hub richiamato.

Ogni volta che i dati vengono scritti nell'oggetto `IAsyncEnumerable` o `ChannelWriter`, il metodo dell'hub sul server riceve un nuovo elemento con i dati del client.

Se si usa un oggetto `IAsyncEnumerable`, il flusso termina dopo che il metodo che restituisce gli elementi del flusso viene chiuso.

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

In alternativa, se si usa una `ChannelWriter`, è necessario completare il canale con `channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Client JavaScript

### <a name="server-to-client-streaming"></a>Streaming da server a client

I client JavaScript chiamano i metodi di streaming da server a client negli hub con `connection.stream`. Il metodo `stream` accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente il nome del metodo dell'hub è `Counter`.
* Argomenti definiti nel metodo dell'hub. Nell'esempio seguente gli argomenti sono un conteggio per il numero di elementi del flusso da ricevere e il ritardo tra gli elementi del flusso.

`connection.stream` restituisce un `IStreamResult`, che contiene un metodo di `subscribe`. Passare un `IStreamSubscriber` `subscribe` e impostare i callback `next`, `error`e `complete` per ricevere le notifiche dalla chiamata `stream`.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dal client, chiamare il metodo `dispose` sul `ISubscription` restituito dal metodo `subscribe`. La chiamata a questo metodo provoca l'annullamento del parametro `CancellationToken` del metodo Hub, se ne è stato specificato uno.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dal client, chiamare il metodo `dispose` sul `ISubscription` restituito dal metodo `subscribe`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Streaming da client a server

I client JavaScript chiamano i metodi di streaming da client a server negli hub passando un `Subject` come argomento per `send`, `invoke`o `stream`, a seconda del metodo dell'hub richiamato. Il `Subject` è una classe che ha un aspetto simile a una `Subject`. Ad esempio, in RxJS è possibile usare la classe [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) di tale libreria.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

La chiamata di `subject.next(item)` con un elemento scrive l'elemento nel flusso e il metodo Hub riceve l'elemento nel server.

Per terminare il flusso, chiamare `subject.complete()`.

## <a name="java-client"></a>Client Java

### <a name="server-to-client-streaming"></a>Streaming da server a client

Il client Java SignalR usa il metodo `stream` per richiamare i metodi di streaming. `stream` accetta tre o più argomenti:

* Tipo previsto degli elementi del flusso.
* Il nome del metodo dell'hub.
* Argomenti definiti nel metodo dell'hub.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

Il metodo `stream` su `HubConnection` restituisce un oggetto osservabile del tipo di elemento del flusso. Il metodo `subscribe` del tipo osservabile è il punto in cui vengono definiti i gestori `onNext`, `onError` e `onCompleted`.

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
