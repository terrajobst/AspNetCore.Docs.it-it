---
title: Uso di hub in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come usare hub in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 244ddc40e647bfcc3ca8cda2797c51bc49174822
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320147"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="73e1b-103">Usare hub di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73e1b-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="73e1b-104">Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="73e1b-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="73e1b-105">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="73e1b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="73e1b-106">Che cos'è un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="73e1b-106">What is a SignalR hub</span></span>

<span data-ttu-id="73e1b-107">L'API di hub SignalR consente di chiamare metodi sul client connessi dal server.</span><span class="sxs-lookup"><span data-stu-id="73e1b-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="73e1b-108">Nel codice del server, si definiscono i metodi chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="73e1b-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="73e1b-109">Nel codice client, si definiscono i metodi chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="73e1b-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="73e1b-110">SignalR si occupa di tutto dietro le quinte che rende possibili comunicazioni client-server e server a client in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="73e1b-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="73e1b-111">Configurare gli hub SignalR</span><span class="sxs-lookup"><span data-stu-id="73e1b-111">Configure SignalR hubs</span></span>

<span data-ttu-id="73e1b-112">Il middleware di SignalR richiede alcuni servizi, che vengono configurati mediante una chiamata `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="73e1b-113">Quando si aggiungono funzionalità SignalR a un'app ASP.NET Core, configurare le route di SignalR chiamando `app.UseSignalR` nella `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="73e1b-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="73e1b-114">Creare e usare gli hub</span><span class="sxs-lookup"><span data-stu-id="73e1b-114">Create and use hubs</span></span>

<span data-ttu-id="73e1b-115">Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungervi i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="73e1b-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="73e1b-116">I client possono chiamare i metodi che sono definiti come `public`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="73e1b-117">È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="73e1b-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="73e1b-118">SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="73e1b-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="73e1b-119">Gli hub sono temporanei:</span><span class="sxs-lookup"><span data-stu-id="73e1b-119">Hubs are transient:</span></span>
>
> * <span data-ttu-id="73e1b-120">Non archiviare lo stato in una proprietà nella classe dell'hub.</span><span class="sxs-lookup"><span data-stu-id="73e1b-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="73e1b-121">Ogni chiamata al metodo dell'hub viene eseguito in una nuova istanza di hub.</span><span class="sxs-lookup"><span data-stu-id="73e1b-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="73e1b-122">Usare `await` quando si chiamano i metodi asincroni che dipendono da hub di rimanere attivo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="73e1b-123">Ad esempio, un metodo, ad esempio `Clients.All.SendAsync(...)` può avere esito negativo se viene chiamato senza `await` e il metodo dell'hub viene completato prima `SendAsync` termina.</span><span class="sxs-lookup"><span data-stu-id="73e1b-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="73e1b-124">L'oggetto di contesto</span><span class="sxs-lookup"><span data-stu-id="73e1b-124">The Context object</span></span>

<span data-ttu-id="73e1b-125">Il `Hub` classe ha un `Context` proprietà che contiene le proprietà seguenti con le informazioni sulla connessione:</span><span class="sxs-lookup"><span data-stu-id="73e1b-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="73e1b-126">Proprietà</span><span class="sxs-lookup"><span data-stu-id="73e1b-126">Property</span></span> | <span data-ttu-id="73e1b-127">Descrizione</span><span class="sxs-lookup"><span data-stu-id="73e1b-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="73e1b-128">Ottiene l'ID univoco per la connessione, assegnata da SignalR.</span><span class="sxs-lookup"><span data-stu-id="73e1b-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="73e1b-129">È un ID di connessione per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="73e1b-130">Ottiene il [identificatore utente](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="73e1b-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="73e1b-131">Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente.</span><span class="sxs-lookup"><span data-stu-id="73e1b-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="73e1b-132">Ottiene il `ClaimsPrincipal` associati all'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="73e1b-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="73e1b-133">Ottiene una raccolta chiave/valore che può essere utilizzata per condividere i dati all'interno dell'ambito di questa connessione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="73e1b-134">I dati possono essere archiviati in questa raccolta e verrà mantenute per la connessione tra chiamate del metodo dell'hub diversi.</span><span class="sxs-lookup"><span data-stu-id="73e1b-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="73e1b-135">Ottiene la raccolta delle funzionalità disponibili nella connessione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="73e1b-136">Per il momento, questa raccolta non è necessaria nella maggior parte degli scenari, in modo che non è ancora documentato in dettaglio.</span><span class="sxs-lookup"><span data-stu-id="73e1b-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="73e1b-137">Ottiene un `CancellationToken` che invia una notifica quando la connessione viene interrotta.</span><span class="sxs-lookup"><span data-stu-id="73e1b-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="73e1b-138">`Hub.Context` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73e1b-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="73e1b-139">Metodo</span><span class="sxs-lookup"><span data-stu-id="73e1b-139">Method</span></span> | <span data-ttu-id="73e1b-140">Descrizione</span><span class="sxs-lookup"><span data-stu-id="73e1b-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="73e1b-141">Restituisce il `HttpContext` per la connessione, o `null` se la connessione non è associata a una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="73e1b-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="73e1b-142">Per le connessioni HTTP, è possibile utilizzare questo metodo per ottenere informazioni quali le intestazioni HTTP e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="73e1b-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="73e1b-143">Interrompe la connessione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="73e1b-144">L'oggetto client</span><span class="sxs-lookup"><span data-stu-id="73e1b-144">The Clients object</span></span>

<span data-ttu-id="73e1b-145">Il `Hub` classe ha un `Clients` proprietà che contiene le proprietà seguenti per la comunicazione tra client e server:</span><span class="sxs-lookup"><span data-stu-id="73e1b-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="73e1b-146">Proprietà</span><span class="sxs-lookup"><span data-stu-id="73e1b-146">Property</span></span> | <span data-ttu-id="73e1b-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="73e1b-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="73e1b-148">Chiama un metodo su tutti i client connessi</span><span class="sxs-lookup"><span data-stu-id="73e1b-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="73e1b-149">Chiama un metodo sul client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="73e1b-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="73e1b-150">Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo</span><span class="sxs-lookup"><span data-stu-id="73e1b-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="73e1b-151">`Hub.Clients` contiene anche i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73e1b-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="73e1b-152">Metodo</span><span class="sxs-lookup"><span data-stu-id="73e1b-152">Method</span></span> | <span data-ttu-id="73e1b-153">Descrizione</span><span class="sxs-lookup"><span data-stu-id="73e1b-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="73e1b-154">Chiama un metodo su tutti i client connessi, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="73e1b-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="73e1b-155">Chiama un metodo in un client connesso specifico</span><span class="sxs-lookup"><span data-stu-id="73e1b-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="73e1b-156">Chiama un metodo su specifici client connessi</span><span class="sxs-lookup"><span data-stu-id="73e1b-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="73e1b-157">Chiama un metodo su tutte le connessioni del gruppo specificato</span><span class="sxs-lookup"><span data-stu-id="73e1b-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="73e1b-158">Chiama un metodo su tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata</span><span class="sxs-lookup"><span data-stu-id="73e1b-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="73e1b-159">Chiama un metodo su più gruppi di connessioni</span><span class="sxs-lookup"><span data-stu-id="73e1b-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="73e1b-160">Chiama un metodo su un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="73e1b-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="73e1b-161">Chiama un metodo su tutte le connessioni associate a un utente specifico</span><span class="sxs-lookup"><span data-stu-id="73e1b-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="73e1b-162">Chiama un metodo su tutte le connessioni associate gli utenti specificati</span><span class="sxs-lookup"><span data-stu-id="73e1b-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="73e1b-163">Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="73e1b-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="73e1b-164">Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="73e1b-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="73e1b-165">Inviare messaggi ai client</span><span class="sxs-lookup"><span data-stu-id="73e1b-165">Send messages to clients</span></span>

<span data-ttu-id="73e1b-166">Per effettuare chiamate a client specifici, usare le proprietà del `Clients` oggetto.</span><span class="sxs-lookup"><span data-stu-id="73e1b-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="73e1b-167">Nell'esempio seguente, esistono tre metodi dell'Hub:</span><span class="sxs-lookup"><span data-stu-id="73e1b-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="73e1b-168">`SendMessage` Invia un messaggio a tutti i client connessi, uso `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="73e1b-169">`SendMessageToCaller` Invia un messaggio al chiamante, mediante `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="73e1b-170">`SendMessageToGroups` Invia un messaggio a tutti i client di `SignalR Users` gruppo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="73e1b-171">Hub fortemente tipizzato</span><span class="sxs-lookup"><span data-stu-id="73e1b-171">Strongly typed hubs</span></span>

<span data-ttu-id="73e1b-172">Uno svantaggio dell'uso `SendAsync` è che si basa su una stringa magic per specificare il metodo client da chiamare.</span><span class="sxs-lookup"><span data-stu-id="73e1b-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="73e1b-173">Questo lascia aperte di codice a errori di runtime se il nome del metodo è errato o mancante dal client.</span><span class="sxs-lookup"><span data-stu-id="73e1b-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="73e1b-174">Un'alternativa all'uso `SendAsync` è per tipizzare fortemente i `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="73e1b-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="73e1b-175">Nell'esempio seguente, il `ChatHub` metodi di client sono state estratte out in un'interfaccia denominata `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="73e1b-176">Questa interfaccia può essere utilizzata per effettuare il refactoring precedenti `ChatHub` esempio.</span><span class="sxs-lookup"><span data-stu-id="73e1b-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="73e1b-177">Usando `Hub<IChatClient>` permette di controllare i metodi del client in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="73e1b-178">In questo modo si evitano problemi causati dall'utilizzo di "stringhe magiche", poiché `Hub<T>` può solo fornire l'accesso ai metodi definiti nell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="73e1b-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="73e1b-179">Usando un oggetto fortemente tipizzato `Hub<T>` disabilita la possibilità di usare `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="73e1b-180">Qualsiasi metodo definito nell'interfaccia può ancora essere definiti asincrone.</span><span class="sxs-lookup"><span data-stu-id="73e1b-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="73e1b-181">In effetti, ognuno di questi metodi debba restituire un `Task`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="73e1b-182">Poiché si tratta di un'interfaccia, non usare il `async` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="73e1b-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="73e1b-183">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73e1b-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="73e1b-184">Il `Async` suffisso non viene rimosso dal nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="73e1b-185">A meno che non è definito il metodo di client con `.on('MyMethodAsync')`, è consigliabile non usare `MyMethodAsync` come nome.</span><span class="sxs-lookup"><span data-stu-id="73e1b-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="73e1b-186">Modificare il nome di un metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="73e1b-186">Change the name of a hub method</span></span>

<span data-ttu-id="73e1b-187">Per impostazione predefinita, un nome di metodo server hub è il nome del metodo .NET.</span><span class="sxs-lookup"><span data-stu-id="73e1b-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="73e1b-188">Tuttavia, è possibile usare la [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attributo per modificare il valore predefinito e specificare manualmente un nome per il metodo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="73e1b-189">Il client deve utilizzare tale nome, anziché il nome del metodo .NET, quando si richiama il metodo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="73e1b-190">Gestire gli eventi per una connessione</span><span class="sxs-lookup"><span data-stu-id="73e1b-190">Handle events for a connection</span></span>

<span data-ttu-id="73e1b-191">L'API di hub SignalR fornisce il `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni.</span><span class="sxs-lookup"><span data-stu-id="73e1b-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="73e1b-192">Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'Hub, ad esempio aggiunta a un gruppo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="73e1b-193">Eseguire l'override di `OnDisconnectedAsync` metodo virtuale per eseguire azioni quando un client si disconnette.</span><span class="sxs-lookup"><span data-stu-id="73e1b-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="73e1b-194">Se il client si disconnette intenzionalmente (chiamando `connection.stop()`, ad esempio), il `exception` parametro sarà `null`.</span><span class="sxs-lookup"><span data-stu-id="73e1b-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="73e1b-195">Tuttavia, se il client è stata disconnessa a causa di un errore (ad esempio un errore di rete), il `exception` parametro conterrà un'eccezione che descrive l'errore.</span><span class="sxs-lookup"><span data-stu-id="73e1b-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="73e1b-196">Gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="73e1b-196">Handle errors</span></span>

<span data-ttu-id="73e1b-197">Le eccezioni generate nei metodi dell'hub vengono inviate al client che ha richiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="73e1b-198">Nel client JavaScript, il `invoke` metodo restituisce un [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="73e1b-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="73e1b-199">Quando il client riceve un errore con un gestore eventi associati all'oggetto mediante promise `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.</span><span class="sxs-lookup"><span data-stu-id="73e1b-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="73e1b-200">Se l'Hub genera un'eccezione, le connessioni non vengono chiuse.</span><span class="sxs-lookup"><span data-stu-id="73e1b-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="73e1b-201">Per impostazione predefinita, SignalR restituisce un messaggio di errore generico nel client.</span><span class="sxs-lookup"><span data-stu-id="73e1b-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="73e1b-202">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73e1b-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="73e1b-203">Le eccezioni impreviste spesso contengono informazioni riservate, ad esempio il nome di un server di database in un'eccezione generato quando la connessione al database ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="73e1b-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="73e1b-204">SignalR non espone questi messaggi di errore dettagliato per impostazione predefinita come misura di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="73e1b-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="73e1b-205">Vedere le [articolo considerazioni sulla sicurezza](xref:signalr/security#exceptions) per altre informazioni sui motivi per cui vengono eliminati i dettagli dell'eccezione.</span><span class="sxs-lookup"><span data-stu-id="73e1b-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="73e1b-206">Se si dispone di un'eccezionale condizione si *scopo* desidera propagare al client, è possibile usare il `HubException` classe.</span><span class="sxs-lookup"><span data-stu-id="73e1b-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="73e1b-207">Se genera una `HubException` dal metodo dell'hub, SignalR **verranno** inviare l'intero messaggio al client, senza modificato.</span><span class="sxs-lookup"><span data-stu-id="73e1b-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="73e1b-208">SignalR Invia solo il `Message` proprietà dell'eccezione al client.</span><span class="sxs-lookup"><span data-stu-id="73e1b-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="73e1b-209">L'analisi dello stack e altre proprietà dell'eccezione non sono disponibili al client.</span><span class="sxs-lookup"><span data-stu-id="73e1b-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="73e1b-210">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="73e1b-210">Related resources</span></span>

* [<span data-ttu-id="73e1b-211">Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="73e1b-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="73e1b-212">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e1b-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="73e1b-213">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="73e1b-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
