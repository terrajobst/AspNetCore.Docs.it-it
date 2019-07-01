---
title: Risolvere i problemi di ASP.NET Core in Servizio app di Azure
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni di ASP.NET Core in Servizio App di Azure.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: d78499c1a82a011239f6b62b546f304a5d5017e2
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313750"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Risolvere i problemi di ASP.NET Core in Servizio app di Azure

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core usando gli strumenti di diagnostica di Servizio app di Azure. Per altre informazioni sulla risoluzione dei problemi, vedere [Panoramica della diagnostica del Servizio app di Azure](/azure/app-service/app-service-diagnostics) e [Procedura: Monitorare le app nel Servizio app di Azure](/azure/app-service/web-sites-monitor) nella documentazione di Azure.

Altri argomenti sulla risoluzione dei problemi:

* IIS usa anche il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare app. Per consigli per la risoluzione dei problemi relativi in particolare a IIS, vedere <xref:host-and-deploy/iis/troubleshoot>.
* <xref:fundamentals/error-handling> offre informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.
* [Informazioni su come eseguire il debug con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) presenta le funzionalità del debugger di Visual Studio.
* [Debug con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) descrive il supporto del debug incorporato in Visual Studio Code.

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Per accedere al log eventi dell'applicazione, usare il pannello **Diagnostica e risolvi i problemi** nel portale di Azure:

1. Nel portale di Azure aprire l'app in **Servizi app**.
1. Selezionare **Diagnostica e risolvi i problemi**.
1. Selezionare l'intestazione **Strumenti di diagnostica**.
1. In **Strumenti di supporto** selezionare il pulsante **Eventi dell'applicazione**.
1. Esaminare l'errore più recente dalla voce *IIS AspNetCoreModule* o *IIS AspNetCoreModule V2* nella colonna **Origine**.

Un'alternativa all'uso del pannello **Diagnostica e risolvi i problemi** consiste nell'esaminare direttamente il file del log eventi dell'applicazione usando [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Aprire la cartella **LogFiles**.
1. Selezionare l'icona della matita accanto al file *eventlog.xml*.
1. Esaminare il log. Scorrere fino alla fine del log per visualizzare gli eventi più recenti.

### <a name="run-the-app-in-the-kudu-console"></a>Eseguire l'app nella console Kudu

Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione. È possibile eseguire l'app nella console di esecuzione remota [Kudu](https://github.com/projectkudu/kudu/wiki) per individuare l'errore:

1. Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.

#### <a name="test-a-32-bit-x86-app"></a>Test di un'app a 32 bit (x86)

##### <a name="current-release"></a>Versione corrente

1. `cd d:\home\site\wwwroot`
1. Eseguire l'app:
   * Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):

     ```console
     {ASSEMBLY NAME}.exe
     ```

L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Distribuzione dipendente dal framework in esecuzione in una versione di anteprima

*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x86).*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` è la versione di runtime)
1. Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.

#### <a name="test-a-64-bit-x64-app"></a>Test di un'app a 64 bit (x64)

##### <a name="current-release"></a>Versione corrente

* Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) a 64 bit (x64):
  1. `cd D:\Program Files\dotnet`
  1. Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):
  1. `cd D:\home\site\wwwroot`
  1. Eseguire l'app: `{ASSEMBLY NAME}.exe`

L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Distribuzione dipendente dal framework in esecuzione in una versione di anteprima

*Richiede l'installazione dell'estensione del sito del runtime ASP.NET Core {VERSION} (x64).*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` è la versione di runtime)
1. Eseguire l'app. `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Log stdout del modulo ASP.NET Core

Il log stdout del modulo ASP.NET Core spesso registra utili messaggi di errore non disponibili nel log eventi dell'applicazione. Per abilitare e visualizzare i log stdout:

1. Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.
1. In **SELECT PROBLEM CATEGORY** (SELEZIONARE LA CATEGORIA DEL PROBLEMA) selezionare il pulsante **Web App Down** (App Web inattiva).
1. In **Suggested Solutions** (Soluzioni consigliate)  > **Enable Stdout Log Redirection** (Abilita il reindirizzamento del log Stdout) selezionare il pulsante **Open Kudu Console to edit Web.Config** (Apri la console Kudu per modificare Web.Config).
1. Nella **console diagnostica** Kudu aprire le cartelle nel percorso **site** > **wwwroot**. Scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.
1. Fare clic sull'icona della matita accanto al file *web.config*.
1. Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.
1. Selezionare **Salva** per salvare il file *web.config* aggiornato.
1. Effettuare una richiesta all'app.
1. Tornare al portale di Azure. Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Selezionare la cartella **LogFiles**.
1. Controllare la colonna **Data modifica** e selezionare l'icona della matita per modificare il log stdout con la data di modifica più recente.
1. Quando si apre il file di log, verrà visualizzato l'errore.

Al termine della risoluzione dei problemi, disabilitare la registrazione stdout:

1. Nella **console diagnostica** Kudu tornare al percorso **site** > **wwwroot** per visualizzare il file *web.config*. Aprire nuovamente il file **web.config** selezionando l'icona della matita.
1. Impostare **stdoutLogEnabled** su `false`.
1. Selezionare **Salva** per salvare il file.

> [!WARNING]
> La mancata disabilitazione del log stdout può causare un errore dell'app o del server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare. Usare la registrazione stdout solo per la risoluzione dei problemi di avvio delle app.
>
> Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a>Log di debug del modulo ASP.NET Core

Il log di debug del modulo ASP.NET Core fornisce dati di registrazione aggiuntivi e più approfonditi dal modulo ASP.NET Core. Per abilitare e visualizzare i log stdout:

1. Per abilitare il log di diagnostica avanzato, eseguire le operazioni seguenti:
   * Seguire le istruzioni in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) per configurare l'app per la registrazione di diagnostica avanzata. Ridistribuire l'app.
   * Aggiungere le impostazioni `<handlerSettings>` indicate in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) al file *web.config* dell'app distribuita, usando la console Kudu:
     1. Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
     1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
     1. Aprire le cartelle nel percorso **site** > **wwwroot**. Modificare il file *web.config* selezionando il pulsante a forma di matita. Aggiungere la sezione `<handlerSettings>` come illustrato in [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Selezionare il pulsante **Salva**.
1. Aprire **Strumenti avanzati** nell'area **Strumenti di sviluppo**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Aprire le cartelle nel percorso **site** > **wwwroot**. Se non è stato specificato un percorso per il file *aspnetcore-debug.log*, il file viene visualizzato nell'elenco. Se è stato specificato un percorso, passare al percorso del file di log.
1. Aprire il file di log con il pulsante a forma di matita accanto al nome del file.

Al termine della risoluzione dei problemi, disabilitare la registrazione di debug:

1. Per disabilitare il log di debug avanzato, eseguire le operazioni seguenti:
   * Rimuovere `<handlerSettings>` dal file *web.config* in locale e ridistribuire l'app.
   * Usare la console Kudu per modificare il file *web.config* e rimuovere la sezione `<handlerSettings>`. Salvare il file.

> [!WARNING]
> La mancata disabilitazione del log di debug può causare un errore dell'app o del server. Non è previsto alcun limite per le dimensioni del file di log. Usare solo la registrazione di debug per la risoluzione dei problemi di avvio delle app.
>
> Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker-end

## <a name="common-startup-errors"></a>Errori di avvio comuni

Vedere <xref:host-and-deploy/azure-iis-errors-reference>. Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.

## <a name="slow-or-hanging-app"></a>App lenta o bloccata

Quando un'app risponde lentamente o si blocca durante una richiesta, vedere gli articoli seguenti:

* [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)
* [Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) (Usare l'estensione del sito Crash Diagnoser per acquisire il dump per i problemi di eccezioni intermittenti o i problemi di prestazioni nell'app Web di Azure)

## <a name="remote-debugging"></a>Debug remoto

Vedere gli argomenti seguenti:

* [Sezione Debug remoto di app Web in Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (documentazione di Azure)
* [Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Debug remoto di ASP.NET Core su IIS in Azure con Visual Studio 2017) (documentazione di Visual Studio)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) fornisce dati di telemetria dalle app ospitate in Servizio app di Azure, inclusi gli errori di registrazione e le funzionalità di creazione di report. Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app. Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Pannelli di monitoraggio

I pannelli di monitoraggio forniscono un'esperienza di risoluzione dei problemi alternativa ai metodi descritti in precedenza in questo argomento. È possibile usare questi pannelli per diagnosticare gli errori della serie 500.

Verificare che le estensioni di ASP.NET Core siano installate. Se le estensioni non sono installate, installarle manualmente:

1. Nella sezione del pannello **STRUMENTI DI SVILUPPO** selezionare il pannello **Estensioni**.
1. Nell'elenco dovrebbe essere visualizzato **ASP.NET Core Extensions** (Estensioni ASP.NET Core).
1. Se le estensioni non sono installate, selezionare il pulsante **Aggiungi**.
1. Scegliere **ASP.NET Core Extensions** (Estensioni ASP.NET Core) dall'elenco.
1. Selezionare **OK** per accettare le condizioni legali.
1. Selezionare **OK** nel pannello **Aggiungi estensione**.
1. Un messaggio popup informativo indica quando le estensioni sono state installate correttamente.

Se la registrazione stdout non è abilitata, attenersi ai passaggi riportati di seguito:

1. Nel portale di Azure selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;** . Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Aprire le cartelle nel percorso **site** > **wwwroot** e scorrere verso il basso fino a visualizzare il file *web.config* in fondo all'elenco.
1. Fare clic sull'icona della matita accanto al file *web.config*.
1. Impostare **stdoutLogEnabled** su `true` e cambiare il percorso di **stdoutLogFile** in: `\\?\%home%\LogFiles\stdout`.
1. Selezionare **Salva** per salvare il file *web.config* aggiornato.

Passare all'attivazione della registrazione diagnostica:

1. Nel portale di Azure selezionare il pannello **Log di diagnostica**.
1. Selezionare l'interruttore **Attivato** per **Registrazione applicazioni (file system)** e **Messaggi di errore dettagliati**. Selezionare il pulsante **Salva** nella parte superiore del pannello.
1. Per includere la traccia delle richieste non riuscite, anche nota come registrazione FREB (Failed Request Event Buffering), selezionare l'interruttore **Attivato** per **Traccia delle richieste non riuscite**.
1. Selezionare il pannello **Flusso di registrazione**, immediatamente sotto il pannello **Log di diagnostica** nel portale.
1. Effettuare una richiesta all'app.
1. Nei dati del flusso di registrazione viene indicata la causa dell'errore.

Al termine della risoluzione dei problemi, assicurarsi di disabilitare la registrazione stdout. Vedere le istruzioni nella sezione [Log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

Per visualizzare i log di traccia delle richieste non riuscite (log FREB):

1. Passare al pannello **Diagnostica e risolvi i problemi** nel portale di Azure.
1. Selezionare **Failed Request Tracing Logs** (Log di traccia delle richieste non riuscite) nell'area **STRUMENTI DI SUPPORTO** della barra laterale.

Per altre informazioni, vedere la [sezione relativa alla traccia delle richieste non riuscite nell'argomento Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) e [Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure: Come si abilita la traccia delle richieste non riuscite?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing).

Per altre informazioni, vedere [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> La mancata disabilitazione del log stdout può causare un errore dell'app o del server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.
>
> Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Risolvere gli errori HTTP "502 - Gateway non valido" e "503 - Servizio non disponibile" nelle App Web di Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)
* [Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure - Limitazioni di esecuzione di runtime di Servizio app di Azure)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)
