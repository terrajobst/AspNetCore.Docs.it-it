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
ms.openlocfilehash: 495aa156dd5e4641d688d7b16df1e5814c9607f4
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819084"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usare gli hub di SignalR per ASP.NET Core

Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Che cos'è un hub SignalR

L'API degli hub SignalR consente di chiamare metodi sul client connessi dal server. Nel codice server, si definiscono i metodi che vengono chiamati dal client. Nel codice client, definire i metodi chiamati dal server. SignalR si occupa di qualsiasi elemento dietro le quinte che permette la comunicazione client-server e il server al client in tempo reale.

## <a name="configure-signalr-hubs"></a>Configurare gli hub di SignalR

Il middleware SignalR richiede alcuni servizi, sono configurate tramite la chiamata `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quando si aggiungono funzionalità SignalR a un'app di ASP.NET Core, del programma di installazione delle route SignalR chiamando `app.UseSignalR` nella `Startup.Configure` metodo.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Creare e usare gli hub

Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungere metodi pubblici. I client possono chiamare i metodi definiti come `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

È possibile specificare un tipo restituito e parametri, inclusi i tipi complessi e matrici, esattamente come si farebbe con qualsiasi metodo c#. SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.

## <a name="the-clients-object"></a>L'oggetto client

Ogni istanza di `Hub` classe ha una proprietà denominata `Clients` che contiene i membri seguenti per la comunicazione tra client e server:

| Proprietà | Descrizione |
| ------ | ----------- |
| `All` | Chiama un metodo su tutti i client connessi |
| `Caller` | Chiama un metodo nel client che ha richiamato il metodo dell'hub |
| `Others` | Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo |


Inoltre, `Hub.Clients` contiene i metodi seguenti:

| Metodo | Descrizione |
| ------ | ----------- |
| `AllExcept` | Chiama un metodo su tutti i client connessi ad eccezione delle connessioni specificate |
| `Client` | Chiama un metodo in un client connesso specifico |
| `Clients` | Chiama un metodo specifico client connessi |
| `Group` | Chiama un metodo a tutte le connessioni del gruppo specificato  |
| `GroupExcept` | Chiama un metodo a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata |
| `Groups` | Chiama un metodo a più gruppi di connessioni  |
| `OthersInGroup` | Chiama un metodo a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub  |
| `User` | Chiama un metodo a tutte le connessioni associate a un utente specifico |
| `Users` | Chiama un metodo a tutte le connessioni associati con gli utenti specificati |

Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` metodo. Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.

## <a name="send-messages-to-clients"></a>Inviare messaggi ai client

Per eseguire chiamate a client specifici, usare le proprietà del `Clients` oggetto. Nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub. Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominato `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Gestire gli eventi per una connessione

L'API degli hub SignalR fornisce le `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni. Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'hub, ad esempio l'aggiunta a un gruppo.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Gestione degli errori

Le eccezioni generate nei metodi di hub vengono inviate al client che ha richiamato il metodo. Nel client JavaScript, il `invoke` metodo restituisce un [suggerimento JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Quando il client riceve un errore con un gestore associati all'oggetto promise mediante `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Risorse correlate

* [Introduzione a ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
