---
title: Client .NET di ASP.NET Core SignalR
author: tdykstra
description: Informazioni relative al Client .NET di ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 488d2ec31ce71534eeff4b9428262cc9ca00d992
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205952"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="10f15-103">Client .NET di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="10f15-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="10f15-104">La libreria client .NET di ASP.NET Core SignalR consente di comunicare con gli hub SignalR dalle app .NET.</span><span class="sxs-lookup"><span data-stu-id="10f15-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="10f15-105">Xamarin sono previsti requisiti speciali per la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10f15-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="10f15-106">Per altre informazioni, vedere [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="10f15-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="10f15-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10f15-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10f15-108">Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="10f15-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="10f15-109">Installare il pacchetto del client .NET di SignalR</span><span class="sxs-lookup"><span data-stu-id="10f15-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="10f15-110">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="10f15-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="10f15-111">Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:</span><span class="sxs-lookup"><span data-stu-id="10f15-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="10f15-112">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="10f15-112">Connect to a hub</span></span>

<span data-ttu-id="10f15-113">Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`.</span><span class="sxs-lookup"><span data-stu-id="10f15-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="10f15-114">L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="10f15-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="10f15-115">Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`.</span><span class="sxs-lookup"><span data-stu-id="10f15-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="10f15-116">Avviare la connessione con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="10f15-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="10f15-117">Handle di connessione è stata interrotta</span><span class="sxs-lookup"><span data-stu-id="10f15-117">Handle lost connection</span></span>

<span data-ttu-id="10f15-118">Usare il <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventi per rispondere a una connessione è stata persa.</span><span class="sxs-lookup"><span data-stu-id="10f15-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="10f15-119">Ad esempio, è possibile automatizzare la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="10f15-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="10f15-120">Il `Closed` eventi richiede un delegato che restituisce un `Task`, che consente di eseguire senza usare codice asincrono `async void`.</span><span class="sxs-lookup"><span data-stu-id="10f15-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="10f15-121">Per soddisfare la firma del delegato in un `Closed` gestore dell'evento che viene eseguito in modo sincrono, restituisce `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="10f15-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="10f15-122">Il motivo principale per il supporto asincrono è in modo che sia possibile riavviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="10f15-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="10f15-123">L'avvio di una connessione è un'azione asincrona.</span><span class="sxs-lookup"><span data-stu-id="10f15-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="10f15-124">In un `Closed` gestore che riavvia la connessione, è consigliabile attendere un ritardo casuale impedire il sovraccarico del server, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="10f15-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="10f15-125">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="10f15-125">Call hub methods from client</span></span>

<span data-ttu-id="10f15-126">`InvokeAsync` chiama i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="10f15-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="10f15-127">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="10f15-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="10f15-128">SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.</span><span class="sxs-lookup"><span data-stu-id="10f15-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="10f15-129">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="10f15-129">Call client methods from hub</span></span>

<span data-ttu-id="10f15-130">Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="10f15-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="10f15-131">Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="10f15-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="10f15-132">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="10f15-132">Error handling and logging</span></span>

<span data-ttu-id="10f15-133">Gestire gli errori con un'istruzione try-catch.</span><span class="sxs-lookup"><span data-stu-id="10f15-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="10f15-134">Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="10f15-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="10f15-135">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="10f15-135">Additional resources</span></span>

* [<span data-ttu-id="10f15-136">Hub</span><span class="sxs-lookup"><span data-stu-id="10f15-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="10f15-137">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="10f15-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="10f15-138">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="10f15-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
