---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Informazioni su come usare il client Java di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 06/27/2019
uid: signalr/java-client
ms.openlocfilehash: eea1dfb7d8afcd34c0dacd8315ad196d7235c9f7
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463276"
---
# <a name="aspnet-core-signalr-java-client"></a>Client ASP.NET Core SignalR Java

Da [Mikael Mengistu](https://twitter.com/MikaelM_12)

Il client Java consente la connessione a un server di ASP.NET Core SignalR dal codice Java, incluse le app Android. Ad esempio la [client JavaScript](xref:signalr/javascript-client) e il [client .NET](xref:signalr/dotnet-client), il client Java consente di ricevere e inviare messaggi a un hub in tempo reale. Il client Java è disponibile in ASP.NET Core 2.2 e successive.

L'app console Java di esempio citato nel presente articolo usa il client Java di SignalR.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Installare il pacchetto client Java di SignalR

Il *signalr 1.0.0* file con estensione JAR consente ai client di connettersi a un hub SignalR. Per trovare il numero di versione di file con estensione JAR più recente, vedere la [risultati della ricerca di Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Se si usa Gradle, aggiungere la riga seguente al `dependencies` sezione il *Build. gradle* file:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Se con Maven, aggiungere le seguenti righe all'interno di `<dependencies>` elemento delle *POM. XML* file:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Per stabilire una `HubConnection`, il `HubConnectionBuilder` deve essere utilizzato. Il livello di log e URL hub può essere configurato durante la compilazione di una connessione. Configurare le opzioni necessarie, chiamare uno dei `HubConnectionBuilder` metodi prima `build`. Avviare la connessione con `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

Una chiamata a `send` richiama un metodo dell'hub. Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Se si usa il servizio Azure SignalR in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client. Per altre informazioni, vedere la [documentazione di SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

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

## <a name="android-development-notes"></a>Note di sviluppo per Android

Per quanto riguarda la compatibilità di Android SDK per le funzionalità client SignalR, prendere in considerazione gli elementi seguenti quando si specifica la versione di Android SDK di destinazione:

* In Android API Level 16 e versioni successive, verrà eseguito il Client di Java SignalR.
* La connessione tramite il servizio Azure SignalR richiederanno Android livello API 20 e versioni successiva poiché il [servizio Azure SignalR](/azure/azure-signalr/signalr-overview) richiede TLS 1.2 e non supporta i pacchetti di crittografia basate su SHA-1. Android [aggiunti i pacchetti di crittografia di supporto per SHA-256 (e versioni successive)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) nel livello API 20.

## <a name="configure-bearer-token-authentication"></a>Configurare l'autenticazione del bearer token

Nel client Java di SignalR, è possibile configurare un token di connessione da utilizzare per l'autenticazione, fornendo una "token factory l'accesso" per il [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Uso [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) per fornire un' [RxJava](https://github.com/ReactiveX/RxJava) [singolo\<String >](http://reactivex.io/documentation/single.html). Con una chiamata a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), è possibile scrivere la logica per produrre i token di accesso per il client.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Limitazioni note

::: moniker range=">= aspnetcore-3.0"

* È supportato solo il protocollo JSON.
* Trasporto fallback e il trasporto di eventi del Server inviati non sono supportati.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* È supportato solo il protocollo JSON.
* È supportato solo il trasporto WebSocket.
* Lo streaming non è ancora supportato.

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Informazioni di riferimento sulle API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Documentazione senza server di Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
