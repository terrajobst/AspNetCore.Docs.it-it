---
title: Introduzione a ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come la libreria di ASP.NET Core SignalR semplifica l'aggiunta di funzionalità in tempo reale per le app.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: da18837c690d2182589db5f486ae74e537ade931
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425146"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introduzione a ASP.NET Core SignalR

## <a name="what-is-signalr"></a>Che cos'è SignalR?

ASP.NET Core SignalR è una libreria open source che semplifica l'aggiunta della funzionalità web in tempo reale per le app. Funzionalità web in tempo reale consente immediatamente il codice lato server push di contenuti ai client.

Buoni candidati per SignalR:

* App che richiedono aggiornamenti ad alta frequenza dal server. Esempi sono di gioco, social network, voto, delle aste, mappe e App GPS.
* I dashboard e App di monitoraggio. Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o avvisi di comunicazione.
* App di collaborazione. Le app di lavagna e riunione software del team sono esempi di App di collaborazione.
* App che richiedono le notifiche. Social network, messaggio di posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre App usare le notifiche.

SignalR fornisce un'API per la creazione di server a client [remote procedure call (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Le chiamate RPC chiamare le funzioni JavaScript nei client dal codice di .NET Core sul lato server.

Ecco alcune funzionalità di SignalR per ASP.NET Core:

* Gestisce automaticamente la gestione della connessione.
* Invia messaggi a tutti i client connessi contemporaneamente. Ad esempio una chat room.
* Invia messaggi al client specifici o gruppi di client.
* Scalabilità sufficiente a gestire il crescente traffico.

L'origine è ospitato in un [SignalR repository in GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Trasporti

SignalR supporta diverse tecniche per la gestione delle comunicazioni in tempo reale:

* [Oggetti WebSocket](https://tools.ietf.org/html/rfc7118)
* Eventi inviati al server
* Polling di lunga durata

SignalR sceglie automaticamente il migliore metodo di trasporto compreso le funzionalità del server e client.

## <a name="hubs"></a>Hub

Usa SignalR *hub* per la comunicazione tra client e server.

Un hub è una pipeline di alto livello che consente a un client e server chiamare i metodi a vicenda. SignalR gestisce l'invio fra computer automaticamente, consentendo ai client di chiamare metodi sul server e viceversa. È possibile passare parametri fortemente tipizzati per metodi, che consente l'associazione di modelli. SignalR fornisce due protocolli di hub predefinito: un protocollo di testo basati su JSON e un protocollo binario basato sul [MessagePack](https://msgpack.org/).  MessagePack crea in genere i messaggi più piccoli rispetto a JSON. Devono supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire supporto del protocollo MessagePack.

Hub di chiamare codice lato client mediante l'invio di messaggi che contengono il nome e parametri del metodo lato client. Gli oggetti inviati come parametri di metodo vengono deserializzati tramite il protocollo configurato. Il client cerca la corrispondenza con il nome a un metodo nel codice lato client. Quando il client trova una corrispondenza, chiama il metodo e passa i dati del parametro deserializzato.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione a SignalR per ASP.NET Core](xref:tutorials/signalr)
* [Piattaforme supportate](xref:signalr/supported-platforms)
* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
