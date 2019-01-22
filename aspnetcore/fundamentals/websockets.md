---
title: Supporto di WebSocket in ASP.NET Core
author: rick-anderson
description: Introduzione all'uso di oggetti WebSocket in ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/06/2018
uid: fundamentals/websockets
ms.openlocfilehash: 6c32269181ea3311c4aea99c08a1c043e7833b05
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341452"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="1adbb-103">Supporto di WebSocket in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1adbb-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="1adbb-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="1adbb-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="1adbb-105">L'articolo contiene l'introduzione all'uso di oggetti WebSocket in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1adbb-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="1adbb-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP.</span><span class="sxs-lookup"><span data-stu-id="1adbb-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="1adbb-107">Viene usato nelle app che sfruttano comunicazioni veloci e in tempo reale, ad esempio le app di chat, i dashboard e le app di gioco.</span><span class="sxs-lookup"><span data-stu-id="1adbb-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="1adbb-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1adbb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="1adbb-109">Per altre informazioni, vedere la sezione [Passaggi successivi](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="1adbb-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1adbb-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1adbb-110">Prerequisites</span></span>

* <span data-ttu-id="1adbb-111">ASP.NET Core 1.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1adbb-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="1adbb-112">Qualsiasi sistema operativo che supporta ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1adbb-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="1adbb-113">Windows 7/Windows Server 2008 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1adbb-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="1adbb-114">Linux</span><span class="sxs-lookup"><span data-stu-id="1adbb-114">Linux</span></span>
  * <span data-ttu-id="1adbb-115">macOS</span><span class="sxs-lookup"><span data-stu-id="1adbb-115">macOS</span></span>
  
* <span data-ttu-id="1adbb-116">Se l'app viene eseguita su Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="1adbb-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="1adbb-117">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1adbb-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="1adbb-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="1adbb-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="1adbb-119">Gli oggetti WebSocket devono essere abilitati (vedere la sezione [Supporto di IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="1adbb-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="1adbb-120">Se l'app viene eseguita su [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="1adbb-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="1adbb-121">Windows 8/Windows Server 2012 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1adbb-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="1adbb-122">Per i browser supportati, vedere https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="1adbb-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="1adbb-123">Quando usare i WebSocket</span><span class="sxs-lookup"><span data-stu-id="1adbb-123">When to use WebSockets</span></span>

<span data-ttu-id="1adbb-124">Usare gli oggetti WebSocket per operare direttamente con una connessione socket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="1adbb-125">Ad esempio usare i WebSocket per avere le prestazioni migliori possibili per un gioco in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="1adbb-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="1adbb-126">[ASP.NET Core SignalR](xref:signalr/introduction) è una libreria che consente di aggiungere in modo più semplice funzionalità Web in tempo reale alle app.</span><span class="sxs-lookup"><span data-stu-id="1adbb-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="1adbb-127">Laddove possibile, usa oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="1adbb-128">Come usare oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="1adbb-128">How to use WebSockets</span></span>

* <span data-ttu-id="1adbb-129">Installare il pacchetto [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="1adbb-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="1adbb-130">Configurare il middleware.</span><span class="sxs-lookup"><span data-stu-id="1adbb-130">Configure the middleware.</span></span>
* <span data-ttu-id="1adbb-131">Accettare le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="1adbb-132">Inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="1adbb-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="1adbb-133">Configurare il middleware</span><span class="sxs-lookup"><span data-stu-id="1adbb-133">Configure the middleware</span></span>

<span data-ttu-id="1adbb-134">Aggiungere il middleware degli oggetti WebSocket nel metodo `Configure` della classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1adbb-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1adbb-135">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1adbb-135">The following settings can be configured:</span></span>

* <span data-ttu-id="1adbb-136">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="1adbb-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="1adbb-137">Il valore predefinito è due minuti.</span><span class="sxs-lookup"><span data-stu-id="1adbb-137">The default is two minutes.</span></span>
* <span data-ttu-id="1adbb-138">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="1adbb-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="1adbb-139">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="1adbb-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="1adbb-140">L'impostazione predefinita è 4 KB.</span><span class="sxs-lookup"><span data-stu-id="1adbb-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1adbb-141">È possibile configurare le impostazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1adbb-141">The following settings can be configured:</span></span>

* <span data-ttu-id="1adbb-142">`KeepAliveInterval`: la frequenza di invio di frame "ping" al client per garantire che i proxy tengano aperta la connessione.</span><span class="sxs-lookup"><span data-stu-id="1adbb-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="1adbb-143">Il valore predefinito è due minuti.</span><span class="sxs-lookup"><span data-stu-id="1adbb-143">The default is two minutes.</span></span>
* <span data-ttu-id="1adbb-144">`ReceiveBufferSize`: le dimensioni del buffer usato per ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="1adbb-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="1adbb-145">Gli utenti avanzati possono avere l'esigenza di modificare questa impostazione per ottimizzare le prestazioni in base alle dimensioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="1adbb-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="1adbb-146">L'impostazione predefinita è 4 KB.</span><span class="sxs-lookup"><span data-stu-id="1adbb-146">The default is 4 KB.</span></span>
* <span data-ttu-id="1adbb-147">`AllowedOrigins`: elenco di valori dell'intestazione Origin consentiti per le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="1adbb-148">Per impostazione predefinita, tutte le origini sono consentite.</span><span class="sxs-lookup"><span data-stu-id="1adbb-148">By default, all origins are allowed.</span></span> <span data-ttu-id="1adbb-149">Per informazioni dettagliate, vedere più avanti "Restrizione per le origini WebSocket".</span><span class="sxs-lookup"><span data-stu-id="1adbb-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="1adbb-150">Accettare le richieste WebSocket</span><span class="sxs-lookup"><span data-stu-id="1adbb-150">Accept WebSocket requests</span></span>

<span data-ttu-id="1adbb-151">In un momento successivo nel ciclo di vita della richiesta, più avanti nel metodo `Configure` o in un'azione MVC, ad esempio, verificare che si tratti di una richiesta WebSocket e accettarla.</span><span class="sxs-lookup"><span data-stu-id="1adbb-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="1adbb-152">L'esempio seguente è tratto da un punto successivo nel metodo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="1adbb-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="1adbb-153">Una richiesta WebSocket può arrivare in qualsiasi URL, ma questo codice di esempio accetta solo le richieste per `/ws`.</span><span class="sxs-lookup"><span data-stu-id="1adbb-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="1adbb-154">Inviare e ricevere messaggi</span><span class="sxs-lookup"><span data-stu-id="1adbb-154">Send and receive messages</span></span>

<span data-ttu-id="1adbb-155">Il metodo `AcceptWebSocketAsync` consente di aggiornare la connessione TCP a una connessione WebSocket e offre un oggetto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="1adbb-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="1adbb-156">Usare l'oggetto `WebSocket` per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="1adbb-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="1adbb-157">Il codice illustrato in precedenza che accetta la richiesta WebSocket passa l'oggetto `WebSocket` a un metodo `Echo`.</span><span class="sxs-lookup"><span data-stu-id="1adbb-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="1adbb-158">Il codice riceve un messaggio e lo reinvia immediatamente.</span><span class="sxs-lookup"><span data-stu-id="1adbb-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="1adbb-159">I messaggi vengono inviati e ricevuti in un ciclo, fino a quando il client non chiude la connessione:</span><span class="sxs-lookup"><span data-stu-id="1adbb-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="1adbb-160">Quando si accetta la connessione WebSocket prima che inizi il ciclo, la pipeline del middleware termina.</span><span class="sxs-lookup"><span data-stu-id="1adbb-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="1adbb-161">Dopo la chiusura del socket, la pipeline si arresta,</span><span class="sxs-lookup"><span data-stu-id="1adbb-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="1adbb-162">In altri termini, quando viene accettato il WebSocket, lo spostamento in avanti della richiesta nella pipeline si interrompe.</span><span class="sxs-lookup"><span data-stu-id="1adbb-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="1adbb-163">Quando il ciclo viene completato e il socket viene chiuso, la richiesta torna ad avanzare nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="1adbb-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="websocket-origin-restriction"></a><span data-ttu-id="1adbb-164">Restrizione per le origini WebSocket</span><span class="sxs-lookup"><span data-stu-id="1adbb-164">WebSocket origin restriction</span></span>

<span data-ttu-id="1adbb-165">La protezione fornita da CORS non si applica agli oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-165">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="1adbb-166">I browser **non**:</span><span class="sxs-lookup"><span data-stu-id="1adbb-166">Browsers do **not**:</span></span>

* <span data-ttu-id="1adbb-167">Eseguono richieste CORS preventive.</span><span class="sxs-lookup"><span data-stu-id="1adbb-167">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="1adbb-168">Rispettano le restrizioni specificate nelle intestazioni `Access-Control` quando eseguono richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-168">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="1adbb-169">I browser, tuttavia, inviano l'intestazione `Origin` quando rilasciano richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-169">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="1adbb-170">Le applicazioni devono essere configurate per la convalida di queste intestazioni per assicurarsi che siano consentiti solo WebSocket provenienti dalle origini previste.</span><span class="sxs-lookup"><span data-stu-id="1adbb-170">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="1adbb-171">Se si ospita il server in "https://server.com" e il client in "https://client.com", aggiungere "https://client.com" all'elenco `AllowedOrigins` per consentire la verifica dei WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-171">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="1adbb-172">L'intestazione `Origin` viene controllata dal client e, come l'intestazione `Referer`, può essere falsificata.</span><span class="sxs-lookup"><span data-stu-id="1adbb-172">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="1adbb-173">**Non** usare queste intestazioni come meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1adbb-173">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="1adbb-174">Supporto di IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="1adbb-174">IIS/IIS Express support</span></span>

<span data-ttu-id="1adbb-175">Windows Server 2012 o versioni successive e Windows 8 o versioni successive con IIS/IIS Express 8 o versioni successive includono il supporto per il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1adbb-175">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="1adbb-176">Gli oggetti WebSocket sono sempre abilitati quando si usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1adbb-176">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="1adbb-177">Abilitazione di oggetti WebSocket in IIS</span><span class="sxs-lookup"><span data-stu-id="1adbb-177">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="1adbb-178">Per abilitare il supporto per il protocollo WebSocket in Windows Server 2012 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="1adbb-178">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="1adbb-179">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="1adbb-179">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="1adbb-180">Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-180">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="1adbb-181">Selezionare **Installazione basata su ruoli o basata su funzionalità**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-181">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="1adbb-182">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-182">Select **Next**.</span></span>
1. <span data-ttu-id="1adbb-183">Selezionare il server appropriato (il server locale è selezionato per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="1adbb-183">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="1adbb-184">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-184">Select **Next**.</span></span>
1. <span data-ttu-id="1adbb-185">Espandere **Server Web (IIS)** nella struttura **Ruoli**, espandere **Server Web**, quindi **Sviluppo applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-185">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="1adbb-186">Selezionare **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-186">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="1adbb-187">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-187">Select **Next**.</span></span>
1. <span data-ttu-id="1adbb-188">Se non sono necessarie le funzionalità aggiuntive, selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-188">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="1adbb-189">Selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-189">Select **Install**.</span></span>
1. <span data-ttu-id="1adbb-190">Al termine dell'installazione, selezionare **Chiudi** per chiudere la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="1adbb-190">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="1adbb-191">Per abilitare il supporto per il protocollo WebSocket in Windows 8 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="1adbb-191">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="1adbb-192">Questi passaggi non sono necessari quando si usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="1adbb-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="1adbb-193">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="1adbb-193">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="1adbb-194">Aprire i nodi seguenti: **Internet Information Services** > **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-194">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="1adbb-195">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-195">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1adbb-196">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="1adbb-196">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="1adbb-197">Disabilitare WebSocket quando si usa socket.io su node.js</span><span class="sxs-lookup"><span data-stu-id="1adbb-197">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="1adbb-198">Se si usa il supporto WebSocket in [socket.io](https://socket.io/) su [Node.js](https://nodejs.org/), disabilitare il modulo WebSocket IIS predefinito mediante l'elemento `webSocket` in *web.config* o in *applicationHost.config*. Se non viene eseguito questo passaggio, il modulo IIS WebSocket prova a gestire le comunicazioni WebSocket anziché Node.js e l'app.</span><span class="sxs-lookup"><span data-stu-id="1adbb-198">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="1adbb-199">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1adbb-199">Next steps</span></span>

<span data-ttu-id="1adbb-200">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) inclusa in questo articolo è un'app echo.</span><span class="sxs-lookup"><span data-stu-id="1adbb-200">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="1adbb-201">Ha una pagina Web che consente la connessione WebSocket e il server reinvia al client tutti i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="1adbb-201">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="1adbb-202">Eseguire l'app da un prompt dei comandi, in quanto non è configurata per l'esecuzione da Visual Studio con IIS Express, quindi passare a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="1adbb-202">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="1adbb-203">Nella parte superiore sinistra della pagina Web viene visualizzato lo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="1adbb-203">The web page shows the connection status in the upper left:</span></span>

![Stato iniziale della pagina Web](websockets/_static/start.png)

<span data-ttu-id="1adbb-205">Selezionare **Connect** (Connetti) per inviare una richiesta WebSocket per l'URL indicato.</span><span class="sxs-lookup"><span data-stu-id="1adbb-205">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="1adbb-206">Immettere un messaggio di prova e selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="1adbb-206">Enter a test message and select **Send**.</span></span> <span data-ttu-id="1adbb-207">Al termine dell'operazione, selezionare **Close Socket** (Chiudi socket).</span><span class="sxs-lookup"><span data-stu-id="1adbb-207">When done, select **Close Socket**.</span></span> <span data-ttu-id="1adbb-208">La sezione **Comminication Log** (Registrazione comunicazione) riporta tutte le azioni di apertura, invio e chiusura nel momento in cui vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="1adbb-208">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Stato iniziale della pagina Web](websockets/_static/end.png)
