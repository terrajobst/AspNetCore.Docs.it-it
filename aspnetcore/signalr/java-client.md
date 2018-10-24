---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Informazioni su come usare il client Java di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 77ea338f08b1986e69ba8ef1578c4cfe01a310de
ms.sourcegitcommit: ce6b6792c650708e92cdea051a5d166c0708c7c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49652306"
---
# <a name="aspnet-core-signalr-java-client"></a>Client ASP.NET Core SignalR Java

Da [Mikael Mengistu](https://twitter.com/MikaelM_12)

Il client Java consente la connessione a un server di ASP.NET Core SignalR dal codice Java, incluse le app Android. Ad esempio la [client JavaScript](xref:signalr/javascript-client) e il [client .NET](xref:signalr/dotnet-client), il client Java consente di ricevere e inviare messaggi a un hub in tempo reale. Il client Java è disponibile in ASP.NET Core 2.2 e successive.

L'app console Java di esempio citato nel presente articolo usa il client Java di SignalR.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installare il pacchetto client Java di SignalR

Il *signalr-1.0.0-preview3-35501* file con estensione JAR consente ai client di connettersi a un hub SignalR. Per trovare il numero di versione di file con estensione JAR più recente, vedere la [risultati della ricerca di Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Se si usa Gradle, aggiungere la riga seguente al `dependencies` sezione il *Build. gradle* file:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Se con Maven, aggiungere le seguenti righe all'interno di `<dependencies>` elemento delle *POM. XML* file:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una `HubConnection`, il `HubConnectionBuilder` deve essere utilizzato. Il livello di log e URL hub può essere configurato durante la compilazione di una connessione. Configurare le opzioni necessarie, chiamare uno dei `HubConnectionBuilder` metodi prima `build`. Avviare la connessione con `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

Una chiamata a `send` richiama un metodo dell'hub. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Usare `hubConnection.on` per definire i metodi sul client che può chiamare l'hub. Definire i metodi dopo la compilazione, ma prima di avviare la connessione.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Aggiungere la registrazione

Il client SignalR Java Usa il [SLF4J](https://www.slf4j.org/) libreria per la registrazione. È un'API di registrazione di alto livello che consente agli utenti della libreria scegliere la propria implementazione di registrazione specifici, grazie alla disponibilità di una dipendenza di registrazione specifici. Il frammento di codice seguente viene illustrato come utilizzare `java.util.logging` con il client Java di SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger di alcuna operazione predefinita con il messaggio di avviso seguente:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Ciò può essere ignorato.

## <a name="known-limitations"></a>Limitazioni note

Si tratta di una versione di anteprima del client Java. Alcune funzionalità non sono supportate:

* È supportato solo il protocollo JSON.
* È supportato solo il trasporto WebSocket.
* Lo streaming non è ancora supportato.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Informazioni di riferimento sulle API Java](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
