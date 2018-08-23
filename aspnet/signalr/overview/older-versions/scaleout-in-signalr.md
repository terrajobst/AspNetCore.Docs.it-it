---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Introduzione alla scalabilità orizzontale in SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0cd1e64af031fea8078c8c1ca4c64b1e2e69d7e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837787"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Introduzione alla scalabilità orizzontale in SignalR 1.x
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In generale, esistono due modi per scalare un'applicazione web: *aumentare* e *scalare in orizzontale*.

- Scalare in verticale indica l'utilizzo di un server più grande (o una macchina virtuale più grande) con più di RAM, CPU e così via.
- La scalabilità orizzontale implica l'inserimento di più server per gestire il carico.

Il problema con la scalabilità verticale sia premuto rapidamente un limite per le dimensioni della macchina. Tuttavia, è necessario scalare orizzontalmente. Tuttavia, quando si scala orizzontalmente, i client possono ottenere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

![](scaleout-in-signalr/_static/image1.png)

Una soluzione consiste nell'inoltrare i messaggi tra server, usando un componente chiamato un' *backplane*. Con un backplane abilitato, ogni istanza dell'applicazione invia messaggi al backplane e li inoltra il backplane alle altre istanze dell'applicazione. (In elettronica, un backplane è un gruppo di connettori paralleli. Per analogia, backplane SignalR si connette più server.)

![](scaleout-in-signalr/_static/image2.png)

SignalR è attualmente forniscono i ripiani posteriori tre delle:

- **Azure Service Bus**. Service Bus è un'infrastruttura di messaggistica che consente ai componenti di inviare messaggi a regime.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello publish/subscribe ("pub/sub") per l'invio di messaggi.
- **SQL Server**. Il backplane di SQL Server scrive i messaggi delle tabelle SQL. Backplane utilizza Service Broker di messaggistica efficiente. Tuttavia funziona anche se Service Broker non è abilitato.

Se si distribuisce l'applicazione in Azure, è consigliabile usare il backplane del Bus di servizio di Azure. Se si distribuisce per la propria server farm, è consigliabile SQL Server o i ripiani posteriori delle Redis.

Gli argomenti seguenti contengono le esercitazioni dettagliate per ogni backplane:

- [Scalabilità orizzontale di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md)
- [Scalabilità orizzontale di SignalR con Redis](scaleout-with-redis.md)
- [Scalabilità orizzontale di SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementazione

In SignalR, ogni messaggio viene inviato tramite un bus di messaggi. Un bus di messaggi implementa il [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaccia, che fornisce un'astrazione di pubblicazione/sottoscrizione. Sostituendo il valore predefinito di lavoro dei ripiani posteriori delle **IMessageBus** con bus progettato per il backplane. È ad esempio il bus di messaggi per Redis [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), e Usa Redis [pub/sub](http://redis.io/topics/pubsub) meccanismo per inviare e ricevere messaggi.

Ogni istanza del server si connette al backplane tramite il bus. Quando viene inviato un messaggio, passa al backplane e backplane lo invia a ogni server. Quando un server riceve un messaggio da un backplane, inserisce il messaggio nella propria cache locale. Il server recapita quindi i messaggi al client dalla cache locale.

Per ogni connessione client, lo stato di avanzamento del client nella lettura del flusso di messaggi viene monitorata mediante un cursore. (Un cursore rappresenta una posizione nel flusso di messaggi). Se un client si disconnette e quindi si riconnette, viene richiesto il bus di eventuali messaggi ricevuti dopo il valore del cursore del client. Lo stesso avviene quando si usa una connessione [polling lungo](../getting-started/introduction-to-signalr.md#transports). Dopo il completamento di una richiesta di polling lungo, il client apre una nuova connessione e richiede di messaggi ricevuti dopo il cursore.

Il funzionamento del meccanismo cursore anche se un client viene indirizzato a un server diverso in riconnessione. Backplane è a conoscenza di tutti i server e non importa quale server si connette a un client.

## <a name="limitations"></a>Limitazioni

Usa un backplane, la velocità effettiva massima dei messaggi è inferiore rispetto a quando i client comunicano direttamente con un singolo nodo server. Ciò avviene perché il backplane inoltra tutti i messaggi per ogni nodo, in modo backplane può diventare un collo di bottiglia. Se questa limitazione è un problema dipende dall'applicazione. Ad esempio, ecco alcuni scenari tipici di SignalR:

- [Trasmissione del server](tutorial-server-broadcast-with-aspnet-signalr.md) (ad esempio, le quotazioni di borsa): i ripiani posteriori delle funziona bene per questo scenario, perché il server controlla la frequenza con cui vengono inviati i messaggi.
- [Da client a](tutorial-getting-started-with-signalr.md) (ad esempio, avvia una chat): In questo scenario, il backplane potrebbe essere un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client; vale a dire, se la frequenza dei messaggi aumenta in modo i client in modo proporzionale man mano join.
- [In tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) (ad esempio i giochi in tempo reale): un backplane non è consigliato per questo scenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Abilitazione della traccia per la scalabilità orizzontale di SignalR

Per abilitare la traccia per il ripiani posteriori delle, aggiungere le sezioni seguenti nel file Web. config, sotto la radice **configurazione** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
