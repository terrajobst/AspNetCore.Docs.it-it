---
title: Supporto di WebSocket in ASP.NET Core
author: tdykstra
description: Informazioni su come iniziare a utilizzare WebSocket in ASP.NET Core.
keywords: ASP.NET Core, WebSocket
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 114d52d831668e5facd1142b5f9e5f68e7456f7e
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="bee9b-104">Introduzione ai WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bee9b-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="bee9b-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-infermiera](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bee9b-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="bee9b-106">In questo articolo viene illustrato come iniziare a utilizzare WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bee9b-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="bee9b-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente di canali di comunicazione persistente su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="bee9b-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="bee9b-108">Viene utilizzato per le applicazioni, ad esempio una chat, ticker, giochi, ovunque si desidera aggiungere la funzionalità in tempo reale in un'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="bee9b-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="bee9b-109">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="bee9b-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="bee9b-110">Vedere il [passaggi successivi](#next-steps) sezione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="bee9b-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="bee9b-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bee9b-111">Prerequisites</span></span>

* <span data-ttu-id="bee9b-112">ASP.NET Core 1.1 (non viene eseguita su 1.0)</span><span class="sxs-lookup"><span data-stu-id="bee9b-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="bee9b-113">Qualsiasi sistema operativo che viene eseguito ASP.NET Core in:</span><span class="sxs-lookup"><span data-stu-id="bee9b-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="bee9b-114">Windows 7 / Windows Server 2008 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="bee9b-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="bee9b-115">Linux</span><span class="sxs-lookup"><span data-stu-id="bee9b-115">Linux</span></span>
  * <span data-ttu-id="bee9b-116">MacOS</span><span class="sxs-lookup"><span data-stu-id="bee9b-116">macOS</span></span>

* <span data-ttu-id="bee9b-117">**Eccezione**: se l'app viene eseguita in Windows con IIS o con WebListener, è necessario utilizzare:</span><span class="sxs-lookup"><span data-stu-id="bee9b-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="bee9b-118">Windows 8 o Windows Server 2012 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="bee9b-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="bee9b-119">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="bee9b-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="bee9b-120">WebSocket deve essere abilitato in IIS</span><span class="sxs-lookup"><span data-stu-id="bee9b-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="bee9b-121">Per i browser supportati, vedere http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="bee9b-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="bee9b-122">Caso in cui utilizzare lo stato</span><span class="sxs-lookup"><span data-stu-id="bee9b-122">When to use it</span></span>

<span data-ttu-id="bee9b-123">Usare i WebSocket quando è necessario lavorare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="bee9b-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="bee9b-124">È ad esempio, potrebbe essere necessario le migliori prestazioni possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="bee9b-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="bee9b-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornisce un più ricco modello di applicazione per la funzionalità in tempo reale, ma viene eseguita solo su ASP.NET, non ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bee9b-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="bee9b-126">Una versione di SignalR Core è in fase di sviluppo; Per seguire lo stato di avanzamento, vedere il [repository GitHub per SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="bee9b-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="bee9b-127">Se non si desidera attendere SignalR Core, è possibile usare i WebSocket direttamente a questo punto.</span><span class="sxs-lookup"><span data-stu-id="bee9b-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="bee9b-128">Tuttavia, potrebbe essere necessario sviluppare funzionalità ai SignalR, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bee9b-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="bee9b-129">Supporto per un'ampia gamma di versioni di browser utilizzando il fallback automatico per metodi di trasporto alternativo.</span><span class="sxs-lookup"><span data-stu-id="bee9b-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="bee9b-130">Riconnessione automatica quando si elimina una connessione.</span><span class="sxs-lookup"><span data-stu-id="bee9b-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="bee9b-131">Supporto per i client che chiamano metodi nel server o viceversa.</span><span class="sxs-lookup"><span data-stu-id="bee9b-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="bee9b-132">Supporto per la scala di più server.</span><span class="sxs-lookup"><span data-stu-id="bee9b-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="bee9b-133">Come utilizzare questo strumento</span><span class="sxs-lookup"><span data-stu-id="bee9b-133">How to use it</span></span>

* <span data-ttu-id="bee9b-134">Installare il [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="bee9b-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="bee9b-135">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="bee9b-135">Configure the middleware.</span></span>
* <span data-ttu-id="bee9b-136">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bee9b-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="bee9b-137">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="bee9b-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="bee9b-138">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="bee9b-138">Configure the middleware</span></span>

<span data-ttu-id="bee9b-139">Aggiungere il middleware WebSocket il `Configure` metodo la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="bee9b-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="bee9b-140">Possono essere configurate le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bee9b-140">The following settings can be configured:</span></span>

* <span data-ttu-id="bee9b-141">`KeepAliveInterval`-La frequenza di invio frame "ping" al client, per garantire proxy tenere aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="bee9b-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="bee9b-142">`ReceiveBufferSize`-La dimensione del buffer utilizzato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="bee9b-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="bee9b-143">Solo gli utenti avanzati sarebbero necessario modificare questa impostazione, per ottimizzare le prestazioni in base alla dimensione dei dati.</span><span class="sxs-lookup"><span data-stu-id="bee9b-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="bee9b-144">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="bee9b-144">Accept WebSocket requests</span></span>

<span data-ttu-id="bee9b-145">In un punto successivo nel ciclo di vita della richiesta (più avanti nel `Configure` metodo o un'azione MVC, ad esempio) verificare che sia una richiesta WebSocket e accettare la richiesta WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bee9b-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="bee9b-146">Questo esempio è tratto successivamente nel `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="bee9b-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="bee9b-147">Una richiesta WebSocket potrebbe giungere in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="bee9b-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="bee9b-148">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="bee9b-148">Send and receive messages</span></span>

<span data-ttu-id="bee9b-149">Il `AcceptWebSocketAsync` metodo consente di aggiornare la connessione TCP a una connessione WebSocket e offre un [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) oggetto.</span><span class="sxs-lookup"><span data-stu-id="bee9b-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="bee9b-150">Utilizzare l'oggetto WebSocket per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="bee9b-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="bee9b-151">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa il `WebSocket` l'oggetto in un `Echo` (metodo), ecco il `Echo` metodo.</span><span class="sxs-lookup"><span data-stu-id="bee9b-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="bee9b-152">Il codice riceve un messaggio e invia immediatamente il messaggio stesso.</span><span class="sxs-lookup"><span data-stu-id="bee9b-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="bee9b-153">Rimane in un ciclo di esecuzione di operazioni con fino a quando il client chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="bee9b-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="bee9b-154">Quando si accetta il WebSocket prima di iniziare il ciclo, la pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="bee9b-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="bee9b-155">Dopo la chiusura del socket, rimuove la pipeline.</span><span class="sxs-lookup"><span data-stu-id="bee9b-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="bee9b-156">Ovvero, si interrompe il richiesta procedendo nella pipeline, quando si accetta un WebSocket, esattamente come accade sarebbe quando si raggiunge un'azione MVC, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="bee9b-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="bee9b-157">Tuttavia, quando si finisce di questo ciclo e si chiude il socket, la richiesta continua la pipeline di backup.</span><span class="sxs-lookup"><span data-stu-id="bee9b-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bee9b-158">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bee9b-158">Next steps</span></span>

<span data-ttu-id="bee9b-159">Il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) che accompagna questo articolo è un'applicazione semplice echo.</span><span class="sxs-lookup"><span data-stu-id="bee9b-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="bee9b-160">Dispone di una pagina web che consente la connessione WebSocket e il server appena reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="bee9b-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="bee9b-161">Eseguirlo da un prompt dei comandi (ha non impostato per l'esecuzione da Visual Studio con IIS Express) e passare all'indirizzo http://localhost:5000/.</span><span class="sxs-lookup"><span data-stu-id="bee9b-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="bee9b-162">La pagina web Mostra lo stato di connessione nella parte superiore sinistra:</span><span class="sxs-lookup"><span data-stu-id="bee9b-162">The web page shows connection status at the upper left:</span></span>

![Stato iniziale della pagina web](websockets/_static/start.png)

<span data-ttu-id="bee9b-164">Selezionare **Connetti** per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="bee9b-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="bee9b-165">Immettere un messaggio di prova e selezionare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="bee9b-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="bee9b-166">Al termine, selezionare **Chiudi Socket**.</span><span class="sxs-lookup"><span data-stu-id="bee9b-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="bee9b-167">Il **comunicazione Log** sezione segnala ogni open, send, e si verifica l'azione Chiudi.</span><span class="sxs-lookup"><span data-stu-id="bee9b-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina web](websockets/_static/end.png)
