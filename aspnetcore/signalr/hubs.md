---
title: Uso di hub in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare hub in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095281"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usare hub di SignalR per ASP.NET Core

Dal [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Che cos'è un hub SignalR

L'API di hub SignalR consente di chiamare metodi sul client connessi dal server. Nel codice del server, si definiscono i metodi chiamati dal client. Nel codice client, si definiscono i metodi chiamati dal server. SignalR si occupa di tutto dietro le quinte che rende possibili comunicazioni client-server e server a client in tempo reale.

## <a name="configure-signalr-hubs"></a>Configurare gli hub SignalR

Il middleware di SignalR richiede alcuni servizi, che vengono configurati mediante una chiamata `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quando si aggiungono funzionalità SignalR a un'app ASP.NET Core, configurare le route di SignalR chiamando `app.UseSignalR` nella `Startup.Configure` (metodo).

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Creare e usare gli hub

Creare un hub, dichiarazione di una classe che eredita da `Hub`e aggiungervi i metodi pubblici. I client possono chiamare i metodi che sono definiti come `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#. SignalR gestisce la serializzazione e deserializzazione di oggetti complessi e le matrici di parametri e valori restituiti.

## <a name="the-clients-object"></a>L'oggetto client

Ogni istanza di `Hub` classe ha una proprietà denominata `Clients` che contiene i membri seguenti per la comunicazione tra client e server:

| Proprietà | Descrizione |
| ------ | ----------- |
| `All` | Chiama un metodo su tutti i client connessi |
| `Caller` | Chiama un metodo sul client che ha richiamato il metodo dell'hub |
| `Others` | Chiama un metodo su tutti i client connessi eccetto il client che ha richiamato il metodo |


Inoltre, `Hub.Clients` contiene i metodi seguenti:

| Metodo | Descrizione |
| ------ | ----------- |
| `AllExcept` | Chiama un metodo su tutti i client connessi, ad eccezione di connessione specificata |
| `Client` | Chiama un metodo in un client connesso specifico |
| `Clients` | Chiama un metodo su specifici client connessi |
| `Group` | Chiama un metodo a tutte le connessioni del gruppo specificato  |
| `GroupExcept` | Chiama un metodo a tutte le connessioni del gruppo specificato, ad eccezione di connessione specificata |
| `Groups` | Chiama un metodo a più gruppi di connessioni  |
| `OthersInGroup` | Chiama un metodo a un gruppo di connessioni, esclusi i client che ha richiamato il metodo dell'hub  |
| `User` | Chiama un metodo per tutte le connessioni associate a un utente specifico |
| `Users` | Chiama un metodo per tutte le connessioni associate gli utenti specificati |

Ogni proprietà o metodo nelle tabelle precedenti restituisce un oggetto con un `SendAsync` (metodo). Il `SendAsync` metodo consente di specificare il nome e i parametri del metodo client da chiamare.

## <a name="send-messages-to-clients"></a>Inviare messaggi ai client

Per effettuare chiamate a client specifici, usare le proprietà del `Clients` oggetto. Nell'esempio seguente, il `SendMessageToCaller` metodo illustra l'invio di un messaggio per la connessione che ha richiamato il metodo dell'hub. Il `SendMessageToGroups` metodo invia un messaggio ai gruppi di cui è archiviati in un `List` denominata `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Gestire gli eventi per una connessione

L'API di hub SignalR fornisce il `OnConnectedAsync` e `OnDisconnectedAsync` metodi virtuali per gestire e tenere traccia delle connessioni. Eseguire l'override di `OnConnectedAsync` metodo virtuale per eseguire azioni quando un client si connette all'Hub, ad esempio aggiunta a un gruppo.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Gestione degli errori

Le eccezioni generate nei metodi dell'hub vengono inviate al client che ha richiamato il metodo. Nel client JavaScript, il `invoke` metodo restituisce un [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Quando il client riceve un errore con un gestore eventi associati all'oggetto mediante promise `catch`, ha richiamato e passato come un JavaScript `Error` oggetto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Risorse correlate

* [Introduzione ad ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
