---
title: Supporto di WebSocket in ASP.NET Core
author: tdykstra
description: "Che cos'è WebSocket è supportato in ASP.NET Core e come utilizzarla."
keywords: ASP.NET Core, WebSocket
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: daa36d117749868e4bb9311a941b7751d7b64744
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introduzione ai WebSocket in ASP.NET Core

Da [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-infermiera](https://github.com/anurse)

In questo articolo viene illustrato come iniziare a utilizzare WebSocket in ASP.NET Core. [WebSocket](https://en.wikipedia.org/wiki/WebSocket) è un protocollo che consente di canali di comunicazione persistente su connessioni TCP. Viene utilizzato per le applicazioni, ad esempio una chat, ticker, giochi, ovunque si desidera aggiungere la funzionalità in tempo reale in un'applicazione web.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample). Vedere il [passaggi successivi](#next-steps) sezione per ulteriori informazioni.


## <a name="prerequisites"></a>Prerequisiti

* ASP.NET Core 1.1 (non viene eseguita su 1.0)
* Qualsiasi sistema operativo che viene eseguito ASP.NET Core in:
  
  * Windows 7 / Windows Server 2008 e versioni successive
  * Linux
  * MacOS

* **Eccezione**: se l'app viene eseguita in Windows con IIS o con WebListener, è necessario utilizzare:

  * Windows 8 o Windows Server 2012 o versione successiva
  * IIS 8 / 8 IIS Express
  * WebSocket deve essere abilitato in IIS

* Per i browser supportati, vedere http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Caso in cui utilizzare lo stato

Usare i WebSocket quando è necessario lavorare direttamente con una connessione socket. È ad esempio, potrebbe essere necessario le migliori prestazioni possibili per un gioco in tempo reale.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornisce un più ricco modello di applicazione per la funzionalità in tempo reale, ma viene eseguita solo su ASP.NET, non ASP.NET Core. Una versione di SignalR Core è in fase di sviluppo; Per seguire lo stato di avanzamento, vedere il [repository GitHub per SignalR Core](https://github.com/aspnet/SignalR).

Se non si desidera attendere SignalR Core, è possibile usare i WebSocket direttamente a questo punto. Tuttavia, potrebbe essere necessario sviluppare funzionalità ai SignalR, ad esempio:

* Supporto per un'ampia gamma di versioni di browser utilizzando il fallback automatico per metodi di trasporto alternativo.
* Riconnessione automatica quando si elimina una connessione.
* Supporto per i client che chiamano metodi nel server o viceversa.
* Supporto per la scala di più server.

## <a name="how-to-use-it"></a>Come utilizzare questo strumento

* Installare il [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pacchetto.
* Configurare il middleware.
* Accettare le richieste WebSocket.
* Inviare e ricevere messaggi.

### <a name="configure-the-middleware"></a>Configurare il middleware

Aggiungere il middleware WebSocket il `Configure` metodo la `Startup` classe.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Possono essere configurate le impostazioni seguenti:

* `KeepAliveInterval`-La frequenza di invio frame "ping" al client, per garantire proxy tenere aperta la connessione.
* `ReceiveBufferSize`-La dimensione del buffer utilizzato per ricevere dati. Solo gli utenti avanzati sarebbero necessario modificare questa impostazione, per ottimizzare le prestazioni in base alla dimensione dei dati.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Accettare le richieste WebSocket

In un punto successivo nel ciclo di vita della richiesta (più avanti nel `Configure` metodo o un'azione MVC, ad esempio) verificare che sia una richiesta WebSocket e accettare la richiesta WebSocket.

Questo esempio è tratto successivamente nel `Configure` metodo.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Una richiesta WebSocket potrebbe giungere in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.

### <a name="send-and-receive-messages"></a>Inviare e ricevere messaggi

Il `AcceptWebSocketAsync` metodo consente di aggiornare la connessione TCP a una connessione WebSocket e offre un [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) oggetto. Utilizzare l'oggetto WebSocket per inviare e ricevere messaggi.

Il codice illustrato in precedenza che accetta la richiesta WebSocket passa il `WebSocket` l'oggetto in un `Echo` (metodo), ecco il `Echo` metodo. Il codice riceve un messaggio e invia immediatamente il messaggio stesso. Rimane in un ciclo di esecuzione di operazioni con fino a quando il client chiude la connessione. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quando si accetta il WebSocket prima di iniziare il ciclo, la pipeline middleware.  Dopo la chiusura del socket, rimuove la pipeline. Ovvero, si interrompe il richiesta procedendo nella pipeline, quando si accetta un WebSocket, esattamente come accade sarebbe quando si raggiunge un'azione MVC, ad esempio.  Tuttavia, quando si finisce di questo ciclo e si chiude il socket, la richiesta continua la pipeline di backup.

## <a name="next-steps"></a>Passaggi successivi

Il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) che accompagna questo articolo è un'applicazione semplice echo. Dispone di una pagina web che consente la connessione WebSocket e il server appena reinvia al client tutti i messaggi ricevuti. Eseguirlo da un prompt dei comandi (ha non impostato per l'esecuzione da Visual Studio con IIS Express) e passare all'indirizzo http://localhost:5000/. La pagina web Mostra lo stato di connessione nella parte superiore sinistra:

![Stato iniziale della pagina web](websockets/_static/start.png)

Selezionare **Connetti** per inviare una richiesta WebSocket per l'URL indicato.  Immettere un messaggio di prova e selezionare **inviare**. Al termine, selezionare **Chiudi Socket**. Il **comunicazione Log** sezione segnala ogni open, send, e si verifica l'azione Chiudi.

![Stato iniziale della pagina web](websockets/_static/end.png)
