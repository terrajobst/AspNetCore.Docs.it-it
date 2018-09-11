---
title: Client .NET di ASP.NET Core SignalR
author: tdykstra
description: Informazioni relative al Client .NET di ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373319"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="67ad8-103">Client .NET di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="67ad8-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="67ad8-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="67ad8-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="67ad8-105">La libreria client .NET di ASP.NET Core SignalR consente di comunicare con gli hub SignalR dalle app .NET.</span><span class="sxs-lookup"><span data-stu-id="67ad8-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="67ad8-106">Xamarin sono previsti requisiti speciali per la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67ad8-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="67ad8-107">Per altre informazioni, vedere [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="67ad8-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="67ad8-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67ad8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="67ad8-109">Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="67ad8-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="67ad8-110">Installare il pacchetto del client .NET di SignalR</span><span class="sxs-lookup"><span data-stu-id="67ad8-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="67ad8-111">Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="67ad8-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="67ad8-112">Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:</span><span class="sxs-lookup"><span data-stu-id="67ad8-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="67ad8-113">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="67ad8-113">Connect to a hub</span></span>

<span data-ttu-id="67ad8-114">Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`.</span><span class="sxs-lookup"><span data-stu-id="67ad8-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="67ad8-115">L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="67ad8-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="67ad8-116">Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`.</span><span class="sxs-lookup"><span data-stu-id="67ad8-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="67ad8-117">Avviare la connessione con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="67ad8-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="67ad8-118">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="67ad8-118">Call hub methods from client</span></span>

<span data-ttu-id="67ad8-119">`InvokeAsync` chiama i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="67ad8-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="67ad8-120">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="67ad8-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="67ad8-121">SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.</span><span class="sxs-lookup"><span data-stu-id="67ad8-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="67ad8-122">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="67ad8-122">Call client methods from hub</span></span>

<span data-ttu-id="67ad8-123">Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="67ad8-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="67ad8-124">Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="67ad8-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="67ad8-125">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="67ad8-125">Error handling and logging</span></span>

<span data-ttu-id="67ad8-126">Gestire gli errori con un'istruzione try-catch.</span><span class="sxs-lookup"><span data-stu-id="67ad8-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="67ad8-127">Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="67ad8-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="67ad8-128">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="67ad8-128">Additional resources</span></span>

* [<span data-ttu-id="67ad8-129">Hub</span><span class="sxs-lookup"><span data-stu-id="67ad8-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="67ad8-130">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="67ad8-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="67ad8-131">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="67ad8-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
