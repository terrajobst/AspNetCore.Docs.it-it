---
title: Registrazione e diagnostica in ASP.NET Core SignalR
author: anurse
description: Informazioni su come raccogliere dati diagnostici dalle app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896888"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Registrazione e diagnostica in ASP.NET Core SignalR

Da [Andrew Stanton-Nurse](https://twitter.com/anurse)

Questo articolo fornisce indicazioni per la raccolta di diagnostica dalla propria app ASP.NET Core SignalR per risolvere i problemi.

## <a name="server-side-logging"></a>Registrazione sul lato server

> [!WARNING]
> File di log lato server possono contenere informazioni riservate dall'app. **Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.

Poiché SignalR fa parte di ASP.NET Core, Usa il sistema di registrazione di ASP.NET Core. Nella configurazione predefinita, SignalR registra informazioni molto piccolo, ma questo può configurato. Vedere la documentazione sul [registrazione di ASP.NET Core](xref:fundamentals/logging/index#configuration) per informazioni dettagliate sulla configurazione della registrazione di ASP.NET Core.

SignalR utilizza due categorie di logger:

* `Microsoft.AspNetCore.SignalR` -per i log relativi ai protocolli di Hub, attivando l'hub, richiamo di metodi e le altre attività correlati all'Hub.
* `Microsoft.AspNetCore.Http.Connections` -per i log relativi ai trasporti, ad esempio WebSocket, di Polling lungo Server-Sent eventi e a basso livello infrastruttura SignalR.

Per abilitare i log dettagliati da SignalR, configurare entrambi i prefissi precedenti per il `Debug` a livello del `appsettings.json` aggiungendo quanto segue al file il `LogLevel` sottosezione `Logging`:

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

È anche possibile configurare questo nel codice nel `CreateWebHostBuilder` metodo:

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

Se non si usa la configurazione basata su JSON, impostare i valori di configurazione seguenti nel sistema di configurazione:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

Consultare la documentazione per il sistema di configurazione determinare come specificare i valori di configurazione annidata. Ad esempio, quando si usano le variabili di ambiente, due `_` vengono utilizzati caratteri anziché il `:` (ad esempio: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

È consigliabile usare il `Debug` livello per la raccolta di informazioni dettagliate di diagnostica per l'app. Il `Trace` livello produce diagnostica di livello molto basso e raramente è necessario per diagnosticare i problemi nell'app.

## <a name="access-server-side-logs"></a>Log lato server di accesso

Modalità di accesso a file di log lato server dipende dall'ambiente in cui in esecuzione.

### <a name="as-a-console-app-outside-iis"></a>Come un'app console all'esterno di IIS

Se si esegue in un'app console, il [logger di Console](xref:fundamentals/logging/index#console-provider) deve essere abilitata per impostazione predefinita. SignalR log saranno visualizzati nella console.

### <a name="within-iis-express-from-visual-studio"></a>All'interno di IIS Express da Visual Studio

Viene visualizzato l'output del log in Visual Studio il **Output** finestra. Selezionare il **ASP.NET Core Web Server** elenco a discesa di opzione.

### <a name="azure-app-service"></a>Servizio app di Azure

Abilitare l'opzione "Registrazione applicazioni (file System)" nella sezione "Log di diagnostica" del portale del servizio App di Azure e configurare il livello a `Verbose`. I log devono essere disponibili da "Lo streaming dei Log" anche il servizio, come i log nel file system del servizio App. Per altre informazioni, vedere la documentazione sul [lo streaming dei log di Azure](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Altri ambienti

Se si esegue in un altro ambiente (Docker, Kubernetes, servizio di Windows, e così via), vedere la documentazione completa sul [registrazione di ASP.NET Core](xref:fundamentals/logging/index) per altre informazioni su come configurare i provider di registrazione adatti al proprio ambiente.

## <a name="javascript-client-logging"></a>La registrazione del client JavaScript

> [!WARNING]
> File di log lato client possono contenere informazioni riservate dall'app. **Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.

Quando si usa il client JavaScript, è possibile configurare le opzioni di registrazione usando il `configureLogging` metodo `HubConnectionBuilder`:

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

Per disabilitare completamente la registrazione, specificare `signalR.LogLevel.None` nella `configureLogging` (metodo).

La tabella seguente illustra i livelli di log disponibili per il client JavaScript. Impostazione del livello di log a uno di questi valori consente la registrazione a tale livello e tutti i livelli superiori nella tabella.

| Livello | Descrizione |
| ----- | ----------- |
| `None` | Viene registrato alcun messaggio. |
| `Critical` | Messaggi che indicano un errore nell'intera app. |
| `Error` | Messaggi che indicano un errore dell'operazione corrente. |
| `Warning` | Messaggi che indicano un problema non grave. |
| `Information` | Messaggi informativi. |
| `Debug` | Messaggi di diagnostica utili per il debug. |
| `Trace` | Messaggi di diagnostica molto dettagliati progettati per la diagnosi dei problemi specifici. |

Dopo aver configurato il livello di dettaglio, il log verrà scritto per la Console del Browser (o di Output Standard in un'app NodeJS).

Se si desidera inviare i log a un sistema di registrazione personalizzato, è possibile fornire un oggetto JavaScript che implementa il `ILogger` interfaccia. È l'unico metodo che deve essere implementato `log`, che accetta il livello dell'evento e il messaggio associato all'evento. Ad esempio:

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>La registrazione del client .NET

> [!WARNING]
> File di log lato client possono contenere informazioni riservate dall'app. **Mai** invio di log non elaborati dalle app di produzione ai forum pubblico come GitHub.

Per ottenere i log del client .NET, è possibile usare la `ConfigureLogging` metodo `HubConnectionBuilder`. Questo procedimento funziona esattamente come le `ConfigureLogging` metodo sul `WebHostBuilder` e `HostBuilder`. È possibile configurare è usare gli stessi provider di registrazione in ASP.NET Core. Tuttavia, è necessario installare e abilitare i pacchetti NuGet per il provider di registrazione singola manualmente.

### <a name="console-logging"></a>Registrazione console

Per abilitare la registrazione della Console, aggiungere il [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pacchetto. Usare quindi il `AddConsole` metodo per configurare il logger della console:

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>La registrazione di finestra di output di debug

È anche possibile configurare i log per visualizzare il **Output** finestra in Visual Studio. Installare il [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pacchetto e usare il `AddDebug` metodo:

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Altri provider di registrazione

SignalR supporta altri provider di registrazione, ad esempio Serilog, Seq, NLog o qualsiasi altro sistema di registrazione che si integra con `Microsoft.Extensions.Logging`. Se il sistema di registrazione fornisce un' `ILoggerProvider`, è possibile registrarlo con `AddProvider`:

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Livello di dettaglio di controllo

Se si sta eseguendo l'app da altre posizioni, modificare il livello predefinito da `Debug` potrebbe essere troppo dettagliata. È possibile usare un filtro per configurare il livello di registrazione per i log di SignalR. Questa operazione può essere eseguita nel codice, in gran parte esattamente come nel server:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Tracce di rete

> [!WARNING]
> Una traccia di rete include il contenuto completo di tutti i messaggi inviati dall'app. **Mai** invio tracce di rete non elaborati dalle app di produzione ai forum pubblico come GitHub.

Se si verifica un problema, una traccia di rete in alcuni casi può fornire numerose informazioni utili. Ciò è particolarmente utile se si desidera segnalare un problema in una pagina.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Raccogliere una traccia di rete con Fiddler (opzione consigliata)

Questo metodo funziona per tutte le app.

Fiddler è uno strumento molto potente per raccogliere le tracce HTTP. Installare l'app da [telerik.com/fiddler](https://www.telerik.com/fiddler), avviarla, quindi eseguire l'app e riprodurre il problema. Fiddler è disponibile per Windows e sono disponibili versioni beta per macOS e Linux.

Se ci si connette tramite HTTPS, esistono alcuni passaggi aggiuntivi per garantire che Fiddler in grado di decrittografare il traffico HTTPS. Per altre informazioni, vedere la [documentazione di Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

Dopo aver raccolto la traccia, è possibile esportare la traccia, scegliendo **File** > **salvare** > **tutte le sessioni...**  dalla barra dei menu.

![Esportazione di tutte le sessioni da Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Raccogliere una traccia di rete con tcpdump (macOS e Linux solo)

Questo metodo funziona per tutte le app.

È possibile raccogliere le tracce TCP non elaborate usando tcpdump eseguendo il comando seguente da una shell dei comandi. Potrebbe essere necessario essere `root` o un prefisso con il comando `sudo` se si verifica un errore di autorizzazione:

```console
tcpdump -i [interface] -w trace.pcap
```

Sostituire `[interface]` con l'interfaccia di rete che si desidera acquisire in. Si tratta in genere simile `/dev/eth0` (per l'interfaccia Ethernet standard) o `/dev/lo0` (per il traffico di localhost). Per altre informazioni, vedere il `tcpdump` pagina di manuale nel sistema host.

## <a name="collect-a-network-trace-in-the-browser"></a>Raccogliere una traccia di rete nel browser

Questo metodo funziona solo per le app basate su browser.

La maggior parte degli strumenti di sviluppo del browser aperta una scheda "Rete" che consente di acquisire attività di rete tra il browser e il server. Tuttavia, queste tracce non includono i messaggi di evento Server-Sent e WebSocket. Se si utilizza questi trasporti, usando uno strumento come Fiddler o TcpDump (descritti di seguito) è un approccio migliore.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge e Internet Explorer

(Le istruzioni sono le stesse sia per Microsoft Edge che per Internet Explorer)

1. Premere F12 per aprire gli strumenti di sviluppo
2. Fare clic sulla scheda di rete
3. Aggiornare la pagina, se necessario e riprodurre il problema
4. Fare clic sull'icona Salva nella barra degli strumenti per esportare la traccia come "HAR" file:

![Il salvataggio degli strumenti della scheda di rete sull'icona allo sviluppo di Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Premere F12 per aprire gli strumenti di sviluppo
2. Fare clic sulla scheda di rete
3. Aggiornare la pagina, se necessario e riprodurre il problema
4. Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva come HAR con contenuto":

![Opzione "Salva come HAR con contenuto" nella scheda di rete di Google Chrome Dev Tools](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Premere F12 per aprire gli strumenti di sviluppo
2. Fare clic sulla scheda di rete
3. Aggiornare la pagina, se necessario e riprodurre il problema
4. Fare clic in un punto qualsiasi nell'elenco delle richieste e scegliere "Salva tutto come HAR"

![Opzione "Salva tutto come HAR" nella scheda di rete strumenti di sviluppo di Mozilla Firefox](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>Collegare i file di diagnostica per problemi di GitHub

È possibile collegare i file di diagnostica per problemi di GitHub rinominandoli pertanto hanno un `.txt` estensione e quindi trascinarli e rilasciarli al problema.

> [!NOTE]
> Problema di GitHub, non incollare il contenuto del file di log o le tracce di rete. Questi log e tracce possono essere piuttosto grande e GitHub in genere non visualizzarle.

![Il trascinamento di file di log a un problema di GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
