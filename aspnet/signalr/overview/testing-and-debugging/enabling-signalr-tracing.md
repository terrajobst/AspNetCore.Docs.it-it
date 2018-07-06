---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Abilitazione della traccia SignalR | Microsoft Docs
author: tfitzmac
description: Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR. Traccia consente di visualizzare informazioni sugli eventi di diagnostica...
ms.author: aspnetcontent
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 3bc8e4c81a6efa1b02d2b3e633ddb92c8ff6c2e3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809197"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="b4b60-104">Abilitazione della traccia di SignalR</span><span class="sxs-lookup"><span data-stu-id="b4b60-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="b4b60-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b4b60-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b4b60-106">Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b4b60-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="b4b60-107">Traccia consente di visualizzare informazioni sugli eventi di diagnostica nell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="b4b60-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="b4b60-108">In questo argomento è stato scritto originariamente da Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="b4b60-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b4b60-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b4b60-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b4b60-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b4b60-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b4b60-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="b4b60-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="b4b60-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="b4b60-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b4b60-113">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="b4b60-113">Questions and comments</span></span>
> 
> <span data-ttu-id="b4b60-114">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="b4b60-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b4b60-115">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b4b60-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="b4b60-116">Quando la traccia è abilitata, un'applicazione di SignalR crea le voci di log per gli eventi.</span><span class="sxs-lookup"><span data-stu-id="b4b60-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="b4b60-117">È possibile registrare eventi da client e server.</span><span class="sxs-lookup"><span data-stu-id="b4b60-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="b4b60-118">Sulla connessione dei log del server, provider di scalabilità orizzontale e gli eventi del bus di messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="b4b60-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="b4b60-119">Per gli eventi di connessione client i log di traccia.</span><span class="sxs-lookup"><span data-stu-id="b4b60-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="b4b60-120">In SignalR 2.1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b4b60-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="b4b60-121">Sommario</span><span class="sxs-lookup"><span data-stu-id="b4b60-121">Contents</span></span>

- [<span data-ttu-id="b4b60-122">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="b4b60-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="b4b60-123">La registrazione di eventi server nei file di testo</span><span class="sxs-lookup"><span data-stu-id="b4b60-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="b4b60-124">La registrazione eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="b4b60-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="b4b60-125">Abilitazione della traccia nel client di .NET (app Desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="b4b60-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="b4b60-126">La registrazione di eventi di client Desktop per la console di</span><span class="sxs-lookup"><span data-stu-id="b4b60-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="b4b60-127">La registrazione di eventi di client Desktop a un file di testo</span><span class="sxs-lookup"><span data-stu-id="b4b60-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="b4b60-128">Abilitazione delle tracce nei client Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="b4b60-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="b4b60-129">La registrazione di eventi di client Windows Phone l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="b4b60-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="b4b60-130">La registrazione di eventi di client Windows Phone per la console di debug</span><span class="sxs-lookup"><span data-stu-id="b4b60-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="b4b60-131">Abilitazione della traccia nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b4b60-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="b4b60-132">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="b4b60-132">Enabling tracing on the server</span></span>

<span data-ttu-id="b4b60-133">Abilitare la traccia nel server all'interno di file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto.) Specificare quali categorie di eventi che si desidera registrare.</span><span class="sxs-lookup"><span data-stu-id="b4b60-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="b4b60-134">Nel file di configurazione, inoltre specificare se registrare gli eventi a un file di testo, il registro eventi di Windows o un log personalizzato utilizzando un'implementazione di [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="b4b60-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="b4b60-135">Le categorie di eventi server includono i seguenti tipi di messaggi:</span><span class="sxs-lookup"><span data-stu-id="b4b60-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="b4b60-136">Origine</span><span class="sxs-lookup"><span data-stu-id="b4b60-136">Source</span></span> | <span data-ttu-id="b4b60-137">Messages</span><span class="sxs-lookup"><span data-stu-id="b4b60-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="b4b60-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="b4b60-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="b4b60-139">Installazione del provider di Bus di messaggi di SQL con scalabilità orizzontale, l'operazione di database, errori e gli eventi di timeout</span><span class="sxs-lookup"><span data-stu-id="b4b60-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="b4b60-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="b4b60-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="b4b60-141">Creazione del servizio bus scale-out del provider argomento e sottoscrizione, errore e gli eventi di messaggistica</span><span class="sxs-lookup"><span data-stu-id="b4b60-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="b4b60-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="b4b60-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="b4b60-143">Eventi di errore di connessione e disconnessione del provider di scalabilità orizzontale di redis</span><span class="sxs-lookup"><span data-stu-id="b4b60-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="b4b60-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="b4b60-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="b4b60-145">Eventi di messaggistica con scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="b4b60-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="b4b60-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="b4b60-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="b4b60-147">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto WebSocket</span><span class="sxs-lookup"><span data-stu-id="b4b60-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b4b60-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="b4b60-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="b4b60-149">Gli eventi di connessione, disconnessione, messaggistica e di errore di trasporto ServerSentEvents</span><span class="sxs-lookup"><span data-stu-id="b4b60-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b4b60-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="b4b60-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="b4b60-151">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="b4b60-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b4b60-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="b4b60-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="b4b60-153">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto LongPolling</span><span class="sxs-lookup"><span data-stu-id="b4b60-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="b4b60-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="b4b60-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="b4b60-155">Il trasporto di eventi di connessione, disconnessione e keepalive</span><span class="sxs-lookup"><span data-stu-id="b4b60-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="b4b60-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="b4b60-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="b4b60-157">Eventi di individuazione dell'hub</span><span class="sxs-lookup"><span data-stu-id="b4b60-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="b4b60-158">La registrazione di eventi server nei file di testo</span><span class="sxs-lookup"><span data-stu-id="b4b60-158">Logging server events to text files</span></span>

<span data-ttu-id="b4b60-159">Il codice seguente viene illustrato come abilitare la traccia per ogni categoria di eventi.</span><span class="sxs-lookup"><span data-stu-id="b4b60-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="b4b60-160">In questo esempio configura l'applicazione per registrare gli eventi nei file di testo.</span><span class="sxs-lookup"><span data-stu-id="b4b60-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="b4b60-161">**Codice XML del server per abilitare la traccia**</span><span class="sxs-lookup"><span data-stu-id="b4b60-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="b4b60-162">Nel codice precedente, il `SignalRSwitch` voce specifica la [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usata per gli eventi inviati a log specificato.</span><span class="sxs-lookup"><span data-stu-id="b4b60-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="b4b60-163">In questo caso, impostarlo su `Verbose` che significa che tutti i messaggi della tracciatura e debug vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="b4b60-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="b4b60-164">L'output seguente mostra le voci dal `transports.log.txt` file per un'applicazione tramite il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="b4b60-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="b4b60-165">Mostra una nuova connessione, una connessione rimossa e gli eventi heartbeat di trasporto.</span><span class="sxs-lookup"><span data-stu-id="b4b60-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="b4b60-166">La registrazione eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="b4b60-166">Logging server events to the event log</span></span>

<span data-ttu-id="b4b60-167">Per inserire eventi nel registro eventi anziché da un file di testo, modificare i valori per le voci di `sharedListeners` nodo.</span><span class="sxs-lookup"><span data-stu-id="b4b60-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="b4b60-168">Il codice seguente mostra come registrare eventi del server nel registro eventi:</span><span class="sxs-lookup"><span data-stu-id="b4b60-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="b4b60-169">**Codice XML del server per la registrazione degli eventi nel registro eventi**</span><span class="sxs-lookup"><span data-stu-id="b4b60-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="b4b60-170">Gli eventi vengono registrati nel registro applicazioni di e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4b60-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Visualizzatore eventi con i log di SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="b4b60-172">Quando si utilizza il registro eventi, impostare il **TraceLevel** al **errore** per mantenere gestibile il numero di messaggi.</span><span class="sxs-lookup"><span data-stu-id="b4b60-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="b4b60-173">Abilitazione della traccia nel client di .NET (app Desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="b4b60-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="b4b60-174">Il client .NET possa registrare gli eventi nella console, un file di testo o in un registro personalizzato utilizzando un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4b60-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="b4b60-175">Per abilitare la registrazione nel client .NET, impostare la connessione `TraceLevel` proprietà per un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valore e il `TraceWriter` proprietà su un valore valido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) istanza.</span><span class="sxs-lookup"><span data-stu-id="b4b60-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="b4b60-176">La registrazione di eventi di client Desktop per la console di</span><span class="sxs-lookup"><span data-stu-id="b4b60-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="b4b60-177">Il codice c# seguente mostra come registrare gli eventi nel client .NET per la console:</span><span class="sxs-lookup"><span data-stu-id="b4b60-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="b4b60-178">La registrazione di eventi di client Desktop a un file di testo</span><span class="sxs-lookup"><span data-stu-id="b4b60-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="b4b60-179">Il codice c# seguente mostra come registrare gli eventi nel client .NET per un file di testo:</span><span class="sxs-lookup"><span data-stu-id="b4b60-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="b4b60-180">L'output seguente mostra le voci dal `ClientLog.txt` file per un'applicazione tramite il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="b4b60-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="b4b60-181">Mostra i client che si connette al server e l'hub si richiama un metodo client chiamato `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="b4b60-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="b4b60-182">Abilitazione delle tracce nei client Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="b4b60-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="b4b60-183">Applicazioni di SignalR per le app di Windows Phone usano lo stesso client .NET come le app desktop, ma [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="b4b60-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="b4b60-184">In alternativa, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia.</span><span class="sxs-lookup"><span data-stu-id="b4b60-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="b4b60-185">La registrazione di eventi di client Windows Phone l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="b4b60-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="b4b60-186">Il [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) include un esempio di Windows Phone che scrive l'output di traccia in un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un oggetto personalizzato [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) chiamato implementazione `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="b4b60-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="b4b60-187">Questa classe è reperibile nella **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** progetto.</span><span class="sxs-lookup"><span data-stu-id="b4b60-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="b4b60-188">Quando si crea un'istanza di `TextBlockWriter`, passare nell'attuale [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)e un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da utilizzare per la traccia output:</span><span class="sxs-lookup"><span data-stu-id="b4b60-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="b4b60-189">Verrà quindi scritto l'output di traccia in una nuova [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creato nel [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) è passato:</span><span class="sxs-lookup"><span data-stu-id="b4b60-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="b4b60-190">La registrazione di eventi di client Windows Phone per la console di debug</span><span class="sxs-lookup"><span data-stu-id="b4b60-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="b4b60-191">Per inviare l'output di console di debug anziché l'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarlo per la connessione [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) proprietà:</span><span class="sxs-lookup"><span data-stu-id="b4b60-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="b4b60-192">Informazioni di traccia verranno quindi scritte alla finestra di debug in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b4b60-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="b4b60-193">Abilitazione della traccia nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b4b60-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="b4b60-194">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="b4b60-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="b4b60-195">**Codice JavaScript client per abilitare la traccia per la console del browser (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="b4b60-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="b4b60-196">**Codice JavaScript client per abilitare la traccia per la console del browser (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="b4b60-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="b4b60-197">Quando la traccia è abilitata, il client JavaScript registra gli eventi per la console del browser.</span><span class="sxs-lookup"><span data-stu-id="b4b60-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="b4b60-198">Per accedere alla console del browser, vedere [trasporti monitoraggio](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="b4b60-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="b4b60-199">Lo screenshot seguente mostra un client JavaScript di SignalR con traccia attivata.</span><span class="sxs-lookup"><span data-stu-id="b4b60-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="b4b60-200">Connessione e gli eventi di chiamata dell'hub viene visualizzato nella console del browser:</span><span class="sxs-lookup"><span data-stu-id="b4b60-200">It shows connection and hub invocation events in the browser console:</span></span>

![Eventi di traccia SignalR nella console del browser](enabling-signalr-tracing/_static/image4.png)
