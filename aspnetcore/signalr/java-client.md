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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="ecee3-103">Client ASP.NET Core SignalR Java</span><span class="sxs-lookup"><span data-stu-id="ecee3-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="ecee3-104">Da [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="ecee3-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="ecee3-105">Il client Java consente la connessione a un server di ASP.NET Core SignalR dal codice Java, incluse le app Android.</span><span class="sxs-lookup"><span data-stu-id="ecee3-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="ecee3-106">Ad esempio la [client JavaScript](xref:signalr/javascript-client) e il [client .NET](xref:signalr/dotnet-client), il client Java consente di ricevere e inviare messaggi a un hub in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="ecee3-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="ecee3-107">Il client Java è disponibile in ASP.NET Core 2.2 e successive.</span><span class="sxs-lookup"><span data-stu-id="ecee3-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="ecee3-108">L'app console Java di esempio citato nel presente articolo usa il client Java di SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecee3-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="ecee3-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ecee3-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="ecee3-110">Installare il pacchetto client Java di SignalR</span><span class="sxs-lookup"><span data-stu-id="ecee3-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="ecee3-111">Il *signalr-1.0.0-preview3-35501* file con estensione JAR consente ai client di connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecee3-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ecee3-112">Per trovare il numero di versione di file con estensione JAR più recente, vedere la [risultati della ricerca di Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="ecee3-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="ecee3-113">Se si usa Gradle, aggiungere la riga seguente al `dependencies` sezione il *Build. gradle* file:</span><span class="sxs-lookup"><span data-stu-id="ecee3-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="ecee3-114">Se con Maven, aggiungere le seguenti righe all'interno di `<dependencies>` elemento delle *POM. XML* file:</span><span class="sxs-lookup"><span data-stu-id="ecee3-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="ecee3-115">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="ecee3-115">Connect to a hub</span></span>

<span data-ttu-id="ecee3-116">Per stabilire una `HubConnection`, il `HubConnectionBuilder` deve essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="ecee3-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="ecee3-117">Il livello di log e URL hub può essere configurato durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="ecee3-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="ecee3-118">Configurare le opzioni necessarie, chiamare uno dei `HubConnectionBuilder` metodi prima `build`.</span><span class="sxs-lookup"><span data-stu-id="ecee3-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="ecee3-119">Avviare la connessione con `start`.</span><span class="sxs-lookup"><span data-stu-id="ecee3-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ecee3-120">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="ecee3-120">Call hub methods from client</span></span>

<span data-ttu-id="ecee3-121">Una chiamata a `send` richiama un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ecee3-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="ecee3-122">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `send`.</span><span class="sxs-lookup"><span data-stu-id="ecee3-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ecee3-123">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="ecee3-123">Call client methods from hub</span></span>

<span data-ttu-id="ecee3-124">Usare `hubConnection.on` per definire i metodi sul client che può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="ecee3-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="ecee3-125">Definire i metodi dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="ecee3-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="ecee3-126">Aggiungere la registrazione</span><span class="sxs-lookup"><span data-stu-id="ecee3-126">Add logging</span></span>

<span data-ttu-id="ecee3-127">Il client SignalR Java Usa il [SLF4J](https://www.slf4j.org/) libreria per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="ecee3-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ecee3-128">È un'API di registrazione di alto livello che consente agli utenti della libreria scegliere la propria implementazione di registrazione specifici, grazie alla disponibilità di una dipendenza di registrazione specifici.</span><span class="sxs-lookup"><span data-stu-id="ecee3-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ecee3-129">Il frammento di codice seguente viene illustrato come utilizzare `java.util.logging` con il client Java di SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecee3-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ecee3-130">Se non si configura la registrazione nelle dipendenze, SLF4J carica un logger di alcuna operazione predefinita con il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="ecee3-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ecee3-131">Ciò può essere ignorato.</span><span class="sxs-lookup"><span data-stu-id="ecee3-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="ecee3-132">Limitazioni note</span><span class="sxs-lookup"><span data-stu-id="ecee3-132">Known limitations</span></span>

<span data-ttu-id="ecee3-133">Si tratta di una versione di anteprima del client Java.</span><span class="sxs-lookup"><span data-stu-id="ecee3-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="ecee3-134">Alcune funzionalità non sono supportate:</span><span class="sxs-lookup"><span data-stu-id="ecee3-134">Some features aren't supported:</span></span>

* <span data-ttu-id="ecee3-135">È supportato solo il protocollo JSON.</span><span class="sxs-lookup"><span data-stu-id="ecee3-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="ecee3-136">È supportato solo il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ecee3-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="ecee3-137">Lo streaming non è ancora supportato.</span><span class="sxs-lookup"><span data-stu-id="ecee3-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecee3-138">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ecee3-138">Additional resources</span></span>

* [<span data-ttu-id="ecee3-139">Informazioni di riferimento sulle API Java</span><span class="sxs-lookup"><span data-stu-id="ecee3-139">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
