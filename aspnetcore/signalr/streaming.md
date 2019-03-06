---
title: Usare lo streaming in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come restituire flussi di valori da metodi dell'hub sul server e utilizzare i flussi con i client .NET e JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345587"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usare lo streaming in ASP.NET Core SignalR

Da [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server. Ciò è utile per scenari in cui risulterà frammenti di dati nel corso del tempo. Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurare l'hub

::: moniker range=">= aspnetcore-3.0"

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

In ASP.NET Core 3.0 o versione successiva, i metodi dell'hub di streaming può restituire `IAsyncEnumerable<T>` oltre a `ChannelReader<T>`. Il modo più semplice per restituire `IAsyncEnumerable<T>` consiste nel rendere un metodo iteratore asincrono del metodo dell'hub come illustrato nell'esempio seguente. Metodi iteratori async hub possono accettare un `CancellationToken` parametro che verrà attivato quando il client annulla la sottoscrizione dal flusso. I metodi iterator Async evitare facilmente i problemi comuni con i canali, ad esempio non restituisce il `ChannelReader` iniziale in modo o metodo esistente senza completare la `ChannelWriter`.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

L'esempio seguente illustra le nozioni di base dei dati al client utilizzando i canali di streaming. Ogni volta che un oggetto viene scritto il `ChannelWriter` quell'oggetto viene inviato immediatamente al client. Al termine, il `ChannelWriter` sia completata per indicare al client il flusso è chiuso.

> [!NOTE]
> * Scrivere il `ChannelWriter` su un thread in background e restituire il `ChannelReader` appena possibile. Altre chiamate dell'hub verranno bloccate fino a un `ChannelReader` viene restituito.
> * Eseguire il wrapping la logica in una `try ... catch` e completare il `Channel` nel catch e all'esterno di catch in modo da assicurarsi che l'hub di chiamata al metodo viene completata correttamente.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

In ASP.NET Core 2.2 o versioni successive, i metodi dell'hub di streaming può accettare un `CancellationToken` parametro che verrà attivato quando il client annulla la sottoscrizione dal flusso. Usare questo token per l'operazione del server di arrestare e rilasciare le risorse se il client si disconnette prima della fine del flusso.

::: moniker-end

## <a name="net-client"></a>Client .NET

Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client. Per leggere i dati, un modello comune consiste nell'eseguire un ciclo nei `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati. Il ciclo termina quando il server ha chiuso il flusso o il token di annullamento passato a `StreamAsChannelAsync` viene annullata.

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

## <a name="javascript-client"></a>Client JavaScript

I client JavaScript chiamano metodi streaming in hub usando `connection.stream`. Il `stream` metodo accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.
* Argomenti definiti nel metodo dell'hub. Nell'esempio seguente, gli argomenti sono: un conteggio per il numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.

`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` (metodo). Passare un `IStreamSubscriber` a `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere le notifiche di `stream` chiamata.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo). Chiamare questo metodo genererà il `CancellationToken` parametro del metodo dell'Hub (se è stata specificata una) deve essere annullata.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a>Client Java
Il client Java di SignalR utilizza i `stream` metodo per richiamare i metodi di streaming. Accetta tre o più argomenti:

* Il tipo previsto degli elementi di flusso 
* Il nome del metodo dell'hub.
* Argomenti definiti nel metodo dell'hub. 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
Il `stream` metodo su `HubConnection` restituisce un oggetto osservabile del tipo di elemento flusso. Il tipo Observable `subscribe` metodo viene usata per definire le `onNext`, `onError` e `onCompleted` gestori.

::: moniker-end

## <a name="related-resources"></a>Risorse correlate

* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
