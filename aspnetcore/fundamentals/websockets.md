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
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="a789c-103">Supporto di WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a789c-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="a789c-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="a789c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="a789c-105">L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a789c-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="a789c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="a789c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="a789c-107">Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.</span><span class="sxs-lookup"><span data-stu-id="a789c-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="a789c-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a789c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a789c-109">Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="a789c-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a789c-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a789c-110">Prerequisites</span></span>

* <span data-ttu-id="a789c-111">ASP.NET Core 1.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="a789c-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="a789c-112">Qualsiasi sistema operativo che supporta ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a789c-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="a789c-113">Windows 7/Windows Server 2008 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="a789c-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="a789c-114">Linux</span><span class="sxs-lookup"><span data-stu-id="a789c-114">Linux</span></span>
  * <span data-ttu-id="a789c-115">macOS</span><span class="sxs-lookup"><span data-stu-id="a789c-115">macOS</span></span>
  
* <span data-ttu-id="a789c-116">Se l'app viene eseguita su Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="a789c-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="a789c-117">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="a789c-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="a789c-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="a789c-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="a789c-119">I WebSocket devono essere abilitati in IIS (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="a789c-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="a789c-120">Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="a789c-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="a789c-121">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="a789c-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="a789c-122">Per i browser supportati, vedere https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="a789c-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="a789c-123">Quando usare i WebSocket</span><span class="sxs-lookup"><span data-stu-id="a789c-123">When to use WebSockets</span></span>

<span data-ttu-id="a789c-124">Usare gli oggetti WebSocket per operare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="a789c-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="a789c-125">Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="a789c-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="a789c-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) offre un modello di app più ricco per la funzionalità in tempo reale, ma viene eseguito solo su ASP.NET 4.x, non su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a789c-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="a789c-127">Una versione di SignalR per ASP.NET Core è prevista per il rilascio con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a789c-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="a789c-128">Vedere [Pianificazione generale di ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="a789c-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="a789c-129">Fino a quando non viene rilasciata SignalR Core è possibile usare i WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a789c-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="a789c-130">Tuttavia, le caratteristiche offerte da SignalR devono essere offerte e supportate dallo sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="a789c-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="a789c-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a789c-131">For example:</span></span>

* <span data-ttu-id="a789c-132">Supporto per un'ampia gamma di versioni di browser mediante il fallback automatico in metodi di trasporto alternativo.</span><span class="sxs-lookup"><span data-stu-id="a789c-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="a789c-133">Riconnessione automatica quando una connessione si interrompe.</span><span class="sxs-lookup"><span data-stu-id="a789c-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="a789c-134">Supporto per i client che chiamano metodi nel server o viceversa.</span><span class="sxs-lookup"><span data-stu-id="a789c-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="a789c-135">Supporto per il ridimensionamento a più server.</span><span class="sxs-lookup"><span data-stu-id="a789c-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="a789c-136">Come usare la funzionalità</span><span class="sxs-lookup"><span data-stu-id="a789c-136">How to use it</span></span>

* <span data-ttu-id="a789c-137">Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="a789c-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="a789c-138">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="a789c-138">Configure the middleware.</span></span>
* <span data-ttu-id="a789c-139">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a789c-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="a789c-140">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="a789c-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="a789c-141">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="a789c-141">Configure the middleware</span></span>

<span data-ttu-id="a789c-142">Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a789c-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="a789c-143">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a789c-143">The following settings can be configured:</span></span>

* <span data-ttu-id="a789c-144">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="a789c-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="a789c-145">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="a789c-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="a789c-146">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="a789c-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="a789c-147">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="a789c-147">Accept WebSocket requests</span></span>

<span data-ttu-id="a789c-148">In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.</span><span class="sxs-lookup"><span data-stu-id="a789c-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="a789c-149">L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="a789c-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="a789c-150">Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="a789c-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="a789c-151">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="a789c-151">Send and receive messages</span></span>

<span data-ttu-id="a789c-152">Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="a789c-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="a789c-153">Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="a789c-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="a789c-154">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`.</span><span class="sxs-lookup"><span data-stu-id="a789c-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="a789c-155">Il codice riceve un messaggio e lo reinvia immediatamente.</span><span class="sxs-lookup"><span data-stu-id="a789c-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="a789c-156">I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:</span><span class="sxs-lookup"><span data-stu-id="a789c-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="a789c-157">Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina.</span><span class="sxs-lookup"><span data-stu-id="a789c-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="a789c-158">Dopo la chiusura del socket, la pipeline si arresta,</span><span class="sxs-lookup"><span data-stu-id="a789c-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="a789c-159">In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe.</span><span class="sxs-lookup"><span data-stu-id="a789c-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="a789c-160">Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="a789c-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="a789c-161">Supporto di IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="a789c-161">IIS/IIS Express support</span></span>

<span data-ttu-id="a789c-162">Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a789c-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="a789c-163">Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="a789c-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="a789c-164">Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="a789c-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="a789c-165">Selezionare **Installazione basata su ruoli o basata su funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="a789c-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="a789c-166">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a789c-166">Select **Next**.</span></span>
1. <span data-ttu-id="a789c-167">Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="a789c-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="a789c-168">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a789c-168">Select **Next**.</span></span>
1. <span data-ttu-id="a789c-169">Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="a789c-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="a789c-170">Selezionare **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="a789c-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="a789c-171">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a789c-171">Select **Next**.</span></span>
1. <span data-ttu-id="a789c-172">Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a789c-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="a789c-173">Selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="a789c-173">Select **Install**.</span></span>
1. <span data-ttu-id="a789c-174">Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="a789c-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="a789c-175">Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="a789c-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="a789c-176">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="a789c-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="a789c-177">Espandere i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="a789c-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="a789c-178">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="a789c-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="a789c-179">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="a789c-179">Select **OK**.</span></span>

<span data-ttu-id="a789c-180">**Disabilitare WebSocket quando si usa socket.io su node.js**</span><span class="sxs-lookup"><span data-stu-id="a789c-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="a789c-181">Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.</span><span class="sxs-lookup"><span data-stu-id="a789c-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="a789c-182">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a789c-182">Next steps</span></span>

<span data-ttu-id="a789c-183">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) inclusa in questo articolo è un'app echo.</span><span class="sxs-lookup"><span data-stu-id="a789c-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="a789c-184">Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="a789c-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="a789c-185">Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="a789c-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="a789c-186">Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="a789c-186">The web page shows the connection status in the upper left:</span></span>

![Stato iniziale della pagina Web](websockets/_static/start.png)

<span data-ttu-id="a789c-188">Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="a789c-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="a789c-189">Immettere un messaggio di prova e selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="a789c-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="a789c-190">Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket).</span><span class="sxs-lookup"><span data-stu-id="a789c-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="a789c-191">La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="a789c-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina Web](websockets/_static/end.png)
