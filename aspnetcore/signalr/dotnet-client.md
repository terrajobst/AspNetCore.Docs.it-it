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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="981c9-103">Client .NET di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="981c9-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="981c9-104">La libreria client .NET di ASP.NET Core SignalR consente di comunicare con gli hub SignalR dalle app .NET.</span><span class="sxs-lookup"><span data-stu-id="981c9-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="981c9-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="981c9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="981c9-106">Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="981c9-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="981c9-107">Installare il pacchetto del client .NET di SignalR</span><span class="sxs-lookup"><span data-stu-id="981c9-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="981c9-108">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="981c9-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="981c9-109">Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:</span><span class="sxs-lookup"><span data-stu-id="981c9-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="981c9-110">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="981c9-110">Connect to a hub</span></span>

<span data-ttu-id="981c9-111">Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`.</span><span class="sxs-lookup"><span data-stu-id="981c9-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="981c9-112">L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="981c9-113">Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`.</span><span class="sxs-lookup"><span data-stu-id="981c9-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="981c9-114">Avviare la connessione con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="981c9-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="981c9-115">Handle di connessione è stata interrotta</span><span class="sxs-lookup"><span data-stu-id="981c9-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="981c9-116">La riconnessione automatica</span><span class="sxs-lookup"><span data-stu-id="981c9-116">Automatically reconnect</span></span>

<span data-ttu-id="981c9-117">Il <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> può essere configurato per la riconnessione automatica usando la `WithAutomaticReconnect` metodo su di <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="981c9-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="981c9-118">Non riconnettersi automaticamente per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="981c9-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="981c9-119">Senza parametri, `WithAutomaticReconnect()` consente di configurare il client per l'attesa di 0, 2, 10 e 30 secondi rispettivamente prima di provare ogni tentativo di riconnessione, fermandosi quando quattro tentativi non riusciti.</span><span class="sxs-lookup"><span data-stu-id="981c9-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="981c9-120">Prima di avviare qualsiasi tentativo di riconnessione, il `HubConnection` verranno transizione per il `HubConnectionState.Reconnecting` lo stato e la generazione del `Reconnecting` evento.</span><span class="sxs-lookup"><span data-stu-id="981c9-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="981c9-121">Ciò offre un'opportunità per avvisare gli utenti che la connessione è stata persa e disattivare gli elementi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="981c9-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="981c9-122">Le app non interattivi possono avviare Accodamento messaggi o l'eliminazione dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="981c9-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="981c9-123">Se il client si riconnette correttamente entro i primi quattro diversi, il `HubConnection` passerà al `Connected` lo stato e la generazione del `Reconnected` evento.</span><span class="sxs-lookup"><span data-stu-id="981c9-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="981c9-124">Ciò offre un'opportunità per informare gli utenti la connessione è stata ristabilita e tutti i messaggi in coda di rimozione dalla coda.</span><span class="sxs-lookup"><span data-stu-id="981c9-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="981c9-125">Poiché la connessione sia completamente nuovo al server, un nuovo `ConnectionId` riceveranno la `Reconnected` gestori eventi.</span><span class="sxs-lookup"><span data-stu-id="981c9-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="981c9-126">Il `Reconnected` del gestore eventi `connectionId` parametro sarà null se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="981c9-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="981c9-127">`WithAutomaticReconnect()` non configurare il `HubConnection` tentativo iniziale di avvio non riuscite, in modo che gli errori di avvio devono essere gestite manualmente:</span><span class="sxs-lookup"><span data-stu-id="981c9-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="981c9-128">Se il client non è stata riconnettersi entro i primi quattro diversi, il `HubConnection` verranno transizione per il `Disconnected` lo stato e la generazione del <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> evento.</span><span class="sxs-lookup"><span data-stu-id="981c9-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="981c9-129">Ciò offre un'opportunità per tentare di riavviare manualmente la connessione o informare gli utenti che la connessione viene definitivamente perduta.</span><span class="sxs-lookup"><span data-stu-id="981c9-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="981c9-130">Per configurare un numero di tentativi di riconnessione prima della disconnessione personalizzato o modificare l'intervallo di tempo di riconnessione, `WithAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="981c9-131">Nell'esempio precedente viene configurata la `HubConnection` per avviare il tentativo di riconnessioni immediatamente dopo la connessione viene persa.</span><span class="sxs-lookup"><span data-stu-id="981c9-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="981c9-132">Questo vale anche per la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="981c9-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="981c9-133">Se non riesce al primo tentativo di riconnessione, il secondo tentativo di riconnessione anche inizierà immediatamente anziché attendere 2 secondi, come verrebbe mostrata nella configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="981c9-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="981c9-134">Se ha esito negativo al secondo tentativo di riconnessione, il terzo tentativo di riconnessione verrà avviato entro 10 secondi, ovvero anche in questo caso, ad esempio la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="981c9-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="981c9-135">Il comportamento personalizzato quindi differisce anche in questo caso il comportamento predefinito arrestando dopo la riconnessione terzo tentativo errore.</span><span class="sxs-lookup"><span data-stu-id="981c9-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="981c9-136">Nella configurazione predefinita esiste potrebbe essere uno più riconnettersi tentativo in un altro 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="981c9-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="981c9-137">Se si desidera maggiore controllo sulla temporizzazione e numero di automatico tentativi, di riconnessione `WithAutomaticReconnect` accetta un oggetto che implementa le `IRetryPolicy` interfaccia che dispone di un singolo metodo denominato `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="981c9-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="981c9-138">`NextRetryDelay` accetta un singolo argomento con il tipo `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="981c9-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="981c9-139">Il `RetryContext` presenta tre proprietà: `PreviousRetryCount`, `ElapsedTime` e `RetryReason` che sono una `long`, un `TimeSpan` e un `Exception` rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="981c9-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="981c9-140">Prima il primo tentativo di riconnessione, entrambe `PreviousRetryCount` e `ElapsedTime` saranno pari a zero e il `RetryReason` sarà l'eccezione che ha causato la connessione persa.</span><span class="sxs-lookup"><span data-stu-id="981c9-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="981c9-141">Dopo ogni tentativo di ripetizione dei tentativi non riusciti, `PreviousRetryCount` verrà incrementato di uno `ElapsedTime` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione fino ad ora e il `RetryReason` sarà l'eccezione che ha causato l'ultimo tentativo di riconnessione esito negativo.</span><span class="sxs-lookup"><span data-stu-id="981c9-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="981c9-142">`NextRetryDelay` deve restituire entrambi un oggetto TimeSpan che rappresenta il tempo di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve interrompere la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="981c9-143">In alternativa, è possibile scrivere codice che si riconnetterà il client manualmente, come illustrato in [riconnettere manualmente](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="981c9-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="981c9-144">Riconnettere manualmente</span><span class="sxs-lookup"><span data-stu-id="981c9-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="981c9-145">Prima 3.0, non riconnettersi automaticamente il client .NET per SignalR.</span><span class="sxs-lookup"><span data-stu-id="981c9-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="981c9-146">È necessario scrivere codice che si riconnetterà manualmente il client.</span><span class="sxs-lookup"><span data-stu-id="981c9-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="981c9-147">Usare il <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventi per rispondere a una connessione è stata persa.</span><span class="sxs-lookup"><span data-stu-id="981c9-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="981c9-148">Ad esempio, è possibile automatizzare la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="981c9-149">Il `Closed` eventi richiede un delegato che restituisce un `Task`, che consente di eseguire senza usare codice asincrono `async void`.</span><span class="sxs-lookup"><span data-stu-id="981c9-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="981c9-150">Per soddisfare la firma del delegato in un `Closed` gestore dell'evento che viene eseguito in modo sincrono, restituisce `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="981c9-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="981c9-151">Il motivo principale per il supporto asincrono è in modo che sia possibile riavviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="981c9-152">L'avvio di una connessione è un'azione asincrona.</span><span class="sxs-lookup"><span data-stu-id="981c9-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="981c9-153">In un `Closed` gestore che riavvia la connessione, è consigliabile attendere un ritardo casuale impedire il sovraccarico del server, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="981c9-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="981c9-154">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="981c9-154">Call hub methods from client</span></span>

<span data-ttu-id="981c9-155">`InvokeAsync` chiama i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="981c9-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="981c9-156">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="981c9-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="981c9-157">SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.</span><span class="sxs-lookup"><span data-stu-id="981c9-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="981c9-158">Il `InvokeAsync` metodo restituisce un `Task` che viene completato quando restituito dal metodo server.</span><span class="sxs-lookup"><span data-stu-id="981c9-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="981c9-159">Il valore restituito, se presente, viene fornito come risultato del `Task`.</span><span class="sxs-lookup"><span data-stu-id="981c9-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="981c9-160">Tutte le eccezioni generate dal metodo nel server di producono un errore `Task`.</span><span class="sxs-lookup"><span data-stu-id="981c9-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="981c9-161">Uso `await` sintassi per attendere il completamento del metodo server e `try...catch` sintassi per la gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="981c9-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="981c9-162">Il `SendAsync` metodo restituisce un `Task` che viene completato quando il messaggio è stato inviato al server.</span><span class="sxs-lookup"><span data-stu-id="981c9-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="981c9-163">Viene specificato alcun valore restituito poiché `Task` non attendere fino al completamento del metodo server.</span><span class="sxs-lookup"><span data-stu-id="981c9-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="981c9-164">Tutte le eccezioni generate nel client durante l'invio del messaggio di producono un errore `Task`.</span><span class="sxs-lookup"><span data-stu-id="981c9-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="981c9-165">Uso `await` e `try...catch` sintassi per gestire errori di invio.</span><span class="sxs-lookup"><span data-stu-id="981c9-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="981c9-166">Se si usa il servizio Azure SignalR in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client.</span><span class="sxs-lookup"><span data-stu-id="981c9-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="981c9-167">Per altre informazioni, vedere la [documentazione di SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="981c9-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="981c9-168">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="981c9-168">Call client methods from hub</span></span>

<span data-ttu-id="981c9-169">Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="981c9-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="981c9-170">Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="981c9-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="981c9-171">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="981c9-171">Error handling and logging</span></span>

<span data-ttu-id="981c9-172">Gestire gli errori con un'istruzione try-catch.</span><span class="sxs-lookup"><span data-stu-id="981c9-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="981c9-173">Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="981c9-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="981c9-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="981c9-174">Additional resources</span></span>

* [<span data-ttu-id="981c9-175">Hub</span><span class="sxs-lookup"><span data-stu-id="981c9-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="981c9-176">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="981c9-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="981c9-177">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="981c9-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="981c9-178">Documentazione senza server di Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="981c9-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
