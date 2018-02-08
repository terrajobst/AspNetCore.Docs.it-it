---
title: Supporto di WebSocket in ASP.NET Core
author: tdykstra
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introduzione agli oggetti WebSocket in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)

L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP. Viene usato per applicazioni come chat, teleborsa, giochi e per qualsiasi funzionalità in tempo reale in un'applicazione Web.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)). Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).


## <a name="prerequisites"></a>Prerequisiti

* ASP.NET Core 1.1 (la versione 1.0 non viene eseguita)
* Qualsiasi sistema operativo su cui venga eseguito ASP.NET Core:
  
  * Windows 7/Windows Server 2008 e versioni successive
  * Linux
  * macOS

* **Eccezione**: se l'app viene eseguita in Windows con IIS o con WebListener, è necessario usare:

  * Windows 8/Windows Server 2012 o versioni successive
  * IIS 8/IIS 8 Express
  * WebSocket deve essere abilitato in IIS

* Per i browser supportati, vedere http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Caso in cui utilizzare lo stato

Usare gli oggetti WebSocket quando è necessario usare direttamente una connessione socket. Ad esempio, per un gioco in tempo reale è necessario avere le prestazioni migliori possibili.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) offre un modello applicativo più ricco per la funzionalità in tempo reale, ma viene eseguito solo su ASP.NET, non su ASP.NET Core. Una versione Core di SignalR è in fase di sviluppo: per seguirne lo stato di avanzamento, vedere il [repository GitHub per SignalR Core](https://github.com/aspnet/SignalR).

Se non si vuole attendere SignalR Core, ora è possibile usare i WebSocket direttamente. Tuttavia, potrebbe essere necessario sviluppare funzionalità come quelle offerte da SignalR, come ad esempio:

* Supporto per un'ampia gamma di versioni di browser mediante il fallback automatico in metodi di trasporto alternativo.
* Riconnessione automatica quando una connessione si interrompe.
* Supporto per i client che chiamano metodi nel server o viceversa.
* Supporto per il ridimensionamento a più server.

## <a name="how-to-use-it"></a>Come usare la funzionalità

* Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configurare il middleware.
* Accettare le richieste WebSocket.
* Inviare e ricevere messaggi.

### <a name="configure-the-middleware"></a>Configurare il middleware

Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

È possibile configurare le impostazioni seguenti:

* `KeepAliveInterval`: la frequenza di invio di frame "ping" al client, per garantire che i proxy tengano aperta la connessione.
* `ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati. Solo gli utenti avanzati possono avere la necessità di modificare questa impostazione, per ottimizzare le prestazioni in base alle dimensioni dei dati.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Accettare le richieste WebSocket

In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.

Questo esempio è stato preso da un punto successivo nel metodo `Configure`.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.

### <a name="send-and-receive-messages"></a>Inviare e ricevere messaggi

Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e specifica un oggetto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket). Usare l'oggetto WebSocket per inviare e ricevere messaggi.

Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`. Di seguito viene illustrato il metodo `Echo`. Il codice riceve un messaggio e lo reinvia immediatamente. Il messaggio rimane nel ciclo finché il client non chiude la connessione. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quando si accetta il WebSocket prima che inizi il ciclo, la pipeline del middleware termina.  Dopo la chiusura del socket, la pipeline si arresta, ovvero quando si accetta una richiesta WebSocket, la richiesta non prosegue più all'interno della pipeline, così come accade quando si esegue un'azione MVC, ad esempio.  Ma quando si finisce il ciclo e si chiude il socket, la richiesta continua lungo la pipeline.

## <a name="next-steps"></a>Passaggi successivi

L'[applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) che accompagna questo articolo è una semplice applicazione eco. Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti. Eseguire l'applicazione da un prompt dei comandi in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express e passare a http://localhost:5000. Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:

![Stato iniziale della pagina Web](websockets/_static/start.png)

Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.  Immettere un messaggio di prova e selezionare **Send** (Invia). Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket). La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.

![Stato iniziale della pagina Web](websockets/_static/end.png)
