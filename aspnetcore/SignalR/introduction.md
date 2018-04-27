---
title: Introduzione a ASP.NET Core SignalR
author: rachelappel
description: Informazioni su come la libreria di ASP.NET SignalR Core semplifica l'aggiunta di funzionalità web in tempo reale alle applicazioni.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: fa9b10201b5dc0e67bcd6d1321a3737e2025fda4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introduzione a ASP.NET Core SignalR

[Rachel Appel](https://twitter.com/rachelappel)


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a>Che cos'è SignalR?

Componenti di base di ASP.NET SignalR è una libreria che semplifica l'aggiunta di funzionalità per le app web in tempo reale. Funzionalità web in tempo reale consente immediatamente il codice lato server per il contenuto push ai client.

Buoni candidati per SignalR:

* Applicazioni che richiedono aggiornamenti ad alta frequenza dal server. Esempi sono giochi, social network, voto, asta, mappe e App GPS.
* I dashboard e le applicazioni di monitoraggio. Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o si spostano gli avvisi.
* Applicazioni di collaborazione. Le applicazioni di lavagna e riunione software del team sono esempi di applicazioni di collaborazione.
* Applicazioni che richiedono le notifiche. Social network, posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre app di usare le notifiche.

SignalR fornisce un'API per la creazione di server a client [chiamate di procedura remota (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Il RPC chiamare le funzioni JavaScript nei client dal codice .NET Core sul lato server.

SignalR per ASP.NET Core:

* Gestisce automaticamente la gestione della connessione.
* Consente la trasmissione di messaggi a tutti i client connessi contemporaneamente. Ad esempio una chat room.
* Abilita l'invio di messaggi al client specifici o gruppi di client.
* È open source in [GitHub](https://github.com/aspnet/signalr).
* Scalabile.

La connessione tra il client e server è persistente, a differenza di una connessione HTTP.

## <a name="transports"></a>Trasporti

Estrae SignalR su diverse tecniche per la compilazione di applicazioni web in tempo reale. [WebSocket](https://tools.ietf.org/html/rfc7118) è il trasporto ottimale, ma altre tecniche quali eventi Server-Sent e di Polling lungo possono essere usati quando quelli non sono disponibili. SignalR verranno automaticamente rilevate e inizializzare il trasporto appropriato in base alle funzionalità supportate nel server e client.

## <a name="hubs-and-endpoints"></a>Hub e endpoint

SignalR utilizza hub e gli endpoint per la comunicazione tra client e server. L'API di hub copre la maggior parte degli scenari.

Un hub è una pipeline di alto livello basata sull'Endpoint API che consente il client e server chiamare i metodi tra loro. SignalR gestisce la distribuzione tra i limiti della macchina automaticamente, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa. Gli hub di consentono il passaggio di parametri fortemente tipizzati per metodi, che consente l'associazione del modello. SignalR fornisce due protocolli di hub incorporato: un protocollo di testo in base a JSON e un protocollo binario in base a [MessagePack](https://msgpack.org/).  In genere, MessagePack crea messaggi più piccoli rispetto all'utilizzo di JSON. Deve supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire il supporto del protocollo MessagePack.

Gli hub di chiamare il codice sul lato client mediante l'invio di messaggi mediante il trasporto attivo. I messaggi contengono il nome e i parametri del metodo sul lato client. Gli oggetti inviati come parametri di metodo vengono deserializzati mediante il protocollo configurato. Il client tenta di corrispondere al nome a un metodo nel codice sul lato client. Quando si verifica una corrispondenza, viene eseguito il metodo di client utilizzando i dati del parametro deserializzato.

Gli endpoint forniscono un'API simile socket non elaborata, che consente di leggere e scrivere dal client. È attivo agli sviluppatori di gestire il raggruppamento, la trasmissione e altre funzioni. L'API di hub si basa sul livello di endpoint.

Il diagramma seguente mostra la relazione tra hub, endpoint e i client.

![Mappa di SignalR](introduction/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>Risorse correlate

[Introduzione a SignalR per ASP.NET Core](xref:signalr/get-started)
