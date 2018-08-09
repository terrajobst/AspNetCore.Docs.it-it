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
# <a name="aspnet-core-signalr-net-client"></a>Client .NET di ASP.NET Core SignalR

[Rachel Appel](http://twitter.com/rachelappel)

Il client di ASP.NET Core SignalR .NET può essere usato dalle app .NET Core, Console, Windows Form, WPF e Xamarin. Ad esempio la [client JavaScript](xref:signalr/javascript-client), il client .NET consente di ricevere e inviare e ricevere messaggi a un hub in tempo reale.

> [!NOTE]
> Xamarin sono previsti requisiti speciali per la versione di Visual Studio. Per altre informazioni, vedere [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Nell'esempio di codice in questo articolo è un'app WPF che utilizza il client .NET di ASP.NET Core SignalR.

## <a name="install-the-signalr-net-client-package"></a>Installare il pacchetto del client .NET di SignalR

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per connettersi a un hub SignalR. Per installare la libreria client, eseguire il comando seguente nel **Console di gestione pacchetti** finestra:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una connessione, creare un `HubConnectionBuilder` e chiamare `Build`. L'URL dell'hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione. Configurare le opzioni necessarie mediante l'inserimento di uno qualsiasi dei `HubConnectionBuilder` metodi in `Build`. Avviare la connessione con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

`InvokeAsync` chiama i metodi dell'hub. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `InvokeAsync`. SignalR è asincrona, utilizzare `async` e `await` durante le chiamate.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Definire i metodi dell'hub le chiamate effettuate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Il codice precedente in `connection.On` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>La registrazione e la gestione degli errori

Gestire gli errori con un'istruzione try-catch. Esaminare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
