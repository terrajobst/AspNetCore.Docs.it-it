---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Informazioni su come usare il client Java di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995415"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java Client

Da [Mikael Mengistu](https://twitter.com/MikaelM_12)

Il client Java consente la connessione a un server di ASP.NET Core SignalR dal codice Java, incluse le app Android. Ad esempio la [client JavaScript](xref:signalr/javascript-client) e il [client .NET](xref:signalr/dotnet-client), il client Java consente di ricevere e inviare messaggi a un hub in tempo reale. Il client Java è disponibile in ASP.NET Core 2.2 e successive.

L'app console Java di esempio citato nel presente articolo usa il client Java di SignalR.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installare il pacchetto client Java di SignalR

Il *signalr-0.1.0-preview1-35029* file con estensione JAR consente ai client di connettersi a un hub SignalR. Per trovare il numero di versione di file con estensione JAR più recente, vedere la [risultati della ricerca di Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Se si usa Gradle, aggiungere la riga seguente al `dependencies` sezione il *Build. gradle* file:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

Se con Maven, aggiungere le seguenti righe all'interno di `<dependencies>` elemento delle *POM. XML* file:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una `HubConnection`, il `HubConnectionBuilder` deve essere utilizzato. Il livello di log e URL hub può essere configurato durante la compilazione di una connessione. Configurare le opzioni necessarie, chiamare uno dei `HubConnectionBuilder` metodi prima `build`. Avviare la connessione con `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

Una chiamata a `send` richiama un metodo dell'hub. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Usare `hubConnection.on` per definire i metodi sul client che può chiamare l'hub. Definire i metodi dopo la compilazione, ma prima di avviare la connessione.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Limitazioni note

Si tratta di una versione di anteprima preliminare del client Java. Esistono molte funzionalità che non sono ancora supportate. I seguenti gap si sta lavorando per le versioni successive:

* Solo i tipi primitivi possono essere accettati in quanto i parametri e i tipi restituiscono.
* Le API sono sincrone.
* In questo momento è supportato solo il tipo di chiamata "Send". "Invoke" e lo streaming dei valori restituiti non sono supportati.
* Il client non supporta attualmente le [servizio Azure SignalR](/azure/azure-signalr/).
* È supportato solo il protocollo JSON.
* È supportato solo il trasporto WebSocket.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
