---
title: Uso di hub in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare hub in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 27aedc5b2f2060d961070fbd1ff5304eaa3956d1
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225356"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="48a5e-103">Usare hub di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48a5e-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="48a5e-104">Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="48a5e-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="48a5e-105">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="48a5e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="48a5e-106">Che cos'è un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="48a5e-106">What is a SignalR hub</span></span>

<span data-ttu-id="48a5e-107">L'API di hub SignalR consente di chiamare metodi sul client connessi dal server.</span><span class="sxs-lookup"><span data-stu-id="48a5e-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="48a5e-108">Nel codice del server, si definiscono i metodi chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="48a5e-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="48a5e-109">Nel codice client, si definiscono i metodi chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="48a5e-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="48a5e-110">SignalR si occupa di tutto dietro le quinte che rende possibili comunicazioni client-server e server a client in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="48a5e-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="48a5e-111">Configurare gli hub SignalR</span><span class="sxs-lookup"><span data-stu-id="48a5e-111">Configure SignalR hubs</span></span>

<span data-ttu-id="48a5e-112">Il middleware di SignalR richiede alcuni servizi, che vengono configurati mediante una chiamata `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="48a5e-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="48a5e-113">Quando si aggiungono funzionalità SignalR a un'app ASP.NET Core, configurare le route di SignalR chiamando `app.UseSignalR` nella `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="48a5e-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="48a5e-114">Creare e usare gli hub</span><span class="sxs-lookup"><span data-stu-id="48a5e-114">Create and use hubs</span></span>

<span data-ttu-id="48a5e-115">Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungervi i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="48a5e-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="48a5e-116">I client possono chiamare i metodi che sono definiti come `public`.</span><span class="sxs-lookup"><span data-stu-id="48a5e-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="48a5e-117">È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="48a5e-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="48a5e-118">SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="48a5e-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="48a5e-119">Gli hub sono temporanei:</span><span class="sxs-lookup"><span data-stu-id="48a5e-119">Hubs are transient:</span></span>
> * <span data-ttu-id="48a5e-120">Non archiviare lo stato in una proprietà nella classe dell'hub.</span><span class="sxs-lookup"><span data-stu-id="48a5e-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="48a5e-121">Ogni chiamata al metodo dell'hub viene eseguito in una nuova istanza di hub.</span><span class="sxs-lookup"><span data-stu-id="48a5e-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="48a5e-122">Usare `await` quando si chiamano i metodi asincroni che dipendono da hub di rimanere attivo.</span><span class="sxs-lookup"><span data-stu-id="48a5e-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="48a5e-123">Ad esempio, un metodo, ad esempio `Clients.All.SendAsync(...)` può avere esito negativo se viene chiamato senza `await` e il metodo dell'hub viene completato prima `SendAsync` termina.</span><span class="sxs-lookup"><span data-stu-id="48a5e-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="48a5e-124">L'oggetto di contesto</span><span class="sxs-lookup"><span data-stu-id="48a5e-124">The Context object</span></span>

<span data-ttu-id="48a5e-125">Il `Hub` classe ha un `Context` proprietà che contiene le proprietà seguenti con le informazioni sulla connessione:</span><span class="sxs-lookup"><span data-stu-id="48a5e-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="48a5e-126">Proprietà</span><span class="sxs-lookup"><span data-stu-id="48a5e-126">Property</span></span> | <span data-ttu-id="48a5e-127">Descrizione</span><span class="sxs-lookup"><span data-stu-id="48a5e-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="48a5e-128">Ottiene l'ID univoco per la connessione, assegnata da SignalR.</span><span class="sxs-lookup"><span data-stu-id="48a5e-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="48a5e-129">È un ID di connessione per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="48a5e-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="48a5e-130">Ottiene il [identificatore utente](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="48a5e-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="48a5e-131">Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente.</span><span class="sxs-lookup"><span data-stu-id="48a5e-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="48a5e-132">Ottiene il `ClaimsPrincipal` associati all'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="48a5e-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="48a5e-133">Ottiene una raccolta chiave/valore che può essere utilizzata per condividere i dati all'interno dell'ambito di questa connessione.</span><span class="sxs-lookup"><span data-stu-id="48a5e-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="48a5e-134">I dati possono essere archiviati in questa raccolta e verrà mantenute per la connessione tra chiamate del metodo dell'hub diversi.</span><span class="sxs-lookup"><span data-stu-id="48a5e-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="48a5e-135">Ottiene la raccolta delle funzionalità disponibili nella connessione.</span><span class="sxs-lookup"><span data-stu-id="48a5e-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="48a5e-136">Per il momento, questa raccolta non è necessaria nella maggior parte degli scenari, in modo che non è ancora documentato in dettaglio.</span><span class="sxs-lookup"><span data-stu-id="48a5e-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="48a5e-137">Ottiene un `CancellationToken` che invia una notifica quando la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="48a5e-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="48a5e-138">`Hub.Context` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="48a5e-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="48a5e-139">Metodo</span><span class="sxs-lookup"><span data-stu-id="48a5e-139">Method</span></span> | <span data-ttu-id="48a5e-140">Descrizione</span><span class="sxs-lookup"><span data-stu-id="48a5e-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="48a5e-141">Restituisce il `HttpContext` per la connessione, o `null` se la connessione non è associata a una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="48a5e-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="48a5e-142">Per le connessioni HTTP, è possibile utilizzare questo metodo per ottenere informazioni quali le intestazioni HTTP e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="48a5e-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="48a5e-143">Interrompe la connessione.</span><span class="sxs-lookup"><span data-stu-id="48a5e-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="48a5e-144">L'oggetto client</span><span class="sxs-lookup"><span data-stu-id="48a5e-144">The Clients object</span></span>

<span data-ttu-id="48a5e-145">Il `Hub` classe ha un `Clients` proprietà che contiene le proprietà seguenti per la comunicazione tra client e server:</span><span class="sxs-lookup"><span data-stu-id="48a5e-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="48a5e-146">Proprietà</span><span class="sxs-lookup"><span data-stu-id="48a5e-146">Property</span></span> | <span data-ttu-id="48a5e-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="48a5e-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="48a5e-148">Chiama un metodo su tutti i client connessi</span><span class="sxs-lookup"><span data-stu-id="48a5e-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="48a5e-149">Chiama un metodo sul client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="48a5e-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="48a5e-150">Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo</span><span class="sxs-lookup"><span data-stu-id="48a5e-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="48a5e-151">`Hub.Clients` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="48a5e-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="48a5e-152">Metodo</span><span class="sxs-lookup"><span data-stu-id="48a5e-152">Method</span></span> | <span data-ttu-id="48a5e-153">Descrizione</span><span class="sxs-lookup"><span data-stu-id="48a5e-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="48a5e-154">Chiama un metodo su tutti i client connessi, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="48a5e-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="48a5e-155">Chiama un metodo in un client connesso specifico</span><span class="sxs-lookup"><span data-stu-id="48a5e-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="48a5e-156">Chiama un metodo su specifici client connessi</span><span class="sxs-lookup"><span data-stu-id="48a5e-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="48a5e-157">Chiama un metodo a tutte le connessioni del gruppo specificato</span><span class="sxs-lookup"><span data-stu-id="48a5e-157">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="48a5e-158">Chiama un metodo a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="48a5e-158">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="48a5e-159">Chiama un metodo a più gruppi di connessioni</span><span class="sxs-lookup"><span data-stu-id="48a5e-159">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="48a5e-160">Chiama un metodo a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="48a5e-160">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="48a5e-161">Chiama un metodo per tutte le connessioni associate a un utente specifico</span><span class="sxs-lookup"><span data-stu-id="48a5e-161">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="48a5e-162">Chiama un metodo per tutte le connessioni associate gli utenti specificati</span><span class="sxs-lookup"><span data-stu-id="48a5e-162">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="48a5e-163">Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="48a5e-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="48a5e-164">Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="48a5e-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="48a5e-165">Inviare messaggi ai client</span><span class="sxs-lookup"><span data-stu-id="48a5e-165">Send messages to clients</span></span>

<span data-ttu-id="48a5e-166">Per effettuare chiamate a client specifici, usare le proprietà del `Clients` oggetto.</span><span class="sxs-lookup"><span data-stu-id="48a5e-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="48a5e-167">Nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="48a5e-167">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="48a5e-168">Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominata `groups`.</span><span class="sxs-lookup"><span data-stu-id="48a5e-168">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="48a5e-169">Hub fortemente tipizzato</span><span class="sxs-lookup"><span data-stu-id="48a5e-169">Strongly typed hubs</span></span>

<span data-ttu-id="48a5e-170">Uno svantaggio dell'uso `SendAsync` è che si basa su una stringa magic per specificare il metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="48a5e-170">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="48a5e-171">Questo lascia aperte di codice a errori di runtime se il nome del metodo è errato o mancante dal client.</span><span class="sxs-lookup"><span data-stu-id="48a5e-171">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="48a5e-172">Un'alternativa all'uso `SendAsync` è per tipizzare fortemente i `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="48a5e-172">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="48a5e-173">Nell'esempio seguente, il `ChatHub` metodi di client sono state estratte out in un'interfaccia denominata `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="48a5e-173">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="48a5e-174">Questa interfaccia può essere utilizzata per effettuare il refactoring precedenti `ChatHub` esempio.</span><span class="sxs-lookup"><span data-stu-id="48a5e-174">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="48a5e-175">Usando `Hub<IChatClient>` permette di controllare i metodi del client in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="48a5e-175">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="48a5e-176">In questo modo si evitano problemi causati dall'utilizzo di "stringhe magiche", poiché `Hub<T>` può solo fornire l'accesso ai metodi definiti nell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="48a5e-176">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="48a5e-177">Usando un oggetto fortemente tipizzato `Hub<T>` disabilita la possibilità di usare `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="48a5e-177">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="48a5e-178">Gestire gli eventi per una connessione</span><span class="sxs-lookup"><span data-stu-id="48a5e-178">Handle events for a connection</span></span>

<span data-ttu-id="48a5e-179">L'API di hub SignalR fornisce il `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni.</span><span class="sxs-lookup"><span data-stu-id="48a5e-179">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="48a5e-180">Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'Hub, ad esempio aggiunta a un gruppo.</span><span class="sxs-lookup"><span data-stu-id="48a5e-180">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="48a5e-181">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="48a5e-181">Handle errors</span></span>

<span data-ttu-id="48a5e-182">Le eccezioni generate nei metodi dell'hub vengono inviate al client che ha richiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="48a5e-182">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="48a5e-183">Nel client JavaScript, il `invoke` metodo restituisce un [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="48a5e-183">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="48a5e-184">Quando il client riceve un errore con un gestore eventi associati all'oggetto mediante promise `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.</span><span class="sxs-lookup"><span data-stu-id="48a5e-184">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="48a5e-185">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="48a5e-185">Related resources</span></span>

* [<span data-ttu-id="48a5e-186">Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="48a5e-186">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="48a5e-187">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="48a5e-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="48a5e-188">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="48a5e-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
