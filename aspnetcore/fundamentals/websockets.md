---
title: Supporto di WebSocket in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b0f1aeff6c7a5777993459274293ba23f2d9dc12
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206740"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="fe5e4-103">Supporto di WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe5e4-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="fe5e4-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="fe5e4-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="fe5e4-105">L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="fe5e4-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="fe5e4-107">Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="fe5e4-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="fe5e4-109">Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe5e4-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fe5e4-110">Prerequisites</span></span>

* <span data-ttu-id="fe5e4-111">ASP.NET Core 1.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="fe5e4-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="fe5e4-112">Qualsiasi sistema operativo che supporta ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="fe5e4-113">Windows 7/Windows Server 2008 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="fe5e4-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="fe5e4-114">Linux</span><span class="sxs-lookup"><span data-stu-id="fe5e4-114">Linux</span></span>
  * <span data-ttu-id="fe5e4-115">macOS</span><span class="sxs-lookup"><span data-stu-id="fe5e4-115">macOS</span></span>
  
* <span data-ttu-id="fe5e4-116">Se l'app viene eseguita su Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="fe5e4-117">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="fe5e4-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="fe5e4-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="fe5e4-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="fe5e4-119">Gli oggetti WebSocket devono essere abilitati (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="fe5e4-120">Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="fe5e4-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="fe5e4-121">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="fe5e4-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="fe5e4-122">Per i browser supportati, vedere https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="fe5e4-123">Quando usare i WebSocket</span><span class="sxs-lookup"><span data-stu-id="fe5e4-123">When to use WebSockets</span></span>

<span data-ttu-id="fe5e4-124">Usare gli oggetti WebSocket per operare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="fe5e4-125">Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="fe5e4-126">[ASP.NET Core SignalR](xref:signalr/introduction) è una libreria che consente di aggiungere in modo più semplice funzionalità Web in tempo reale alle app.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="fe5e4-127">Laddove possibile, usa oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="fe5e4-128">Come usare oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="fe5e4-128">How to use WebSockets</span></span>

* <span data-ttu-id="fe5e4-129">Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="fe5e4-130">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-130">Configure the middleware.</span></span>
* <span data-ttu-id="fe5e4-131">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="fe5e4-132">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="fe5e4-133">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="fe5e4-133">Configure the middleware</span></span>

<span data-ttu-id="fe5e4-134">Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="fe5e4-135">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-135">The following settings can be configured:</span></span>

* <span data-ttu-id="fe5e4-136">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="fe5e4-137">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="fe5e4-138">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="fe5e4-139">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="fe5e4-139">Accept WebSocket requests</span></span>

<span data-ttu-id="fe5e4-140">In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="fe5e4-141">L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="fe5e4-142">Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="fe5e4-143">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="fe5e4-143">Send and receive messages</span></span>

<span data-ttu-id="fe5e4-144">Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="fe5e4-145">Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="fe5e4-146">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="fe5e4-147">Il codice riceve un messaggio e lo reinvia immediatamente.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="fe5e4-148">I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="fe5e4-149">Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="fe5e4-150">Dopo la chiusura del socket, la pipeline si arresta,</span><span class="sxs-lookup"><span data-stu-id="fe5e4-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="fe5e4-151">In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="fe5e4-152">Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="fe5e4-153">Supporto di IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="fe5e4-153">IIS/IIS Express support</span></span>

<span data-ttu-id="fe5e4-154">Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="fe5e4-155">Gli oggetti WebSocket sono sempre abilitati quando si usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="fe5e4-156">Abilitazione di oggetti WebSocket in IIS</span><span class="sxs-lookup"><span data-stu-id="fe5e4-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="fe5e4-157">Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="fe5e4-158">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="fe5e4-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="fe5e4-159">Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="fe5e4-160">Selezionare **Installazione basata su ruoli o basata su funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="fe5e4-161">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-161">Select **Next**.</span></span>
1. <span data-ttu-id="fe5e4-162">Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="fe5e4-163">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-163">Select **Next**.</span></span>
1. <span data-ttu-id="fe5e4-164">Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="fe5e4-165">Selezionare **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="fe5e4-166">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-166">Select **Next**.</span></span>
1. <span data-ttu-id="fe5e4-167">Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="fe5e4-168">Selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-168">Select **Install**.</span></span>
1. <span data-ttu-id="fe5e4-169">Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="fe5e4-170">Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="fe5e4-171">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="fe5e4-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="fe5e4-172">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="fe5e4-173">Espandere i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="fe5e4-174">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="fe5e4-175">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="fe5e4-176">Disabilitare WebSocket quando si usa socket.io su node.js</span><span class="sxs-lookup"><span data-stu-id="fe5e4-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="fe5e4-177">Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="fe5e4-178">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fe5e4-178">Next steps</span></span>

<span data-ttu-id="fe5e4-179">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) inclusa in questo articolo è un'app echo.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="fe5e4-180">Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="fe5e4-181">Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="fe5e4-182">Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="fe5e4-182">The web page shows the connection status in the upper left:</span></span>

![Stato iniziale della pagina Web](websockets/_static/start.png)

<span data-ttu-id="fe5e4-184">Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="fe5e4-185">Immettere un messaggio di prova e selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="fe5e4-186">Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket).</span><span class="sxs-lookup"><span data-stu-id="fe5e4-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="fe5e4-187">La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="fe5e4-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina Web](websockets/_static/end.png)
