---
title: Client .NET di ASP.NET Core SignalR
author: bradygaster
description: Informazioni relative al Client .NET di ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153125"
---
# <a name="aspnet-core-signalr-net-client"></a>Client .NET di ASP.NET Core SignalR

La libreria client .NET di ASP.NET Core SignalR consente di comunicare con gli hub SignalR dalle app .NET.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.

## <a name="install-the-signalr-net-client-package"></a>Installare il pacchetto del client .NET di SignalR

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR. Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`. L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione. Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`. Avviare la connessione con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Handle di connessione è stata interrotta

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>La riconnessione automatica

Il <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> può essere configurato per la riconnessione automatica usando la `WithAutomaticReconnect` metodo su di <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Non riconnettersi automaticamente per impostazione predefinita.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Senza parametri, `WithAutomaticReconnect()` consente di configurare il client per l'attesa di 0, 2, 10 e 30 secondi rispettivamente prima di provare ogni tentativo di riconnessione, fermandosi quando quattro tentativi non riusciti.

Prima di avviare qualsiasi tentativo di riconnessione, il `HubConnection` verranno transizione per il `HubConnectionState.Reconnecting` lo stato e la generazione del `Reconnecting` evento.  Ciò offre un'opportunità per avvisare gli utenti che la connessione è stata persa e disattivare gli elementi dell'interfaccia utente. Le app non interattivi possono avviare Accodamento messaggi o l'eliminazione dei messaggi.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Se il client si riconnette correttamente entro i primi quattro diversi, il `HubConnection` passerà al `Connected` lo stato e la generazione del `Reconnected` evento. Ciò offre un'opportunità per informare gli utenti la connessione è stata ristabilita e tutti i messaggi in coda di rimozione dalla coda.

Poiché la connessione sia completamente nuovo al server, un nuovo `ConnectionId` riceveranno la `Reconnected` gestori eventi.

> [!WARNING]
> Il `Reconnected` del gestore eventi `connectionId` parametro sarà null se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` non configurare il `HubConnection` tentativo iniziale di avvio non riuscite, in modo che gli errori di avvio devono essere gestite manualmente:

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

Se il client non è stata riconnettersi entro i primi quattro diversi, il `HubConnection` verranno transizione per il `Disconnected` lo stato e la generazione del <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> evento. Ciò offre un'opportunità per tentare di riavviare manualmente la connessione o informare gli utenti che la connessione viene definitivamente perduta.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Per configurare un numero di tentativi di riconnessione prima della disconnessione personalizzato o modificare l'intervallo di tempo di riconnessione, `WithAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

Nell'esempio precedente viene configurata la `HubConnection` per avviare il tentativo di riconnessioni immediatamente dopo la connessione viene persa. Questo vale anche per la configurazione predefinita.

Se non riesce al primo tentativo di riconnessione, il secondo tentativo di riconnessione anche inizierà immediatamente anziché attendere 2 secondi, come verrebbe mostrata nella configurazione predefinita.

Se ha esito negativo al secondo tentativo di riconnessione, il terzo tentativo di riconnessione verrà avviato entro 10 secondi, ovvero anche in questo caso, ad esempio la configurazione predefinita.

Il comportamento personalizzato quindi differisce anche in questo caso il comportamento predefinito arrestando dopo la riconnessione terzo tentativo errore. Nella configurazione predefinita esiste potrebbe essere uno più riconnettersi tentativo in un altro 30 secondi.

Se si desidera maggiore controllo sulla temporizzazione e numero di automatico tentativi, di riconnessione `WithAutomaticReconnect` accetta un oggetto che implementa le `IRetryPolicy` interfaccia che dispone di un singolo metodo denominato `NextRetryDelay`.

`NextRetryDelay` accetta un singolo argomento con il tipo `RetryContext`. Il `RetryContext` presenta tre proprietà: `PreviousRetryCount`, `ElapsedTime` e `RetryReason` che sono una `long`, un `TimeSpan` e un `Exception` rispettivamente. Prima il primo tentativo di riconnessione, entrambe `PreviousRetryCount` e `ElapsedTime` saranno pari a zero e il `RetryReason` sarà l'eccezione che ha causato la connessione persa. Dopo ogni tentativo di ripetizione dei tentativi non riusciti, `PreviousRetryCount` verrà incrementato di uno `ElapsedTime` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione fino ad ora e il `RetryReason` sarà l'eccezione che ha causato l'ultimo tentativo di riconnessione esito negativo.

`NextRetryDelay` deve restituire entrambi un oggetto TimeSpan che rappresenta il tempo di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve interrompere la riconnessione.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

In alternativa, è possibile scrivere codice che si riconnetterà il client manualmente, come illustrato in [riconnettere manualmente](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Riconnettere manualmente

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Prima 3.0, non riconnettersi automaticamente il client .NET per SignalR. È necessario scrivere codice che si riconnetterà manualmente il client.

::: moniker-end

Usare il <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventi per rispondere a una connessione è stata persa. Ad esempio, è possibile automatizzare la riconnessione.

Il `Closed` eventi richiede un delegato che restituisce un `Task`, che consente di eseguire senza usare codice asincrono `async void`. Per soddisfare la firma del delegato in un `Closed` gestore dell'evento che viene eseguito in modo sincrono, restituisce `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Il motivo principale per il supporto asincrono è in modo che sia possibile riavviare la connessione. L'avvio di una connessione è un'azione asincrona.

In un `Closed` gestore che riavvia la connessione, è consigliabile attendere un ritardo casuale impedire il sovraccarico del server, come illustrato nell'esempio seguente:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

`InvokeAsync` chiama i metodi dell'hub. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`. SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

Il `InvokeAsync` metodo restituisce un `Task` che viene completato quando restituito dal metodo server. Il valore restituito, se presente, viene fornito come risultato del `Task`. Tutte le eccezioni generate dal metodo nel server di producono un errore `Task`. Uso `await` sintassi per attendere il completamento del metodo server e `try...catch` sintassi per la gestione degli errori.

Il `SendAsync` metodo restituisce un `Task` che viene completato quando il messaggio è stato inviato al server. Viene specificato alcun valore restituito poiché `Task` non attendere fino al completamento del metodo server. Tutte le eccezioni generate nel client durante l'invio del messaggio di producono un errore `Task`. Uso `await` e `try...catch` sintassi per gestire errori di invio.

> [!NOTE]
> Se si usa il servizio Azure SignalR in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client. Per altre informazioni, vedere la [documentazione di SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>La registrazione e la gestione degli errori

Gestire gli errori con un'istruzione try-catch. Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
* [Documentazione senza server di Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
