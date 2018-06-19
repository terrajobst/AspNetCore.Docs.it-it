---
title: Supporto di WebSocket in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153683"
---
# <a name="websockets-support-in-aspnet-core"></a>Supporto di WebSocket in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)

L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP. Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)). Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).

## <a name="prerequisites"></a>Prerequisiti

* ASP.NET Core 1.1 o versione successiva
* Qualsiasi sistema operativo che supporta ASP.NET Core:
  
  * Windows 7/Windows Server 2008 o versioni successive
  * Linux
  * macOS
  
* Se l'app viene eseguita su Windows con IIS:

  * Windows 8/Windows Server 2012 o versioni successive
  * IIS 8/IIS 8 Express
  * I WebSocket devono essere abilitati in IIS (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).
  
* Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8/Windows Server 2012 o versioni successive

* Per i browser supportati, vedere https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Quando usare i WebSocket

Usare gli oggetti WebSocket per operare direttamente con una connessione socket. Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.

[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) offre un modello di app più ricco per la funzionalità in tempo reale, ma viene eseguito solo su ASP.NET 4.x, non su ASP.NET Core. Una versione di SignalR per ASP.NET Core è prevista per il rilascio con ASP.NET Core 2.1. Vedere [Pianificazione generale di ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).

Fino a quando non viene rilasciata SignalR Core è possibile usare i WebSocket. Tuttavia, le caratteristiche offerte da SignalR devono essere offerte e supportate dallo sviluppatore. Ad esempio:

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

Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

È possibile configurare le impostazioni seguenti:

* `KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.
* `ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati. Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Accettare le richieste WebSocket

In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.

L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.

### <a name="send-and-receive-messages"></a>Inviare e ricevere messaggi

Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.

Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`. Il codice riceve un messaggio e lo reinvia immediatamente. I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina. Dopo la chiusura del socket, la pipeline si arresta, In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe. Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.

## <a name="iisiis-express-support"></a>Supporto di IIS/IIS Express

Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.

Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:

1. Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.
1. Selezionare **Installazione basata su ruoli o basata su funzionalità**. Scegliere **Avanti**.
1. Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita). Scegliere **Avanti**.
1. Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.
1. Selezionare **Protocollo WebSocket**. Scegliere **Avanti**.
1. Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.
1. Selezionare **Installa**.
1. Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.

Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:

1. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).
1. Espandere i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.
1. Selezionare la funzionalità **Protocollo WebSocket**. Scegliere **OK**.

**Disabilitare WebSocket quando si usa socket.io su node.js**

Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Passaggi successivi

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) inclusa in questo articolo è un'app echo. Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti. Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000. Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:

![Stato iniziale della pagina Web](websockets/_static/start.png)

Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato. Immettere un messaggio di prova e selezionare **Send** (Invia). Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket). La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.

![Stato iniziale della pagina Web](websockets/_static/end.png)
