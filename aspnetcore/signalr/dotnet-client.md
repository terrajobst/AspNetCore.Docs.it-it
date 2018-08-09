---
title: Client .NET di ASP.NET Core SignalR
author: tdykstra
description: Informazioni relative al Client .NET di ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655252"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="077be-103">Client .NET di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="077be-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="077be-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="077be-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="077be-105">Il client di ASP.NET Core SignalR .NET può essere usato dalle app .NET Core, Console, Windows Form, WPF e Xamarin.</span><span class="sxs-lookup"><span data-stu-id="077be-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="077be-106">Ad esempio la [client JavaScript](xref:signalr/javascript-client), il client .NET consente di ricevere e inviare e ricevere messaggi a un hub in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="077be-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="077be-107">Xamarin sono previsti requisiti speciali per la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="077be-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="077be-108">Per altre informazioni, vedere [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="077be-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="077be-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="077be-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="077be-110">Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="077be-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="077be-111">Installare il pacchetto del client .NET di SignalR</span><span class="sxs-lookup"><span data-stu-id="077be-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="077be-112">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="077be-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="077be-113">Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:</span><span class="sxs-lookup"><span data-stu-id="077be-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="077be-114">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="077be-114">Connect to a hub</span></span>

<span data-ttu-id="077be-115">Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`.</span><span class="sxs-lookup"><span data-stu-id="077be-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="077be-116">L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="077be-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="077be-117">Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`.</span><span class="sxs-lookup"><span data-stu-id="077be-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="077be-118">Avviare la connessione con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="077be-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="077be-119">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="077be-119">Call hub methods from client</span></span>

<span data-ttu-id="077be-120">`InvokeAsync` chiama i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="077be-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="077be-121">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="077be-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="077be-122">SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.</span><span class="sxs-lookup"><span data-stu-id="077be-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="077be-123">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="077be-123">Call client methods from hub</span></span>

<span data-ttu-id="077be-124">Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="077be-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="077be-125">Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="077be-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="077be-126">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="077be-126">Error handling and logging</span></span>

<span data-ttu-id="077be-127">Gestire gli errori con un'istruzione try-catch.</span><span class="sxs-lookup"><span data-stu-id="077be-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="077be-128">Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="077be-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="077be-129">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="077be-129">Additional resources</span></span>

* [<span data-ttu-id="077be-130">Hub</span><span class="sxs-lookup"><span data-stu-id="077be-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="077be-131">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="077be-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="077be-132">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="077be-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
