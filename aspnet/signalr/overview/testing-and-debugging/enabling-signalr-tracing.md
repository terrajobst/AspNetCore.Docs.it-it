---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Abilitazione della traccia SignalR | Documenti Microsoft
author: tfitzmac
description: Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR. Traccia consente di visualizzare informazioni sugli eventi di diagnostica...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032818"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="8bcfd-104">Abilitazione della traccia SignalR</span><span class="sxs-lookup"><span data-stu-id="8bcfd-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="8bcfd-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8bcfd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8bcfd-106">Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="8bcfd-107">Traccia consente di visualizzare informazioni sugli eventi di diagnostica nell'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="8bcfd-108">In questo argomento è stato scritto originariamente dal Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8bcfd-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="8bcfd-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="8bcfd-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8bcfd-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8bcfd-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="8bcfd-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="8bcfd-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="8bcfd-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8bcfd-113">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="8bcfd-113">Questions and comments</span></span>
> 
> <span data-ttu-id="8bcfd-114">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8bcfd-115">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8bcfd-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="8bcfd-116">Quando la traccia è abilitata, un'applicazione di SignalR crea voci del registro eventi.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="8bcfd-117">È possibile registrare eventi da client e il server.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="8bcfd-118">Sulla connessione del server di log, provider di scalabilità orizzontale e gli eventi di bus di messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="8bcfd-119">Per gli eventi di connessione client registri di traccia.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="8bcfd-120">In SignalR 2.1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata di hub.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="8bcfd-121">Sommario</span><span class="sxs-lookup"><span data-stu-id="8bcfd-121">Contents</span></span>

- [<span data-ttu-id="8bcfd-122">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="8bcfd-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="8bcfd-123">Registrazione di eventi server per i file di testo</span><span class="sxs-lookup"><span data-stu-id="8bcfd-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="8bcfd-124">Registrazione di eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="8bcfd-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="8bcfd-125">Abilitazione della traccia nel client di .NET (applicazioni Desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="8bcfd-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="8bcfd-126">Registrazione di eventi client Desktop per la console di</span><span class="sxs-lookup"><span data-stu-id="8bcfd-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="8bcfd-127">Registrazione di eventi client Desktop in un file di testo</span><span class="sxs-lookup"><span data-stu-id="8bcfd-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="8bcfd-128">Abilitazione della traccia nel client di Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="8bcfd-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="8bcfd-129">Registrazione di eventi client di Windows Phone l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="8bcfd-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="8bcfd-130">Registrazione eventi di Windows Phone client nella console di debug</span><span class="sxs-lookup"><span data-stu-id="8bcfd-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="8bcfd-131">Abilitazione della traccia nel client di JavaScript</span><span class="sxs-lookup"><span data-stu-id="8bcfd-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="8bcfd-132">Abilitazione della traccia nel server</span><span class="sxs-lookup"><span data-stu-id="8bcfd-132">Enabling tracing on the server</span></span>

<span data-ttu-id="8bcfd-133">Abilitare la traccia nel server in file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto.) Specificare le categorie di eventi che si desidera registrare.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="8bcfd-134">Nel file di configurazione, inoltre specificare se registrare gli eventi per un file di testo, il registro eventi di Windows o un log personalizzato usando un'implementazione del [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="8bcfd-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="8bcfd-135">Le categorie di eventi server includono i seguenti ordinamenti di messaggi:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="8bcfd-136">Origine</span><span class="sxs-lookup"><span data-stu-id="8bcfd-136">Source</span></span> | <span data-ttu-id="8bcfd-137">Messages</span><span class="sxs-lookup"><span data-stu-id="8bcfd-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="8bcfd-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="8bcfd-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="8bcfd-139">Installazione del provider SQL Bus di messaggi di scalabilità orizzontale, l'operazione di database, errore e gli eventi di timeout</span><span class="sxs-lookup"><span data-stu-id="8bcfd-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="8bcfd-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="8bcfd-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="8bcfd-141">Creazione del servizio bus di scalabilità orizzontale provider argomento e sottoscrizione, segnalazione errori e gli eventi di messaggistica</span><span class="sxs-lookup"><span data-stu-id="8bcfd-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="8bcfd-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="8bcfd-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="8bcfd-143">Eventi di connessione, disconnessione e di errore del provider di scalabilità orizzontale di redis</span><span class="sxs-lookup"><span data-stu-id="8bcfd-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="8bcfd-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="8bcfd-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="8bcfd-145">Eventi di messaggistica di scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="8bcfd-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="8bcfd-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="8bcfd-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="8bcfd-147">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto WebSocket</span><span class="sxs-lookup"><span data-stu-id="8bcfd-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="8bcfd-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="8bcfd-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="8bcfd-149">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ServerSentEvents</span><span class="sxs-lookup"><span data-stu-id="8bcfd-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="8bcfd-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="8bcfd-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="8bcfd-151">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="8bcfd-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="8bcfd-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="8bcfd-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="8bcfd-153">Eventi di connessione, disconnessione, messaggistica e di errore di trasporto LongPolling</span><span class="sxs-lookup"><span data-stu-id="8bcfd-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="8bcfd-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="8bcfd-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="8bcfd-155">Eventi di connessione e disconnessione keepalive di trasporto</span><span class="sxs-lookup"><span data-stu-id="8bcfd-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="8bcfd-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="8bcfd-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="8bcfd-157">Eventi di individuazione di hub</span><span class="sxs-lookup"><span data-stu-id="8bcfd-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="8bcfd-158">Registrazione di eventi server per i file di testo</span><span class="sxs-lookup"><span data-stu-id="8bcfd-158">Logging server events to text files</span></span>

<span data-ttu-id="8bcfd-159">Il codice seguente viene illustrato come abilitare la traccia per ogni categoria di eventi.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="8bcfd-160">In questo esempio consente di configurare l'applicazione per registrare gli eventi in file di testo.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="8bcfd-161">**Codice XML del server per l'abilitazione della traccia**</span><span class="sxs-lookup"><span data-stu-id="8bcfd-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="8bcfd-162">Nel codice precedente, il `SignalRSwitch` voce specifica di [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilizzati per gli eventi inviati al Registro di specificato.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="8bcfd-163">In questo caso, è impostata su `Verbose` ovvero tutti i messaggi di analisi e di debug vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="8bcfd-164">L'output seguente mostra le voci di `transports.log.txt` file per un'applicazione tramite il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="8bcfd-165">Mostra una nuova connessione, una connessione rimossa ed eventi di heartbeat di trasporto.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="8bcfd-166">Registrazione di eventi del server nel registro eventi</span><span class="sxs-lookup"><span data-stu-id="8bcfd-166">Logging server events to the event log</span></span>

<span data-ttu-id="8bcfd-167">Per registrare gli eventi nel registro eventi anziché un file di testo, modificare i valori per le voci di `sharedListeners` nodo.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="8bcfd-168">Il codice seguente viene illustrato come registrare gli eventi di server nel registro eventi:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="8bcfd-169">**Codice XML del server per la registrazione degli eventi nel registro eventi**</span><span class="sxs-lookup"><span data-stu-id="8bcfd-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="8bcfd-170">Gli eventi vengono registrati nel registro dell'applicazione e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Visualizzatore eventi visualizzati registri SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="8bcfd-172">Quando si utilizza il registro eventi, impostare il **TraceLevel** a **errore** per mantenere gestibile il numero di messaggi.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="8bcfd-173">Abilitazione della traccia nel client di .NET (applicazioni Desktop di Windows)</span><span class="sxs-lookup"><span data-stu-id="8bcfd-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="8bcfd-174">Il client .NET è possibile registrare eventi nella console, un file di testo o in un log personalizzato usando un'implementazione del [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bcfd-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="8bcfd-175">Per abilitare la registrazione del client .NET, impostare la connessione `TraceLevel` proprietà per un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valore e `TraceWriter` proprietà su un valore valido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) istanza.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="8bcfd-176">Registrazione di eventi client Desktop per la console di</span><span class="sxs-lookup"><span data-stu-id="8bcfd-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="8bcfd-177">Il codice c# seguente viene illustrato come registrare gli eventi nel client .NET per la console:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="8bcfd-178">Registrazione di eventi client Desktop in un file di testo</span><span class="sxs-lookup"><span data-stu-id="8bcfd-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="8bcfd-179">Il codice c# seguente viene illustrato come registrare gli eventi nel client .NET per un file di testo:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="8bcfd-180">L'output seguente mostra le voci di `ClientLog.txt` file per un'applicazione tramite il file di configurazione precedente.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="8bcfd-181">Viene illustrato il client che si connette al server e l'hub di richiamare un metodo client chiamato `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="8bcfd-182">Abilitazione della traccia nel client di Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="8bcfd-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="8bcfd-183">Applicazioni di SignalR per le app di Windows Phone utilizzano lo stesso client .NET come App desktop, ma [console](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="8bcfd-184">In alternativa, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="8bcfd-185">Registrazione di eventi client di Windows Phone l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="8bcfd-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="8bcfd-186">Il [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) è incluso un esempio di Windows Phone che scrive l'output di traccia a un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mediante un oggetto personalizzato [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) chiamato implementazione `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="8bcfd-187">Questa classe è reperibile nel **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** progetto.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="8bcfd-188">Quando si crea un'istanza di `TextBlockWriter`, passare l'oggetto corrente [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)e un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da utilizzare per la traccia output:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="8bcfd-189">Verrà quindi scritto l'output di traccia a un nuovo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creati nel [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) è passato:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="8bcfd-190">Registrazione eventi di Windows Phone client nella console di debug</span><span class="sxs-lookup"><span data-stu-id="8bcfd-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="8bcfd-191">Per inviare l'output di console di debug anziché l'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarlo per la connessione [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) proprietà:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="8bcfd-192">Informazioni di traccia verranno quindi scritte nella finestra di debug in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="8bcfd-193">Abilitazione della traccia nel client di JavaScript</span><span class="sxs-lookup"><span data-stu-id="8bcfd-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="8bcfd-194">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà sull'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="8bcfd-195">**Codice client JavaScript per abilitare la traccia per la console del browser (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="8bcfd-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="8bcfd-196">**Codice client JavaScript per abilitare la traccia per la console del browser (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="8bcfd-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="8bcfd-197">Quando la traccia è abilitata, il client JavaScript registra gli eventi per la console del browser.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="8bcfd-198">Per accedere alla console del browser, vedere [monitoraggio trasporti](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="8bcfd-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="8bcfd-199">La schermata seguente mostra un client SignalR JavaScript con la traccia attivata.</span><span class="sxs-lookup"><span data-stu-id="8bcfd-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="8bcfd-200">Mostra eventi di chiamata di hub e di connessione nella console di browser:</span><span class="sxs-lookup"><span data-stu-id="8bcfd-200">It shows connection and hub invocation events in the browser console:</span></span>

![Eventi di traccia SignalR nella console di browser](enabling-signalr-tracing/_static/image4.png)
