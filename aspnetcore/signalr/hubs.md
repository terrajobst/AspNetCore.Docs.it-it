---
title: Uso di hub in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come usare hub in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: eb87aab2b7f3a58c6cec80f48f7616749f0809e2
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897078"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usare hub di SignalR per ASP.NET Core

Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Che cos'è un hub SignalR

L'API di hub SignalR consente di chiamare metodi sul client connessi dal server. Nel codice del server, si definiscono i metodi chiamati dal client. Nel codice client, si definiscono i metodi chiamati dal server. SignalR si occupa di tutto dietro le quinte che rende possibili comunicazioni client-server e server a client in tempo reale.

## <a name="configure-signalr-hubs"></a>Configurare gli hub SignalR

Il middleware di SignalR richiede alcuni servizi, che vengono configurati mediante una chiamata `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quando si aggiungono funzionalità SignalR a un'app ASP.NET Core, configurare le route di SignalR chiamando `app.UseSignalR` nella `Startup.Configure` (metodo).

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Creare e usare gli hub

Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungervi i metodi pubblici. I client possono chiamare i metodi che sono definiti come `public`.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#. SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.

> [!NOTE]
> Gli hub sono temporanei:
>
> * Non archiviare lo stato in una proprietà nella classe dell'hub. Ogni chiamata al metodo dell'hub viene eseguito in una nuova istanza di hub.  
> * Usare `await` quando si chiamano i metodi asincroni che dipendono da hub di rimanere attivo. Ad esempio, un metodo, ad esempio `Clients.All.SendAsync(...)` può avere esito negativo se viene chiamato senza `await` e il metodo dell'hub viene completato prima `SendAsync` termina.

## <a name="the-context-object"></a>L'oggetto di contesto

Il `Hub` classe ha un `Context` proprietà che contiene le proprietà seguenti con le informazioni sulla connessione:

| Proprietà | Descrizione |
| ------ | ----------- |
| `ConnectionId` | Ottiene l'ID univoco per la connessione, assegnata da SignalR. È un ID di connessione per ogni connessione.|
| `UserIdentifier` | Ottiene il [identificatore utente](xref:signalr/groups). Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente. |
| `User` | Ottiene il `ClaimsPrincipal` associati all'utente corrente. |
| `Items` | Ottiene una raccolta chiave/valore che può essere utilizzata per condividere i dati all'interno dell'ambito di questa connessione. I dati possono essere archiviati in questa raccolta e verrà mantenute per la connessione tra chiamate del metodo dell'hub diversi. |
| `Features` | Ottiene la raccolta delle funzionalità disponibili nella connessione. Per il momento, questa raccolta non è necessaria nella maggior parte degli scenari, in modo che non è ancora documentato in dettaglio. |
| `ConnectionAborted` | Ottiene un `CancellationToken` che invia una notifica quando la connessione viene interrotta. |

`Hub.Context` contiene anche i metodi seguenti:

| Metodo | Descrizione |
| ------ | ----------- |
| `GetHttpContext` | Restituisce il `HttpContext` per la connessione, o `null` se la connessione non è associata a una richiesta HTTP. Per le connessioni HTTP, è possibile utilizzare questo metodo per ottenere informazioni quali le intestazioni HTTP e le stringhe di query. |
| `Abort` | Interrompe la connessione. |

## <a name="the-clients-object"></a>L'oggetto client

Il `Hub` classe ha un `Clients` proprietà che contiene le proprietà seguenti per la comunicazione tra client e server:

| Proprietà | Descrizione |
| ------ | ----------- |
| `All` | Chiama un metodo su tutti i client connessi |
| `Caller` | Chiama un metodo sul client che ha richiamato il metodo dell'hub |
| `Others` | Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo |

`Hub.Clients` contiene anche i metodi seguenti:

| Metodo | Descrizione |
| ------ | ----------- |
| `AllExcept` | Chiama un metodo su tutti i client connessi, ad eccezione di connessione specificata |
| `Client` | Chiama un metodo in un client connesso specifico |
| `Clients` | Chiama un metodo su specifici client connessi |
| `Group` | Chiama un metodo su tutte le connessioni del gruppo specificato  |
| `GroupExcept` | Chiama un metodo su tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata |
| `Groups` | Chiama un metodo su più gruppi di connessioni  |
| `OthersInGroup` | Chiama un metodo su un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub  |
| `User` | Chiama un metodo su tutte le connessioni associate a un utente specifico |
| `Users` | Chiama un metodo su tutte le connessioni associate gli utenti specificati |

Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` (metodo). Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.

## <a name="send-messages-to-clients"></a>Inviare messaggi ai client

Per effettuare chiamate a client specifici, usare le proprietà del `Clients` oggetto. Nell'esempio seguente, esistono tre metodi dell'Hub:

* `SendMessage` Invia un messaggio a tutti i client connessi, uso `Clients.All`.
* `SendMessageToCaller` Invia un messaggio al chiamante, mediante `Clients.Caller`.
* `SendMessageToGroups` Invia un messaggio a tutti i client di `SignalR Users` gruppo.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Hub fortemente tipizzato

Uno svantaggio dell'uso `SendAsync` è che si basa su una stringa magic per specificare il metodo client da chiamare. Questo lascia aperte di codice a errori di runtime se il nome del metodo è errato o mancante dal client.

Un'alternativa all'uso `SendAsync` è per tipizzare fortemente i `Hub` con <xref:Microsoft.AspNetCore.SignalR.Hub%601>. Nell'esempio seguente, il `ChatHub` metodi di client sono state estratte out in un'interfaccia denominata `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Questa interfaccia può essere utilizzata per effettuare il refactoring precedenti `ChatHub` esempio.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

Usando `Hub<IChatClient>` permette di controllare i metodi del client in fase di compilazione. In questo modo si evitano problemi causati dall'utilizzo di "stringhe magiche", poiché `Hub<T>` può solo fornire l'accesso ai metodi definiti nell'interfaccia.

Usando un oggetto fortemente tipizzato `Hub<T>` disabilita la possibilità di usare `SendAsync`. Qualsiasi metodo definito nell'interfaccia può ancora essere definiti asincrone. In effetti, ognuno di questi metodi debba restituire un `Task`. Poiché si tratta di un'interfaccia, non usare il `async` (parola chiave). Ad esempio:

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> Il `Async` suffisso non viene rimosso dal nome del metodo. A meno che non è definito il metodo di client con `.on('MyMethodAsync')`, è consigliabile non usare `MyMethodAsync` come nome.

## <a name="change-the-name-of-a-hub-method"></a>Modificare il nome di un metodo dell'hub

Per impostazione predefinita, un nome di metodo server hub è il nome del metodo .NET. Tuttavia, è possibile usare la [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attributo per modificare il valore predefinito e specificare manualmente un nome per il metodo. Il client deve utilizzare tale nome, anziché il nome del metodo .NET, quando si richiama il metodo.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Gestire gli eventi per una connessione

L'API di hub SignalR fornisce il `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni. Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'Hub, ad esempio aggiunta a un gruppo.

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Eseguire l'override di `OnDisconnectedAsync` metodo virtuale per eseguire azioni quando un client si disconnette. Se il client si disconnette intenzionalmente (chiamando `connection.stop()`, ad esempio), il `exception` parametro sarà `null`. Tuttavia, se il client è stata disconnessa a causa di un errore (ad esempio un errore di rete), il `exception` parametro conterrà un'eccezione che descrive l'errore.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>Gestire gli errori

Le eccezioni generate nei metodi dell'hub vengono inviate al client che ha richiamato il metodo. Nel client JavaScript, il `invoke` metodo restituisce un [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Quando il client riceve un errore con un gestore eventi associati all'oggetto mediante promise `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Se l'Hub genera un'eccezione, le connessioni non vengono chiuse. Per impostazione predefinita, SignalR restituisce un messaggio di errore generico nel client. Ad esempio:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Le eccezioni impreviste spesso contengono informazioni riservate, ad esempio il nome di un server di database in un'eccezione generato quando la connessione al database ha esito negativo. SignalR non espone questi messaggi di errore dettagliato per impostazione predefinita come misura di sicurezza. Vedere le [articolo considerazioni sulla sicurezza](xref:signalr/security#exceptions) per altre informazioni sui motivi per cui vengono eliminati i dettagli dell'eccezione.

Se si dispone di un'eccezionale condizione si *scopo* desidera propagare al client, è possibile usare il `HubException` classe. Se genera una `HubException` dal metodo dell'hub, SignalR **verranno** inviare l'intero messaggio al client, senza modificato.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR Invia solo il `Message` proprietà dell'eccezione al client. L'analisi dello stack e altre proprietà dell'eccezione non sono disponibili al client.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione ad ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
