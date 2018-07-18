---
title: Introduzione a ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come la libreria di ASP.NET Core SignalR semplifica l'aggiunta di funzionalità in tempo reale per le app.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095389"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introduzione a ASP.NET Core SignalR

[Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET Core SignalR è una libreria che semplifica l'aggiunta della funzionalità web in tempo reale per le app. Funzionalità web in tempo reale consente immediatamente il codice lato server push di contenuti ai client.

Buoni candidati per SignalR:

* App che richiedono aggiornamenti ad alta frequenza dal server. Esempi sono di gioco, social network, voto, delle aste, mappe e App GPS.
* I dashboard e App di monitoraggio. Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o avvisi di comunicazione.
* App di collaborazione. Le app di lavagna e riunione software del team sono esempi di App di collaborazione.
* App che richiedono le notifiche. Social network, messaggio di posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre App usare le notifiche.

SignalR fornisce un'API per la creazione di server a client [remote procedure call (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Le chiamate RPC chiamare le funzioni JavaScript nei client dal codice di .NET Core sul lato server.

SignalR per ASP.NET Core:

* Gestisce automaticamente la gestione della connessione.
* Abilita la trasmissione di messaggi per tutti i client connessi contemporaneamente. Ad esempio una chat room.
* Abilita l'invio di messaggi a client specifici o gruppi di client.
* È open source in [GitHub](https://github.com/aspnet/signalr).
* Scalabile.

La connessione tra il client e server è permanente, a differenza di una connessione HTTP.

## <a name="transports"></a>Trasporti

Riassunti di SignalR su una serie di tecniche per la compilazione di applicazioni web in tempo reale. [WebSockets](https://tools.ietf.org/html/rfc7118) è il trasporto ottima, ma è possibile utilizzare altre tecniche quali Server-Sent eventi e di Polling lungo quando questi non sono disponibili. SignalR verranno automaticamente rilevate e inizializzare il trasporto appropriato in base alle funzionalità supportate nel server e client.

## <a name="hubs"></a>Hub

SignalR Usa hub per la comunicazione tra client e server.

Un hub è una pipeline di alto livello che consente il client e server chiamare i metodi a vicenda. SignalR gestisce l'invio fra computer automaticamente, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa. Hub consentono il passaggio di parametri fortemente tipizzati per metodi, che consente l'associazione di modelli. SignalR fornisce due protocolli di hub predefinito: un protocollo di testo basati su JSON e un protocollo binario basato sul [MessagePack](https://msgpack.org/).  MessagePack crea in genere i messaggi più piccoli rispetto all'uso di JSON. Devono supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire supporto del protocollo MessagePack.

Hub di chiamare codice lato client mediante l'invio di messaggi mediante il trasporto attivo. I messaggi contengono il nome e parametri del metodo lato client. Gli oggetti inviati come parametri di metodo vengono deserializzati tramite il protocollo configurato. Il client cerca la corrispondenza con il nome a un metodo nel codice lato client. Quando viene trovata una corrispondenza, viene eseguito il metodo client usando i dati del parametro deserializzato.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione a SignalR per ASP.NET Core](xref:tutorials/signalr)
* [Piattaforme supportate](xref:signalr/supported-platforms)
* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
