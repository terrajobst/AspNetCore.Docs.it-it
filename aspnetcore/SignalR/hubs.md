---
title: Utilizzo degli hub in ASP.NET SignalR Core
author: rachelappel
description: Informazioni sull'utilizzo degli hub in ASP.NET SignalR Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="056cc-103">Usare gli hub di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="056cc-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="056cc-104">Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="056cc-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="056cc-105">Che cos'è un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="056cc-105">What is a SignalR hub</span></span>

<span data-ttu-id="056cc-106">L'API degli hub SignalR consente di chiamare metodi sul client connessi dal server.</span><span class="sxs-lookup"><span data-stu-id="056cc-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="056cc-107">Nel codice server, si definiscono i metodi che vengono chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="056cc-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="056cc-108">Nel codice client, definire i metodi chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="056cc-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="056cc-109">SignalR si occupa di qualsiasi elemento dietro le quinte che permette la comunicazione client-server e il server al client in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="056cc-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="056cc-110">Configurare gli hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="056cc-110">Configure SignalR hubs</span></span>

<span data-ttu-id="056cc-111">Il middleware SignalR richiede alcuni servizi, sono configurate tramite la chiamata `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="056cc-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="056cc-112">Quando si aggiungono funzionalità SignalR a un'app di ASP.NET Core, del programma di installazione delle route SignalR chiamando `app.UseSignalR` nella `Startup.Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="056cc-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="056cc-113">Creare e usare gli hub</span><span class="sxs-lookup"><span data-stu-id="056cc-113">Create and use hubs</span></span>

<span data-ttu-id="056cc-114">Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungere metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="056cc-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="056cc-115">I client possono chiamare i metodi definiti come `public`.</span><span class="sxs-lookup"><span data-stu-id="056cc-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="056cc-116">È possibile specificare un tipo restituito e parametri, inclusi i tipi complessi e matrici, esattamente come si farebbe con qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="056cc-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="056cc-117">SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="056cc-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="056cc-118">L'oggetto client</span><span class="sxs-lookup"><span data-stu-id="056cc-118">The Clients object</span></span>

<span data-ttu-id="056cc-119">Ogni istanza di `Hub` classe ha una proprietà denominata `Clients` che contiene i membri seguenti per la comunicazione tra client e server:</span><span class="sxs-lookup"><span data-stu-id="056cc-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="056cc-120">Proprietà</span><span class="sxs-lookup"><span data-stu-id="056cc-120">Property</span></span> | <span data-ttu-id="056cc-121">Descrizione</span><span class="sxs-lookup"><span data-stu-id="056cc-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="056cc-122">Chiama un metodo su tutti i client connessi</span><span class="sxs-lookup"><span data-stu-id="056cc-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="056cc-123">Chiama un metodo nel client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="056cc-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="056cc-124">Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo</span><span class="sxs-lookup"><span data-stu-id="056cc-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="056cc-125">Inoltre, il `Hub` classe contiene i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="056cc-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="056cc-126">Metodo</span><span class="sxs-lookup"><span data-stu-id="056cc-126">Method</span></span> | <span data-ttu-id="056cc-127">Descrizione</span><span class="sxs-lookup"><span data-stu-id="056cc-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="056cc-128">Chiama un metodo su tutti i client connessi ad eccezione delle connessioni specificate</span><span class="sxs-lookup"><span data-stu-id="056cc-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="056cc-129">Chiama un metodo in un client connesso specifico</span><span class="sxs-lookup"><span data-stu-id="056cc-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="056cc-130">Chiama un metodo specifico client connessi</span><span class="sxs-lookup"><span data-stu-id="056cc-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="056cc-131">Invia un messaggio a tutte le connessioni del gruppo specificato</span><span class="sxs-lookup"><span data-stu-id="056cc-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="056cc-132">Invia un messaggio a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="056cc-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="056cc-133">Invia un messaggio a più gruppi di connessioni</span><span class="sxs-lookup"><span data-stu-id="056cc-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="056cc-134">Invia un messaggio a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="056cc-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="056cc-135">Invia un messaggio a tutte le connessioni associate a un utente specifico</span><span class="sxs-lookup"><span data-stu-id="056cc-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="056cc-136">Invia un messaggio a tutte le connessioni associate con gli utenti specificati</span><span class="sxs-lookup"><span data-stu-id="056cc-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="056cc-137">Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="056cc-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="056cc-138">Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="056cc-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="056cc-139">Inviare messaggi ai client</span><span class="sxs-lookup"><span data-stu-id="056cc-139">Send messages to clients</span></span>

<span data-ttu-id="056cc-140">Per eseguire chiamate a client specifici, usare le proprietà del `Clients` oggetto.</span><span class="sxs-lookup"><span data-stu-id="056cc-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="056cc-141">Nell'esempio seguente nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="056cc-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="056cc-142">Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominato `groups`.</span><span class="sxs-lookup"><span data-stu-id="056cc-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="056cc-143">Gestire gli eventi per una connessione</span><span class="sxs-lookup"><span data-stu-id="056cc-143">Handle events for a connection</span></span>

<span data-ttu-id="056cc-144">L'API degli hub SignalR fornisce le `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni.</span><span class="sxs-lookup"><span data-stu-id="056cc-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="056cc-145">Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'hub, ad esempio l'aggiunta a un gruppo.</span><span class="sxs-lookup"><span data-stu-id="056cc-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="056cc-146">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="056cc-146">Handle errors</span></span>

<span data-ttu-id="056cc-147">Le eccezioni generate nei metodi di hub vengono inviate al client che ha richiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="056cc-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="056cc-148">Nel client JavaScript, il `invoke` metodo restituisce un [suggerimento JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="056cc-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="056cc-149">Quando il client riceve un errore con un gestore associati all'oggetto promise mediante `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.</span><span class="sxs-lookup"><span data-stu-id="056cc-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="056cc-150">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="056cc-150">Related resources</span></span>

[<span data-ttu-id="056cc-151">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="056cc-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)