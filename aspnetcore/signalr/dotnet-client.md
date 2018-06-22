---
title: Client .NET di base di ASP.NET SignalR
author: rachelappel
description: Informazioni relative al Client di .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273295"
---
# <a name="aspnet-core-signalr-net-client"></a>Client .NET di base di ASP.NET SignalR

[Rachel Appel](http://twitter.com/rachelappel)

Il client di ASP.NET Core SignalR .NET può essere usato da app Xamarin, WPF, Windows Form, la Console e .NET Core. Ad esempio il [client JavaScript](xref:signalr/javascript-client), il client .NET consente di ricevere e inviare e ricevere messaggi a un hub in tempo reale.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'esempio di codice in questo articolo è un'applicazione WPF che utilizza il client di ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Installare il pacchetto client .NET di SignalR

Il `Microsoft.AspNetCore.SignalR.Client` pacchetto è necessario per i client .NET per la connessione agli hub SignalR. Per installare la libreria client, eseguire il comando seguente **Console di gestione pacchetti** finestra:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una connessione, creare una `HubConnectionBuilder` e chiamare `Build`. L'URL di hub, protocollo, il tipo di trasporto, livello di registrazione, le intestazioni e altre opzioni possono essere configurati durante la compilazione di una connessione. Configurare le opzioni necessarie tramite l'inserimento di uno qualsiasi del `HubConnectionBuilder` metodi in `Build`. Avviare la connessione con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

`InvokeAsync` chiama i metodi dell'hub. Passare il nome del metodo dell'hub e gli eventuali argomenti definiti nel metodo dell'hub per `InvokeAsync`. SignalR è asincrono, prestare `async` e `await` quando si effettuano le chiamate.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi di client da hub

Definire i metodi dell'hub chiamate tramite `connection.On` dopo la compilazione, ma prima di avviare la connessione.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Il codice precedente in `connection.On` viene eseguito quando sul lato server viene chiamato dal codice utilizzando il `SendAsync` metodo.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>La registrazione e la gestione degli errori

Gestire gli errori con un'istruzione try-catch. Controllare il `Exception` oggetto per determinare l'azione appropriata da eseguire dopo che si verifica un errore.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)