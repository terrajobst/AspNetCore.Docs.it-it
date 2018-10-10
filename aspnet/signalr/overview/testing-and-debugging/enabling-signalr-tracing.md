---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Abilitazione della traccia SignalR | Microsoft Docs
author: tfitzmac
description: Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR. Traccia consente di visualizzare informazioni sugli eventi di diagnostica...
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 89b27267bec5edb0692fe75061d08b4688df5a8c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912067"
---
<a name="enabling-signalr-tracing"></a>Abilitazione della traccia di SignalR
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo documento descrive come abilitare e configurare la traccia per client e server di SignalR. Traccia consente di visualizzare informazioni sugli eventi di diagnostica nell'applicazione SignalR.
>
> In questo argomento è stato scritto originariamente da Patrick Fletcher.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - SignalR versione 2
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


Quando la traccia è abilitata, un'applicazione di SignalR crea le voci di log per gli eventi. È possibile registrare eventi da client e server. Sulla connessione dei log del server, provider di scalabilità orizzontale e gli eventi del bus di messaggi di traccia. Per gli eventi di connessione client i log di traccia. In SignalR 2.1 e versioni successive, la traccia nel client registra il contenuto completo dei messaggi di chiamata dell'hub.

## <a name="contents"></a>Sommario

- [Abilitazione della traccia nel server](#server)

    - [La registrazione di eventi server nei file di testo](#server_text)
    - [La registrazione eventi del server nel registro eventi](#server_eventlog)
- [Abilitazione della traccia nel client di .NET (app Desktop di Windows)](#net_client)

    - [La registrazione di eventi di client Desktop per la console di](#desktop_console)
    - [La registrazione di eventi di client Desktop a un file di testo](#desktop_text)
- [Abilitazione delle tracce nei client Windows Phone 8](#phone)

    - [La registrazione di eventi di client Windows Phone l'interfaccia utente](#phone_ui)
    - [La registrazione di eventi di client Windows Phone per la console di debug](#phone_debug)
- [Abilitazione della traccia nel client JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Abilitazione della traccia nel server

Abilitare la traccia nel server all'interno di file di configurazione dell'applicazione (app. config o Web. config a seconda del tipo di progetto.) Specificare quali categorie di eventi che si desidera registrare. Nel file di configurazione, inoltre specificare se registrare gli eventi a un file di testo, il registro eventi di Windows o un log personalizzato utilizzando un'implementazione di [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Le categorie di eventi server includono i seguenti tipi di messaggi:

| Origine | Messages |
| --- | --- |
| SignalR.SqlMessageBus | Installazione del provider di Bus di messaggi di SQL con scalabilità orizzontale, l'operazione di database, errori e gli eventi di timeout |
| SignalR.ServiceBusMessageBus | Creazione del servizio bus scale-out del provider argomento e sottoscrizione, errore e gli eventi di messaggistica |
| SignalR.RedisMessageBus | Eventi di errore di connessione e disconnessione del provider di scalabilità orizzontale di redis |
| SignalR.ScaleoutMessageBus | Eventi di messaggistica con scalabilità orizzontale |
| SignalR.Transports.WebSocketTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto WebSocket |
| SignalR.Transports.ServerSentEventsTransport | Gli eventi di connessione, disconnessione, messaggistica e di errore di trasporto ServerSentEvents |
| SignalR.Transports.ForeverFrameTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto ForeverFrame |
| SignalR.Transports.LongPollingTransport | Eventi di connessione, disconnessione, messaggistica e di errore di trasporto LongPolling |
| SignalR.Transports.TransportHeartBeat | Il trasporto di eventi di connessione, disconnessione e keepalive |
| SignalR.ReflectedHubDescriptorProvider | Eventi di individuazione dell'hub |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>La registrazione di eventi server nei file di testo

Il codice seguente viene illustrato come abilitare la traccia per ogni categoria di eventi. In questo esempio configura l'applicazione per registrare gli eventi nei file di testo.

**Codice XML del server per abilitare la traccia**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Nel codice precedente, il `SignalRSwitch` voce specifica la [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usata per gli eventi inviati a log specificato. In questo caso, impostarlo su `Verbose` che significa che tutti i messaggi della tracciatura e debug vengono registrati.

L'output seguente mostra le voci dal `transports.log.txt` file per un'applicazione tramite il file di configurazione precedente. Mostra una nuova connessione, una connessione rimossa e gli eventi heartbeat di trasporto.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>La registrazione eventi del server nel registro eventi

Per inserire eventi nel registro eventi anziché da un file di testo, modificare i valori per le voci di `sharedListeners` nodo. Il codice seguente mostra come registrare eventi del server nel registro eventi:

**Codice XML del server per la registrazione degli eventi nel registro eventi**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Gli eventi vengono registrati nel registro applicazioni di e sono disponibili tramite il Visualizzatore eventi, come illustrato di seguito:

![Visualizzatore eventi con i log di SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Quando si utilizza il registro eventi, impostare il **TraceLevel** al **errore** per mantenere gestibile il numero di messaggi.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Abilitazione della traccia nel client di .NET (app Desktop di Windows)

Il client .NET possa registrare gli eventi nella console, un file di testo o in un registro personalizzato utilizzando un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Per abilitare la registrazione nel client .NET, impostare la connessione `TraceLevel` proprietà per un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valore e il `TraceWriter` proprietà su un valore valido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) istanza.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>La registrazione di eventi di client Desktop per la console di

Il codice c# seguente mostra come registrare gli eventi nel client .NET per la console:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>La registrazione di eventi di client Desktop a un file di testo

Il codice c# seguente mostra come registrare gli eventi nel client .NET per un file di testo:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

L'output seguente mostra le voci dal `ClientLog.txt` file per un'applicazione tramite il file di configurazione precedente. Mostra i client che si connette al server e l'hub si richiama un metodo client chiamato `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Abilitazione delle tracce nei client Windows Phone 8

Applicazioni di SignalR per le app di Windows Phone usano lo stesso client .NET come le app desktop, ma [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e la scrittura in un file con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) non sono disponibili. In alternativa, è necessario creare un'implementazione personalizzata di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) per la traccia.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>La registrazione di eventi di client Windows Phone l'interfaccia utente

Il [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) include un esempio di Windows Phone che scrive l'output di traccia in un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un oggetto personalizzato [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) chiamato implementazione `TextBlockWriter`. Questa classe è reperibile nella **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** progetto. Quando si crea un'istanza di `TextBlockWriter`, passare nell'attuale [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)e un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) in cui verrà creato un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) da utilizzare per la traccia output:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Verrà quindi scritto l'output di traccia in una nuova [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creato nel [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) è passato:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>La registrazione di eventi di client Windows Phone per la console di debug

Per inviare l'output di console di debug anziché l'interfaccia utente, creare un'implementazione di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) che scrive nella finestra di debug e assegnarlo per la connessione [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) proprietà:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informazioni di traccia verranno quindi scritte alla finestra di debug in Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Abilitazione della traccia nel client JavaScript

Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.

**Codice JavaScript client per abilitare la traccia per la console del browser (con il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Codice JavaScript client per abilitare la traccia per la console del browser (senza il proxy generato)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Quando la traccia è abilitata, il client JavaScript registra gli eventi per la console del browser. Per accedere alla console del browser, vedere [trasporti monitoraggio](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Lo screenshot seguente mostra un client JavaScript di SignalR con traccia attivata. Connessione e gli eventi di chiamata dell'hub viene visualizzato nella console del browser:

![Eventi di traccia SignalR nella console del browser](enabling-signalr-tracing/_static/image4.png)
