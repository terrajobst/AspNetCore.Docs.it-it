---
title: Usare lo streaming in ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 70f12999b7f4230147b9ea43f6f7730b0816c43a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206388"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usare lo streaming in ASP.NET Core SignalR

Da [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR supporta streaming valori restituiti dei metodi del server. Ciò è utile per scenari in cui risulterà frammenti di dati nel corso del tempo. Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, anziché attendere che tutti i dati diventino disponibili.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurare l'hub

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`. Di seguito è un esempio che illustra le nozioni di base del flusso di dati al client. Ogni volta che un oggetto viene scritto il `ChannelReader` quell'oggetto viene inviato immediatamente al client. Al termine, il `ChannelReader` sia completata per indicare al client il flusso è chiuso.

> [!NOTE]
> Scrivere il `ChannelReader` su un thread in background e restituire il `ChannelReader` appena possibile. Altre chiamate dell'hub verranno bloccate fino a un `ChannelReader` viene restituito.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Client .NET

Il `StreamAsChannelAsync` metodo `HubConnection` viene utilizzato per richiamare un metodo di streaming. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client. Per leggere i dati, un modello comune consiste nell'eseguire un ciclo nei `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati. Il ciclo termina quando il server ha chiuso il flusso o il token di annullamento passato a `StreamAsChannelAsync` viene annullata.

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

## <a name="javascript-client"></a>Client JavaScript

I client JavaScript chiamano metodi streaming in hub usando `connection.stream`. Il `stream` metodo accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.
* Argomenti definiti nel metodo dell'hub. Nell'esempio seguente, gli argomenti sono: un conteggio per il numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.

`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` (metodo). Passare un `IStreamSubscriber` a `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere le notifiche di `stream` chiamata.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dal client, chiamare il `dispose` metodo sul `ISubscription` restituito dal `subscribe` (metodo).

## <a name="related-resources"></a>Risorse correlate

* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
