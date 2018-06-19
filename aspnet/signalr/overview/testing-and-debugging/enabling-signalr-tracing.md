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
<a name="enabling-signalr-tracing"></a>Abilitazione della traccia SignalR
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR. Traccia consente di visualizzare informazioni sugli eventi di diagnostica nell'applicazione SignalR.
> 
> In questo argomento è stato scritto originariamente dal Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Quando la traccia è abilitata, un'applicazione di SignalR crea voci del registro eventi. È possibile registrare eventi da client e il server. Sulla connessione del server di log, provider di scalabilità orizzontale e gli eventi di bus di messaggi di traccia. Per gli eventi di connessione client registri di traccia. In SignalR 2.1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata di hub.

## <a name="contents"></a>Sommario

- [Abilitazione della traccia nel server](#server)

    - [Registrazione di eventi server per i file di testo](#server_text)
    - [Registrazione di eventi del server nel registro eventi](#server_eventlog)
- [Abilitazione della traccia nel client di .NET (applicazioni Desktop di Windows)](#net_client)

    - [Registrazione di eventi client Desktop per la console di](#desktop_console)
    - [Registrazione di eventi client Desktop in un file di testo](#desktop_text)
- [Abilitazione della traccia nel client di Windows Phone 8](#phone)

    - [Registrazione di eventi client di Windows Phone l'interfaccia utente](#phone_ui)
    - [Registrazione eventi di Windows Phone client nella console di debug](#phone_debug)
- [Abilitazione della traccia nel client di JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Abilitazione della traccia nel server

Abilitare la traccia nel server in file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto.) Specificare le categorie di eventi che si desidera registrare. Nel file di configurazione, inoltre specificare se registrare gli eventi per un file di testo, il registro eventi di Windows o un log personalizzato usando un'implementazione del [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Le categorie di eventi server includono i seguenti ordinamenti di messaggi:

| Origine | Messages |
| --- | --- |
| SignalR.SqlMessageBus | Installazione del provider SQL Bus di messaggi di scalabilità orizzontale, l'operazione di database, errore e gli eventi di timeout |
| SignalR.ServiceBusMessageBus | Creazione del servizio bus di scalabilità orizzontale provider argomento e sottoscrizione, segnalazione errori e gli eventi di messaggistica |
| SignalR.RedisMessageBus | Eventi di connessione, disconnessione e di errore del provider di scalabilità orizzontale di redis |
| SignalR.ScaleoutMessageBus | Eventi di messaggistica di scalabilità orizzontale |
| SignalR.Transports.WebSocketTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto WebSocket |
| SignalR.Transports.ServerSentEventsTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ServerSentEvents |
| SignalR.Transports.ForeverFrameTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ForeverFrame |
| SignalR.Transports.LongPollingTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto LongPolling |
| SignalR.Transports.TransportHeartBeat | Eventi di connessione e disconnessione keepalive di trasporto |
| SignalR.ReflectedHubDescriptorProvider | Eventi di individuazione di hub |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registrazione di eventi server per i file di testo

Il codice seguente viene illustrato come abilitare la traccia per ogni categoria di eventi. In questo esempio consente di configurare l'applicazione per registrare gli eventi in file di testo.

**Codice XML del server per l'abilitazione della traccia**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Nel codice precedente, il `SignalRSwitch` voce specifica di [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilizzati per gli eventi inviati al Registro di specificato. In questo caso, è impostata su `Verbose` ovvero tutti i messaggi di analisi e di debug vengono registrati.

L'output seguente mostra le voci di `transports.log.txt` file per un'applicazione tramite il file di configurazione precedente. Mostra una nuova connessione, una connessione rimossa ed eventi di heartbeat di trasporto.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registrazione di eventi del server nel registro eventi

Per registrare gli eventi nel registro eventi anziché un file di testo, modificare i valori per le voci di `sharedListeners` nodo. Il codice seguente viene illustrato come registrare gli eventi di server nel registro eventi:

**Codice XML del server per la registrazione degli eventi nel registro eventi**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Gli eventi vengono registrati nel registro dell'applicazione e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:

![Visualizzatore eventi visualizzati registri SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Quando si utilizza il registro eventi, impostare il **TraceLevel** a **errore** per mantenere gestibile il numero di messaggi.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Abilitazione della traccia nel client di .NET (applicazioni Desktop di Windows)

Il client .NET è possibile registrare eventi nella console, un file di testo o in un log personalizzato usando un'implementazione del [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Per abilitare la registrazione del client .NET, impostare la connessione `TraceLevel` proprietà per un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valore e `TraceWriter` proprietà su un valore valido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) istanza.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registrazione di eventi client Desktop per la console di

Il codice c# seguente viene illustrato come registrare gli eventi nel client .NET per la console:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registrazione di eventi client Desktop in un file di testo

Il codice c# seguente viene illustrato come registrare gli eventi nel client .NET per un file di testo:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

L'output seguente mostra le voci di `ClientLog.txt` file per un'applicazione tramite il file di configurazione precedente. Viene illustrato il client che si connette al server e l'hub di richiamare un metodo client chiamato `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Abilitazione della traccia nel client di Windows Phone 8

Applicazioni di SignalR per le app di Windows Phone utilizzano lo stesso client .NET come App desktop, ma [console](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili. In alternativa, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registrazione di eventi client di Windows Phone l'interfaccia utente

Il [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) è incluso un esempio di Windows Phone che scrive l'output di traccia a un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mediante un oggetto personalizzato [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) chiamato implementazione `TextBlockWriter`. Questa classe è reperibile nel **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** progetto. Quando si crea un'istanza di `TextBlockWriter`, passare l'oggetto corrente [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)e un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da utilizzare per la traccia output:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Verrà quindi scritto l'output di traccia a un nuovo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creati nel [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) è passato:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registrazione eventi di Windows Phone client nella console di debug

Per inviare l'output di console di debug anziché l'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarlo per la connessione [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) proprietà:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informazioni di traccia verranno quindi scritte nella finestra di debug in Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Abilitazione della traccia nel client di JavaScript

Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà sull'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.

**Codice client JavaScript per abilitare la traccia per la console del browser (con il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Codice client JavaScript per abilitare la traccia per la console del browser (senza il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Quando la traccia è abilitata, il client JavaScript registra gli eventi per la console del browser. Per accedere alla console del browser, vedere [monitoraggio trasporti](../getting-started/introduction-to-signalr.md#MonitoringTransports).

La schermata seguente mostra un client SignalR JavaScript con la traccia attivata. Mostra eventi di chiamata di hub e di connessione nella console di browser:

![Eventi di traccia SignalR nella console di browser](enabling-signalr-tracing/_static/image4.png)
