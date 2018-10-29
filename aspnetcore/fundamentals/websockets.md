---
title: Supporto di WebSocket in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b1e2180ed8dc93e2474ecca371d386830b7f3a9f
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348455"
---
# <a name="websockets-support-in-aspnet-core"></a>Supporto di WebSocket in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)

L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP. Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)). Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).

## <a name="prerequisites"></a>Prerequisiti

* ASP.NET Core 1.1 o versione successiva
* Qualsiasi sistema operativo che supporta ASP.NET Core:
  
  * Windows 7/Windows Server 2008 o versioni successive
  * Linux
  * macOS
  
* Se l'app viene eseguita su Windows con IIS:

  * Windows 8/Windows Server 2012 o versioni successive
  * IIS 8/IIS 8 Express
  * Gli oggetti WebSocket devono essere abilitati (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).
  
* Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8/Windows Server 2012 o versioni successive

* Per i browser supportati, vedere https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Quando usare i WebSocket

Usare gli oggetti WebSocket per operare direttamente con una connessione socket. Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.

[ASP.NET Core SignalR](xref:signalr/introduction) è una libreria che consente di aggiungere in modo più semplice funzionalità Web in tempo reale alle app. Laddove possibile, usa oggetti WebSocket.

## <a name="how-to-use-websockets"></a>Come usare oggetti WebSocket

* Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configurare il middleware.
* Accettare le richieste WebSocket.
* Inviare e ricevere messaggi.

### <a name="configure-the-middleware"></a>Configurare il middleware

Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

È possibile configurare le impostazioni seguenti:

* `KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.
* `ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati. Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a>Accettare le richieste WebSocket

In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.

L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.

### <a name="send-and-receive-messages"></a>Inviare e ricevere messaggi

Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.

Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`. Il codice riceve un messaggio e lo reinvia immediatamente. I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina. Dopo la chiusura del socket, la pipeline si arresta, In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe. Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.

## <a name="iisiis-express-support"></a>Supporto di IIS/IIS Express

Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.

> [!NOTE]
> Gli oggetti WebSocket sono sempre abilitati quando si usa IIS Express.

### <a name="enabling-websockets-on-iis"></a>Abilitazione di oggetti WebSocket in IIS

Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:

> [!NOTE]
> Questi passaggi non sono necessari quando si usa IIS Express

1. Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.
1. Selezionare **Installazione basata su ruoli o basata su funzionalità**. Scegliere **Avanti**.
1. Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita). Scegliere **Avanti**.
1. Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.
1. Selezionare **Protocollo WebSocket**. Scegliere **Avanti**.
1. Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.
1. Selezionare **Installa**.
1. Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.

Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:

> [!NOTE]
> Questi passaggi non sono necessari quando si usa IIS Express

1. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).
1. Espandere i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.
1. Selezionare la funzionalità **Protocollo WebSocket**. Scegliere **OK**.

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>Disabilitare WebSocket quando si usa socket.io su node.js

Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Passaggi successivi

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) inclusa in questo articolo è un'app echo. Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti. Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000. Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:

![Stato iniziale della pagina Web](websockets/_static/start.png)

Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato. Immettere un messaggio di prova e selezionare **Send** (Invia). Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket). La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.

![Stato iniziale della pagina Web](websockets/_static/end.png)
