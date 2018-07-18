---
title: Client .NET di ASP.NET Core SignalR
author: tdykstra
description: Informazioni relative al Client .NET di ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ce5be911e67831cbf6c09e24744111e73ffdbe63
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095034"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="936ec-103">Client .NET di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="936ec-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="936ec-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="936ec-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="936ec-105">Il client di ASP.NET Core SignalR .NET può essere usato dalle app .NET Core, Console, Windows Form, WPF e Xamarin.</span><span class="sxs-lookup"><span data-stu-id="936ec-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="936ec-106">Ad esempio la [client JavaScript](xref:signalr/javascript-client), il client .NET consente di ricevere e inviare e ricevere messaggi a un hub in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="936ec-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="936ec-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="936ec-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="936ec-108">Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="936ec-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="936ec-109">Installare il pacchetto del client .NET di SignalR</span><span class="sxs-lookup"><span data-stu-id="936ec-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="936ec-110">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="936ec-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="936ec-111">Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:</span><span class="sxs-lookup"><span data-stu-id="936ec-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="936ec-112">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="936ec-112">Connect to a hub</span></span>

<span data-ttu-id="936ec-113">Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`.</span><span class="sxs-lookup"><span data-stu-id="936ec-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="936ec-114">L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="936ec-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="936ec-115">Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`.</span><span class="sxs-lookup"><span data-stu-id="936ec-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="936ec-116">Avviare la connessione con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="936ec-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="936ec-117">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="936ec-117">Call hub methods from client</span></span>

<span data-ttu-id="936ec-118">`InvokeAsync` chiama i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="936ec-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="936ec-119">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="936ec-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="936ec-120">SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.</span><span class="sxs-lookup"><span data-stu-id="936ec-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="936ec-121">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="936ec-121">Call client methods from hub</span></span>

<span data-ttu-id="936ec-122">Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="936ec-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="936ec-123">Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="936ec-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="936ec-124">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="936ec-124">Error handling and logging</span></span>

<span data-ttu-id="936ec-125">Gestire gli errori con un'istruzione try-catch.</span><span class="sxs-lookup"><span data-stu-id="936ec-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="936ec-126">Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="936ec-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="936ec-127">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="936ec-127">Additional resources</span></span>

* [<span data-ttu-id="936ec-128">Hub</span><span class="sxs-lookup"><span data-stu-id="936ec-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="936ec-129">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="936ec-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="936ec-130">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="936ec-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
