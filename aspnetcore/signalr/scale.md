---
title: Hosting di ASP.NET Core SignalR produzione e la scalabilità
author: bradygaster
description: Informazioni su come evitare le prestazioni e scalabilità problemi nelle App che usano ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836922"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>ASP.NET Core SignalR hosting e scalabilità

Dal [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), e [Tom Dykstra](https://github.com/tdykstra),

Questo articolo illustra l'hosting e scalabilità considerazioni per le app a traffico elevato che usano ASP.NET Core SignalR.

## <a name="tcp-connection-resources"></a>Risorse di connessione TCP

Il numero di connessioni TCP simultanee che possa supportare un server web è limitato. I client HTTP standard usano *temporanee* connessioni. Queste connessioni possono essere chiusa quando il client diventa inattivo e riaperti in un secondo momento. D'altra parte, è una connessione SignalR *permanente*. Le connessioni SignalR rimangono aperti anche quando il client diventa inattivo. In un'app a traffico elevato che gestisce molti client, queste connessioni permanenti possono causare i server da sottoporre a hit il numero massimo di connessioni.

Le connessioni persistenti anche utilizzano ulteriore memoria, per tenere traccia di ogni connessione.

L'uso eccessivo delle risorse correlate alla connessione da SignalR può influire sulle altre App web ospitate nello stesso server. Quando SignalR apre e mantiene l'ultimo connessioni TCP disponibili, altre App web nello stesso server non dispone anche più disponibili a tali connessioni.

Se un server esaurisce le connessioni, si noterà che gli errori di socket casuale e connessione Reimposta gli errori. Ad esempio:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Per evitare l'utilizzo delle risorse di SignalR causando errori in altre App web, eseguire SignalR su server diversi da altre App web.

Per evitare l'utilizzo delle risorse di SignalR causando errori in un'app di SignalR, per limitare il numero di connessioni di che deve gestire un server scalabilità orizzontale.

## <a name="scale-out"></a>Scalare in orizzontale

Un'app che usa SignalR deve tenere traccia di tutte le connessioni, che crea problemi per una server farm. Aggiungere un server e ottiene nuove connessioni che non conoscono gli altri server. Ad esempio, non è a conoscenza delle connessioni in altri server di SignalR in ogni server nel diagramma seguente. Quando si vuole inviare un messaggio a tutti i client SignalR su uno dei server, il messaggio viene inserito solo i client connessi a tale server.

![La scalabilità senza un backplane SignalR](scale/_static/scale-no-backplane.png)

Le opzioni per risolvere questo problema sono le [servizio Azure SignalR](#azure-signalr-service) e [Redis backplane](#redis-backplane).

## <a name="azure-signalr-service"></a>Azure SignalR Service

Il servizio Azure SignalR è un proxy anziché un backplane. Ogni volta che un client avvia una connessione al server, viene reindirizzato il client per connettersi al servizio. Tale processo è illustrato nel diagramma seguente:

![Stabilire una connessione al servizio Azure SignalR](scale/_static/azure-signalr-service-one-connection.png)

Il risultato è che il servizio gestisce tutte le connessioni client, mentre ogni server necessita solo un piccolo numero costante di connessioni al servizio, come illustrato nel diagramma seguente:

![Client connessi al servizio, i server connessi al servizio](scale/_static/azure-signalr-service-multiple-connections.png)

Questo approccio per la scalabilità presenta diversi vantaggi rispetto l'alternativa di backplane Redis:

* Le sessioni permanenti, note anche come [affinità del client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), non è necessaria perché i client vengono immediatamente reindirizzati al servizio Azure SignalR al momento della connessione.
* Un'app consente la scalabilità orizzontale di SignalR in base al numero di messaggi inviati, mentre il servizio Azure SignalR ridimensiona automaticamente per gestire un numero qualsiasi di connessioni. Ad esempio, potrebbe essere presenti migliaia di client, ma se solo alcuni messaggi al secondo inviati, l'app SignalR non sarà necessario scalare orizzontalmente su più server per gestire le connessioni a se stessi.
* Un'app di SignalR non verrà usate molte più risorse di connessione di un'app web senza SignalR.

Per questi motivi, è consigliabile il servizio Azure SignalR per tutte le app ASP.NET Core SignalR ospitate in Azure, tra cui servizio App, le macchine virtuali e contenitori.

Per altre informazioni vedere la [documentazione del servizio Azure SignalR](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Redis backplane

[Redis](https://redis.io/) è un archivio chiave-valore in memoria che supporta un sistema di messaggistica con un modello publish/subscribe. Backplane SignalR Redis Usa la funzionalità di pubblicazione/sottoscrizione per inoltrare i messaggi ad altri server. Quando un client stabilisce una connessione, le informazioni di connessione viene passate al backplane. Quando un server vuole inviare un messaggio a tutti i client, invia al backplane. Backplane SA connesse tutte le soluzioni client e che i server si trovano nella. Invia il messaggio a tutti i client tramite le rispettive istanze server. Questo processo è illustrato nel diagramma seguente:

![Backplane, messaggio inviato da un server a tutti i client redis](scale/_static/redis-backplane.png)

Backplane Redis è l'approccio di scalabilità orizzontale consigliata per le app ospitate nella propria infrastruttura. Azure SignalR Service non è un'opzione possibile per la produzione con le App in locale a causa di latenza della connessione tra il data center e un data center di Azure.

I vantaggi del servizio Azure SignalR indicati in precedenza sono gli svantaggi per il backplane di Redis:

* Le sessioni permanenti, note anche come [affinità del client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), è necessario. Una volta che viene avviata una connessione in un server, la connessione deve rimanere in tale server.
* Un'app di SignalR è necessario scalare orizzontalmente basata sul numero di client anche se alcuni messaggi vengono inviati.
* Un'app di SignalR utilizza molte più risorse di connessione di un'app web senza SignalR.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Documentazione di Azure SignalR Service](/azure/azure-signalr/signalr-overview)
* [Configurare un backplane di Redis](xref:signalr/redis-backplane)
