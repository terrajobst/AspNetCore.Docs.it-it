---
title: Utilizzo degli hub in ASP.NET SignalR Core
author: rachelappel
description: Informazioni sull'utilizzo degli hub in ASP.NET SignalR Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: e23d7ef6d5e5e93d5fc69ad4c845a6a896836170
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="cdc1e-103">Usare gli hub di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cdc1e-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="cdc1e-104">Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="cdc1e-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="cdc1e-105">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cdc1e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="cdc1e-106">Che cos'è un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="cdc1e-106">What is a SignalR hub</span></span>

<span data-ttu-id="cdc1e-107">L'API degli hub SignalR consente di chiamare metodi sul client connessi dal server.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="cdc1e-108">Nel codice server, si definiscono i metodi che vengono chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="cdc1e-109">Nel codice client, definire i metodi chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="cdc1e-110">SignalR si occupa di qualsiasi elemento dietro le quinte che permette la comunicazione client-server e il server al client in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="cdc1e-111">Configurare gli hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="cdc1e-111">Configure SignalR hubs</span></span>

<span data-ttu-id="cdc1e-112">Il middleware SignalR richiede alcuni servizi, sono configurate tramite la chiamata `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=37)]

<span data-ttu-id="cdc1e-113">Quando si aggiungono funzionalità SignalR a un'app di ASP.NET Core, del programma di installazione delle route SignalR chiamando `app.UseSignalR` nella `Startup.Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="cdc1e-114">Creare e usare gli hub</span><span class="sxs-lookup"><span data-stu-id="cdc1e-114">Create and use hubs</span></span>

<span data-ttu-id="cdc1e-115">Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungere metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="cdc1e-116">I client possono chiamare i metodi definiti come `public`.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="cdc1e-117">È possibile specificare un tipo restituito e parametri, inclusi i tipi complessi e matrici, esattamente come si farebbe con qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="cdc1e-118">SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="cdc1e-119">L'oggetto client</span><span class="sxs-lookup"><span data-stu-id="cdc1e-119">The Clients object</span></span>

<span data-ttu-id="cdc1e-120">Ogni istanza di `Hub` classe ha una proprietà denominata `Clients` che contiene i membri seguenti per la comunicazione tra client e server:</span><span class="sxs-lookup"><span data-stu-id="cdc1e-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="cdc1e-121">Proprietà</span><span class="sxs-lookup"><span data-stu-id="cdc1e-121">Property</span></span> | <span data-ttu-id="cdc1e-122">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cdc1e-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="cdc1e-123">Chiama un metodo su tutti i client connessi</span><span class="sxs-lookup"><span data-stu-id="cdc1e-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="cdc1e-124">Chiama un metodo nel client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="cdc1e-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="cdc1e-125">Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo</span><span class="sxs-lookup"><span data-stu-id="cdc1e-125">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="cdc1e-126">Inoltre, il `Hub` classe contiene i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdc1e-126">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="cdc1e-127">Metodo</span><span class="sxs-lookup"><span data-stu-id="cdc1e-127">Method</span></span> | <span data-ttu-id="cdc1e-128">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cdc1e-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="cdc1e-129">Chiama un metodo su tutti i client connessi ad eccezione delle connessioni specificate</span><span class="sxs-lookup"><span data-stu-id="cdc1e-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="cdc1e-130">Chiama un metodo in un client connesso specifico</span><span class="sxs-lookup"><span data-stu-id="cdc1e-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="cdc1e-131">Chiama un metodo specifico client connessi</span><span class="sxs-lookup"><span data-stu-id="cdc1e-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="cdc1e-132">Invia un messaggio a tutte le connessioni del gruppo specificato</span><span class="sxs-lookup"><span data-stu-id="cdc1e-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="cdc1e-133">Invia un messaggio a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="cdc1e-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="cdc1e-134">Invia un messaggio a più gruppi di connessioni</span><span class="sxs-lookup"><span data-stu-id="cdc1e-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="cdc1e-135">Invia un messaggio a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="cdc1e-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="cdc1e-136">Invia un messaggio a tutte le connessioni associate a un utente specifico</span><span class="sxs-lookup"><span data-stu-id="cdc1e-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="cdc1e-137">Invia un messaggio a tutte le connessioni associate con gli utenti specificati</span><span class="sxs-lookup"><span data-stu-id="cdc1e-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="cdc1e-138">Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="cdc1e-139">Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="cdc1e-140">Inviare messaggi ai client</span><span class="sxs-lookup"><span data-stu-id="cdc1e-140">Send messages to clients</span></span>

<span data-ttu-id="cdc1e-141">Per eseguire chiamate a client specifici, usare le proprietà del `Clients` oggetto.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="cdc1e-142">Nell'esempio seguente nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-142">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="cdc1e-143">Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominato `groups`.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="cdc1e-144">Gestire gli eventi per una connessione</span><span class="sxs-lookup"><span data-stu-id="cdc1e-144">Handle events for a connection</span></span>

<span data-ttu-id="cdc1e-145">L'API degli hub SignalR fornisce le `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="cdc1e-146">Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'hub, ad esempio l'aggiunta a un gruppo.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="cdc1e-147">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="cdc1e-147">Handle errors</span></span>

<span data-ttu-id="cdc1e-148">Le eccezioni generate nei metodi di hub vengono inviate al client che ha richiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="cdc1e-149">Nel client JavaScript, il `invoke` metodo restituisce un [suggerimento JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="cdc1e-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="cdc1e-150">Quando il client riceve un errore con un gestore associati all'oggetto promise mediante `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.</span><span class="sxs-lookup"><span data-stu-id="cdc1e-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=22)]

## <a name="related-resources"></a><span data-ttu-id="cdc1e-151">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="cdc1e-151">Related resources</span></span>

[<span data-ttu-id="cdc1e-152">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cdc1e-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)