---
title: Supporto di WebSocket in ASP.NET Core
author: tdykstra
description: Informazioni su come iniziare a utilizzare WebSocket in ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 46c1f42b925a43df470d7491a1e259ab51ea5f50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="655b2-103">Introduzione ai WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="655b2-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="655b2-104">Da [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-infermiera](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="655b2-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="655b2-105">In questo articolo viene illustrato come iniziare a utilizzare WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="655b2-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="655b2-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="655b2-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="655b2-107">Viene utilizzato per le applicazioni, ad esempio una chat, ticker, giochi, ovunque si desidera aggiungere la funzionalità in tempo reale in un'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="655b2-107">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="655b2-108">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="655b2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="655b2-109">Vedere il [passaggi successivi](#next-steps) sezione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="655b2-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="655b2-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="655b2-110">Prerequisites</span></span>

* <span data-ttu-id="655b2-111">ASP.NET Core 1.1 (non viene eseguita su 1.0)</span><span class="sxs-lookup"><span data-stu-id="655b2-111">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="655b2-112">Qualsiasi sistema operativo che viene eseguito ASP.NET Core in:</span><span class="sxs-lookup"><span data-stu-id="655b2-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="655b2-113">Windows 7 / Windows Server 2008 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="655b2-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="655b2-114">Linux</span><span class="sxs-lookup"><span data-stu-id="655b2-114">Linux</span></span>
  * <span data-ttu-id="655b2-115">macOS</span><span class="sxs-lookup"><span data-stu-id="655b2-115">macOS</span></span>

* <span data-ttu-id="655b2-116">**Eccezione**: se l'app viene eseguita in Windows con IIS o con WebListener, è necessario utilizzare:</span><span class="sxs-lookup"><span data-stu-id="655b2-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="655b2-117">Windows 8 o Windows Server 2012 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="655b2-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="655b2-118">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="655b2-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="655b2-119">WebSocket deve essere abilitato in IIS</span><span class="sxs-lookup"><span data-stu-id="655b2-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="655b2-120">Per i browser supportati, vedere http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="655b2-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="655b2-121">Caso in cui utilizzare lo stato</span><span class="sxs-lookup"><span data-stu-id="655b2-121">When to use it</span></span>

<span data-ttu-id="655b2-122">Usare i WebSocket quando è necessario lavorare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="655b2-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="655b2-123">È ad esempio, potrebbe essere necessario le migliori prestazioni possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="655b2-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="655b2-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornisce un più ricco modello di applicazione per la funzionalità in tempo reale, ma viene eseguita solo su ASP.NET, non ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="655b2-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="655b2-125">Una versione di SignalR Core è in fase di sviluppo; Per seguire lo stato di avanzamento, vedere il [repository GitHub per SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="655b2-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="655b2-126">Se non si desidera attendere SignalR Core, è possibile usare i WebSocket direttamente a questo punto.</span><span class="sxs-lookup"><span data-stu-id="655b2-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="655b2-127">Tuttavia, potrebbe essere necessario sviluppare funzionalità ai SignalR, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="655b2-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="655b2-128">Supporto per un'ampia gamma di versioni di browser utilizzando il fallback automatico per metodi di trasporto alternativo.</span><span class="sxs-lookup"><span data-stu-id="655b2-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="655b2-129">Riconnessione automatica quando si elimina una connessione.</span><span class="sxs-lookup"><span data-stu-id="655b2-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="655b2-130">Supporto per i client che chiamano metodi nel server o viceversa.</span><span class="sxs-lookup"><span data-stu-id="655b2-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="655b2-131">Supporto per la scala di più server.</span><span class="sxs-lookup"><span data-stu-id="655b2-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="655b2-132">Come usare la funzionalità</span><span class="sxs-lookup"><span data-stu-id="655b2-132">How to use it</span></span>

* <span data-ttu-id="655b2-133">Installare il [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="655b2-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="655b2-134">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="655b2-134">Configure the middleware.</span></span>
* <span data-ttu-id="655b2-135">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="655b2-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="655b2-136">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="655b2-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="655b2-137">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="655b2-137">Configure the middleware</span></span>

<span data-ttu-id="655b2-138">Aggiungere il middleware WebSocket il `Configure` metodo la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="655b2-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="655b2-139">Possono essere configurate le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="655b2-139">The following settings can be configured:</span></span>

* <span data-ttu-id="655b2-140">`KeepAliveInterval`-La frequenza di invio frame "ping" al client, per garantire proxy tenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="655b2-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="655b2-141">`ReceiveBufferSize`-La dimensione del buffer utilizzato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="655b2-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="655b2-142">Solo gli utenti avanzati sarebbero necessario modificare questa impostazione, per ottimizzare le prestazioni in base alla dimensione dei dati.</span><span class="sxs-lookup"><span data-stu-id="655b2-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="655b2-143">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="655b2-143">Accept WebSocket requests</span></span>

<span data-ttu-id="655b2-144">In un punto successivo nel ciclo di vita della richiesta (più avanti nel `Configure` metodo o un'azione MVC, ad esempio) verificare che sia una richiesta WebSocket e accettare la richiesta WebSocket.</span><span class="sxs-lookup"><span data-stu-id="655b2-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="655b2-145">Questo esempio è tratto successivamente nel `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="655b2-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="655b2-146">Una richiesta WebSocket potrebbe giungere in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="655b2-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="655b2-147">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="655b2-147">Send and receive messages</span></span>

<span data-ttu-id="655b2-148">Il `AcceptWebSocketAsync` metodo consente di aggiornare la connessione TCP a una connessione WebSocket e offre un [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) oggetto.</span><span class="sxs-lookup"><span data-stu-id="655b2-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="655b2-149">Utilizzare l'oggetto WebSocket per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="655b2-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="655b2-150">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa il `WebSocket` l'oggetto in un `Echo` (metodo), ecco il `Echo` metodo.</span><span class="sxs-lookup"><span data-stu-id="655b2-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="655b2-151">Il codice riceve un messaggio e invia immediatamente il messaggio stesso.</span><span class="sxs-lookup"><span data-stu-id="655b2-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="655b2-152">Rimane in un ciclo di esecuzione di operazioni con fino a quando il client chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="655b2-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="655b2-153">Quando si accetta il WebSocket prima di iniziare il ciclo, la pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="655b2-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="655b2-154">Dopo la chiusura del socket, rimuove la pipeline.</span><span class="sxs-lookup"><span data-stu-id="655b2-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="655b2-155">Ovvero, si interrompe il richiesta procedendo nella pipeline, quando si accetta un WebSocket, esattamente come accade sarebbe quando si raggiunge un'azione MVC, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="655b2-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="655b2-156">Tuttavia, quando si finisce di questo ciclo e si chiude il socket, la richiesta continua la pipeline di backup.</span><span class="sxs-lookup"><span data-stu-id="655b2-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="655b2-157">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="655b2-157">Next steps</span></span>

<span data-ttu-id="655b2-158">Il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) che accompagna questo articolo è un'applicazione semplice echo.</span><span class="sxs-lookup"><span data-stu-id="655b2-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="655b2-159">Dispone di una pagina web che consente la connessione WebSocket e il server appena reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="655b2-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="655b2-160">Eseguirlo da un prompt dei comandi (ha non impostato per l'esecuzione da Visual Studio con IIS Express) e passare all'indirizzo http://localhost:5000/.</span><span class="sxs-lookup"><span data-stu-id="655b2-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="655b2-161">La pagina web Mostra lo stato di connessione nella parte superiore sinistra:</span><span class="sxs-lookup"><span data-stu-id="655b2-161">The web page shows connection status at the upper left:</span></span>

![Stato iniziale della pagina web](websockets/_static/start.png)

<span data-ttu-id="655b2-163">Selezionare **Connetti** per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="655b2-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="655b2-164">Immettere un messaggio di prova e selezionare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="655b2-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="655b2-165">Al termine, selezionare **Chiudi Socket**.</span><span class="sxs-lookup"><span data-stu-id="655b2-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="655b2-166">Il **comunicazione Log** sezione segnala ogni open, send, e si verifica l'azione Chiudi.</span><span class="sxs-lookup"><span data-stu-id="655b2-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina web](websockets/_static/end.png)
