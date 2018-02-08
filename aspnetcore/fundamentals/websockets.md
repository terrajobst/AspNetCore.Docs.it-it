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
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="fb596-103">Introduzione agli oggetti WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb596-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="fb596-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="fb596-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="fb596-105">L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb596-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="fb596-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="fb596-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="fb596-107">Viene usato per applicazioni come chat, teleborsa, giochi e per qualsiasi funzionalità in tempo reale in un'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="fb596-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="fb596-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fb596-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="fb596-109">Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="fb596-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fb596-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fb596-110">Prerequisites</span></span>

* <span data-ttu-id="fb596-111">ASP.NET Core 1.1 (la versione 1.0 non viene eseguita)</span><span class="sxs-lookup"><span data-stu-id="fb596-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="fb596-112">Qualsiasi sistema operativo su cui venga eseguito ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fb596-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="fb596-113">Windows 7/Windows Server 2008 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="fb596-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="fb596-114">Linux</span><span class="sxs-lookup"><span data-stu-id="fb596-114">Linux</span></span>
  * <span data-ttu-id="fb596-115">macOS</span><span class="sxs-lookup"><span data-stu-id="fb596-115">macOS</span></span>

* <span data-ttu-id="fb596-116">**Eccezione**: se l'app viene eseguita in Windows con IIS o con WebListener, è necessario usare:</span><span class="sxs-lookup"><span data-stu-id="fb596-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="fb596-117">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="fb596-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="fb596-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="fb596-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="fb596-119">WebSocket deve essere abilitato in IIS</span><span class="sxs-lookup"><span data-stu-id="fb596-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="fb596-120">Per i browser supportati, vedere http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="fb596-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="fb596-121">Caso in cui utilizzare lo stato</span><span class="sxs-lookup"><span data-stu-id="fb596-121">When to use it</span></span>

<span data-ttu-id="fb596-122">Usare gli oggetti WebSocket quando è necessario usare direttamente una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="fb596-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="fb596-123">Ad esempio, per un gioco in tempo reale è necessario avere le prestazioni migliori possibili.</span><span class="sxs-lookup"><span data-stu-id="fb596-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="fb596-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) offre un modello applicativo più ricco per la funzionalità in tempo reale, ma viene eseguito solo su ASP.NET, non su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb596-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="fb596-125">Una versione Core di SignalR è in fase di sviluppo: per seguirne lo stato di avanzamento, vedere il [repository GitHub per SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="fb596-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="fb596-126">Se non si vuole attendere SignalR Core, ora è possibile usare i WebSocket direttamente.</span><span class="sxs-lookup"><span data-stu-id="fb596-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="fb596-127">Tuttavia, potrebbe essere necessario sviluppare funzionalità come quelle offerte da SignalR, come ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fb596-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="fb596-128">Supporto per un'ampia gamma di versioni di browser mediante il fallback automatico in metodi di trasporto alternativo.</span><span class="sxs-lookup"><span data-stu-id="fb596-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="fb596-129">Riconnessione automatica quando una connessione si interrompe.</span><span class="sxs-lookup"><span data-stu-id="fb596-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="fb596-130">Supporto per i client che chiamano metodi nel server o viceversa.</span><span class="sxs-lookup"><span data-stu-id="fb596-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="fb596-131">Supporto per il ridimensionamento a più server.</span><span class="sxs-lookup"><span data-stu-id="fb596-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="fb596-132">Come usare la funzionalità</span><span class="sxs-lookup"><span data-stu-id="fb596-132">How to use it</span></span>

* <span data-ttu-id="fb596-133">Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="fb596-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="fb596-134">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="fb596-134">Configure the middleware.</span></span>
* <span data-ttu-id="fb596-135">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fb596-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="fb596-136">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="fb596-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="fb596-137">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="fb596-137">Configure the middleware</span></span>

<span data-ttu-id="fb596-138">Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="fb596-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="fb596-139">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fb596-139">The following settings can be configured:</span></span>

* <span data-ttu-id="fb596-140">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client, per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="fb596-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="fb596-141">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="fb596-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="fb596-142">Solo gli utenti avanzati possono avere la necessità di modificare questa impostazione, per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="fb596-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="fb596-143">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="fb596-143">Accept WebSocket requests</span></span>

<span data-ttu-id="fb596-144">In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.</span><span class="sxs-lookup"><span data-stu-id="fb596-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="fb596-145">Questo esempio è stato preso da un punto successivo nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="fb596-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="fb596-146">Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="fb596-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="fb596-147">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="fb596-147">Send and receive messages</span></span>

<span data-ttu-id="fb596-148">Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e specifica un oggetto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="fb596-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="fb596-149">Usare l'oggetto WebSocket per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="fb596-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="fb596-150">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`. Di seguito viene illustrato il metodo `Echo`.</span><span class="sxs-lookup"><span data-stu-id="fb596-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="fb596-151">Il codice riceve un messaggio e lo reinvia immediatamente.</span><span class="sxs-lookup"><span data-stu-id="fb596-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="fb596-152">Il messaggio rimane nel ciclo finché il client non chiude la connessione.</span><span class="sxs-lookup"><span data-stu-id="fb596-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="fb596-153">Quando si accetta il WebSocket prima che inizi il ciclo, la pipeline del middleware termina.</span><span class="sxs-lookup"><span data-stu-id="fb596-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="fb596-154">Dopo la chiusura del socket, la pipeline si arresta,</span><span class="sxs-lookup"><span data-stu-id="fb596-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="fb596-155">ovvero quando si accetta una richiesta WebSocket, la richiesta non prosegue più all'interno della pipeline, così come accade quando si esegue un'azione MVC, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="fb596-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="fb596-156">Ma quando si finisce il ciclo e si chiude il socket, la richiesta continua lungo la pipeline.</span><span class="sxs-lookup"><span data-stu-id="fb596-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb596-157">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fb596-157">Next steps</span></span>

<span data-ttu-id="fb596-158">L'[applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) che accompagna questo articolo è una semplice applicazione eco.</span><span class="sxs-lookup"><span data-stu-id="fb596-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="fb596-159">Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="fb596-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="fb596-160">Eseguire l'applicazione da un prompt dei comandi in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express e passare a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="fb596-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="fb596-161">Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="fb596-161">The web page shows connection status at the upper left:</span></span>

![Stato iniziale della pagina Web](websockets/_static/start.png)

<span data-ttu-id="fb596-163">Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="fb596-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="fb596-164">Immettere un messaggio di prova e selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="fb596-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="fb596-165">Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket).</span><span class="sxs-lookup"><span data-stu-id="fb596-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="fb596-166">La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="fb596-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina Web](websockets/_static/end.png)
