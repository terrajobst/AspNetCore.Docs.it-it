---
title: Utilizzare il flusso in ASP.NET SignalR Core
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358431"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Utilizzare il flusso in ASP.NET SignalR Core

Da [Brennan Conroy](https://github.com/BrennanConroy)

SignalR ASP.NET Core supporta streaming valori restituiti dei metodi di server. Ciò è utile per scenari in cui frammenti di dati saranno quello nel corso del tempo. Quando un valore restituito viene trasmesso al client, ogni frammento viene inviato al client, non appena diventa disponibile, piuttosto che in attesa di tutti i dati diventano disponibili.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Impostare l'hub

Un metodo dell'hub diventa automaticamente un metodo dell'hub sul flusso quando viene restituito un `ChannelReader<T>` o un `Task<ChannelReader<T>>`. Di seguito è riportato un esempio che illustra le nozioni di base del flusso di dati al client. Ogni volta che un oggetto viene scritto il `ChannelReader` quell'oggetto viene inviato immediatamente al client. Al termine, il `ChannelReader` sia completata per indicare al client il flusso è chiuso.

> [!NOTE]
> Scrivere il `ChannelReader` su un thread in background e restituire il `ChannelReader` appena possibile. Altre chiamate dell'hub verranno bloccate finché un `ChannelReader` viene restituito.

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>Client .NET

Il `StreamAsChannelAsync` metodo su `HubConnection` viene utilizzato per richiamare un metodo di streaming. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `StreamAsChannelAsync`. Il parametro generico in `StreamAsChannelAsync<T>` specifica il tipo di oggetti restituiti dal metodo di streaming. Oggetto `ChannelReader<T>` viene restituito dalla chiamata del flusso e rappresenta il flusso nel client. Per leggere i dati, un modello comune consiste nel ciclo sugli `WaitToReadAsync` e chiamare `TryRead` quando sono disponibili dati. Il ciclo terminerà quando il flusso è stato chiuso dal server oppure il token di annullamento passato a `StreamAsChannelAsync` viene annullata.

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

I client JavaScript chiamare metodi streaming su hub utilizzando `connection.stream`. Il `stream` metodo accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente, è il nome del metodo dell'hub `Counter`.
* Argomenti definiti del metodo dell'hub. Nell'esempio seguente, gli argomenti sono: un conteggio del numero di elementi di flusso per ricevere e il ritardo tra gli elementi di flusso.

`connection.stream` Restituisce un `IStreamResult` che contiene un `subscribe` metodo. Passare un `IStreamSubscriber` al `subscribe` e impostare il `next`, `error`, e `complete` i callback per ricevere notifiche dal `stream` chiamata.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Per terminare il flusso dalla chiamata al client il `dispose` metodo sul `ISubscription` restituito dal `subscribe` metodo.

## <a name="related-resources"></a>Risorse correlate

* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)