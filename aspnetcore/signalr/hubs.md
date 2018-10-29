---
title: Uso di hub in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare hub in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: be42314afad4ff43d2fcf1abbc96c5b78c773977
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206016"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="8cbcf-103">Usare hub di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cbcf-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="8cbcf-104">Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="8cbcf-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="8cbcf-105">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="8cbcf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="8cbcf-106">Che cos'è un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="8cbcf-106">What is a SignalR hub</span></span>

<span data-ttu-id="8cbcf-107">L'API di hub SignalR consente di chiamare metodi sul client connessi dal server.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="8cbcf-108">Nel codice del server, si definiscono i metodi chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="8cbcf-109">Nel codice client, si definiscono i metodi chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="8cbcf-110">SignalR si occupa di tutto dietro le quinte che rende possibili comunicazioni client-server e server a client in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="8cbcf-111">Configurare gli hub SignalR</span><span class="sxs-lookup"><span data-stu-id="8cbcf-111">Configure SignalR hubs</span></span>

<span data-ttu-id="8cbcf-112">Il middleware di SignalR richiede alcuni servizi, che vengono configurati mediante una chiamata `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="8cbcf-113">Quando si aggiungono funzionalità SignalR a un'app ASP.NET Core, configurare le route di SignalR chiamando `app.UseSignalR` nella `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="8cbcf-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="8cbcf-114">Creare e usare gli hub</span><span class="sxs-lookup"><span data-stu-id="8cbcf-114">Create and use hubs</span></span>

<span data-ttu-id="8cbcf-115">Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungervi i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="8cbcf-116">I client possono chiamare i metodi che sono definiti come `public`.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="8cbcf-117">È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="8cbcf-118">SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="8cbcf-119">L'oggetto di contesto</span><span class="sxs-lookup"><span data-stu-id="8cbcf-119">The Context object</span></span>

<span data-ttu-id="8cbcf-120">Il `Hub` classe ha un `Context` proprietà che contiene le proprietà seguenti con le informazioni sulla connessione:</span><span class="sxs-lookup"><span data-stu-id="8cbcf-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="8cbcf-121">Proprietà</span><span class="sxs-lookup"><span data-stu-id="8cbcf-121">Property</span></span> | <span data-ttu-id="8cbcf-122">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8cbcf-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="8cbcf-123">Ottiene l'ID univoco per la connessione, assegnata da SignalR.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="8cbcf-124">È un ID di connessione per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="8cbcf-125">Ottiene il [identificatore utente](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="8cbcf-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="8cbcf-126">Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="8cbcf-127">Ottiene il `ClaimsPrincipal` associati all'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="8cbcf-128">Ottiene una raccolta chiave/valore che può essere utilizzata per condividere i dati all'interno dell'ambito di questa connessione.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="8cbcf-129">I dati possono essere archiviati in questa raccolta e verrà mantenute per la connessione tra chiamate del metodo dell'hub diversi.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="8cbcf-130">Ottiene la raccolta delle funzionalità disponibili nella connessione.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="8cbcf-131">Per il momento, questa raccolta non è necessaria nella maggior parte degli scenari, in modo che non è ancora documentato in dettaglio.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="8cbcf-132">Ottiene un `CancellationToken` che invia una notifica quando la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="8cbcf-133">`Hub.Context` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cbcf-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="8cbcf-134">Metodo</span><span class="sxs-lookup"><span data-stu-id="8cbcf-134">Method</span></span> | <span data-ttu-id="8cbcf-135">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8cbcf-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="8cbcf-136">Restituisce il `HttpContext` per la connessione, o `null` se la connessione non è associata a una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="8cbcf-137">Per le connessioni HTTP, è possibile utilizzare questo metodo per ottenere informazioni quali le intestazioni HTTP e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="8cbcf-138">Interrompe la connessione.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="8cbcf-139">L'oggetto client</span><span class="sxs-lookup"><span data-stu-id="8cbcf-139">The Clients object</span></span>

<span data-ttu-id="8cbcf-140">Il `Hub` classe ha un `Clients` proprietà che contiene le proprietà seguenti per la comunicazione tra client e server:</span><span class="sxs-lookup"><span data-stu-id="8cbcf-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="8cbcf-141">Proprietà</span><span class="sxs-lookup"><span data-stu-id="8cbcf-141">Property</span></span> | <span data-ttu-id="8cbcf-142">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8cbcf-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="8cbcf-143">Chiama un metodo su tutti i client connessi</span><span class="sxs-lookup"><span data-stu-id="8cbcf-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="8cbcf-144">Chiama un metodo sul client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="8cbcf-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="8cbcf-145">Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo</span><span class="sxs-lookup"><span data-stu-id="8cbcf-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="8cbcf-146">`Hub.Clients` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cbcf-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="8cbcf-147">Metodo</span><span class="sxs-lookup"><span data-stu-id="8cbcf-147">Method</span></span> | <span data-ttu-id="8cbcf-148">Descrizione</span><span class="sxs-lookup"><span data-stu-id="8cbcf-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="8cbcf-149">Chiama un metodo su tutti i client connessi, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="8cbcf-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="8cbcf-150">Chiama un metodo in un client connesso specifico</span><span class="sxs-lookup"><span data-stu-id="8cbcf-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="8cbcf-151">Chiama un metodo su specifici client connessi</span><span class="sxs-lookup"><span data-stu-id="8cbcf-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="8cbcf-152">Chiama un metodo a tutte le connessioni del gruppo specificato</span><span class="sxs-lookup"><span data-stu-id="8cbcf-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="8cbcf-153">Chiama un metodo a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="8cbcf-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="8cbcf-154">Chiama un metodo a più gruppi di connessioni</span><span class="sxs-lookup"><span data-stu-id="8cbcf-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="8cbcf-155">Chiama un metodo a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="8cbcf-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="8cbcf-156">Chiama un metodo per tutte le connessioni associate a un utente specifico</span><span class="sxs-lookup"><span data-stu-id="8cbcf-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="8cbcf-157">Chiama un metodo per tutte le connessioni associate gli utenti specificati</span><span class="sxs-lookup"><span data-stu-id="8cbcf-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="8cbcf-158">Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="8cbcf-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="8cbcf-159">Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="8cbcf-160">Inviare messaggi ai client</span><span class="sxs-lookup"><span data-stu-id="8cbcf-160">Send messages to clients</span></span>

<span data-ttu-id="8cbcf-161">Per effettuare chiamate a client specifici, usare le proprietà del `Clients` oggetto.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="8cbcf-162">Nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="8cbcf-163">Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominata `groups`.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="8cbcf-164">Hub fortemente tipizzato</span><span class="sxs-lookup"><span data-stu-id="8cbcf-164">Strongly typed hubs</span></span>

<span data-ttu-id="8cbcf-165">Uno svantaggio dell'uso `SendAsync` è che si basa su una stringa magic per specificare il metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="8cbcf-166">Questo lascia aperte di codice a errori di runtime se il nome del metodo è errato o mancante dal client.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="8cbcf-167">Un'alternativa all'uso `SendAsync` è per tipizzare fortemente i `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="8cbcf-168">Nell'esempio seguente, il `ChatHub` metodi di client sono state estratte out in un'interfaccia denominata `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="8cbcf-169">Questa interfaccia può essere utilizzata per effettuare il refactoring precedenti `ChatHub` esempio.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="8cbcf-170">Usando `Hub<IChatClient>` permette di controllare i metodi del client in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="8cbcf-171">In questo modo si evitano problemi causati dall'utilizzo di "stringhe magiche", poiché `Hub<T>` può solo fornire l'accesso ai metodi definiti nell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="8cbcf-172">Usando un oggetto fortemente tipizzato `Hub<T>` disabilita la possibilità di usare `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="8cbcf-173">Gestire gli eventi per una connessione</span><span class="sxs-lookup"><span data-stu-id="8cbcf-173">Handle events for a connection</span></span>

<span data-ttu-id="8cbcf-174">L'API di hub SignalR fornisce il `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="8cbcf-175">Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'Hub, ad esempio aggiunta a un gruppo.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="8cbcf-176">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="8cbcf-176">Handle errors</span></span>

<span data-ttu-id="8cbcf-177">Le eccezioni generate nei metodi dell'hub vengono inviate al client che ha richiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="8cbcf-178">Nel client JavaScript, il `invoke` metodo restituisce un [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="8cbcf-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="8cbcf-179">Quando il client riceve un errore con un gestore eventi associati all'oggetto mediante promise `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.</span><span class="sxs-lookup"><span data-stu-id="8cbcf-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="8cbcf-180">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="8cbcf-180">Related resources</span></span>

* [<span data-ttu-id="8cbcf-181">Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8cbcf-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="8cbcf-182">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8cbcf-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8cbcf-183">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="8cbcf-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
