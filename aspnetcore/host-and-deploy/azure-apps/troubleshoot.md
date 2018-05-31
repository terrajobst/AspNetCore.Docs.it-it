---
title: Risolvere i problemi di ASP.NET Core in Servizio app di Azure
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni di ASP.NET Core in Servizio App di Azure.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 47056c80c7abf5dd5ad5ae96af7b821d31b21b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897427"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Risolvere i problemi di ASP.NET Core in Servizio app di Azure

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core usando gli strumenti di diagnostica di Servizio app di Azure. Per altre indicazioni sulla risoluzione dei problemi, vedere [Panoramica della diagnostica del servizio app di Azure](/azure/app-service/app-service-diagnostics) e [Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure](/azure/app-service/web-sites-monitor) nella documentazione di Azure.

## <a name="app-startup-errors"></a>Errori di avvio delle app

**502.5 Errore del processo**  
Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tenta di avviare il processo di lavoro, ma l'avvio non riesce. Esaminare il log eventi dell'applicazione spesso è utile per risolvere i problemi di questo tipo. La procedura per accedere al log è illustrata nella sezione [Log eventi dell'applicazione](#application-event-log).

La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'app:

![Finestra del browser con la pagina 502.5 Errore del processo](troubleshoot/_static/process-failure-page.png)

**500 Errore interno del server**  
L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.

Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta. La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*. Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente. Dal punto di vista del server, questo è corretto. L'app è stata effettivamente avviata, ma non può generare una risposta valida. Per risolvere il problema, [eseguire l'app nella console Kudu](#run-the-app-in-the-kudu-console) o [abilitare il log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

**Reimpostazione della connessione**

Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore. Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta. Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client. La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.

## <a name="default-startup-limits"></a>Limiti di avvio predefiniti

Il modulo ASP.NET Core è configurato con un valore predefinito *startupTimeLimit* di 120 secondi. Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo. Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Per accedere al log eventi dell'applicazione, usare il pannello **Diagnostica e risolvi i problemi** nel portale di Azure:

1. Nel portale di Azure aprire il pannello dell'app nel pannello **Servizi app**.
1. Selezionare il pannello **Diagnostica e risolvi i problemi**.
1. In **SELECT PROBLEM CATEGORY** (SELEZIONARE LA CATEGORIA DEL PROBLEMA) selezionare il pulsante **Web App Down** (App Web inattiva).
1. In **Suggested Solutions** (Soluzioni consigliate) aprire il riquadro **Open Application Event Logs** (Apri log eventi dell'applicazione). Selezionare il pulsante **Open Application Event Logs** (Apri log eventi dell'applicazione).
1. Esaminare l'errore più recente fornito da *IIS AspNetCoreModule* nella colonna **Origine**.

Un'alternativa all'uso del pannello **Diagnostica e risolvi i problemi** consiste nell'esaminare direttamente il file del log eventi dell'applicazione usando [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;**. Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Aprire la cartella **LogFiles**.
1. Selezionare l'icona della matita accanto al file *eventlog.xml*.
1. Esaminare il log. Scorrere fino alla fine del log per visualizzare gli eventi più recenti.

### <a name="run-the-app-in-the-kudu-console"></a>Eseguire l'app nella console Kudu

Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione. È possibile eseguire l'app nella console di esecuzione remota [Kudu](https://github.com/projectkudu/kudu/wiki) per individuare l'errore:

1. Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;**. Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Aprire le cartelle nel percorso **site** > **wwwroot**.
1. Nella console avviare l'app eseguendo il relativo assembly.
   * Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), eseguire l'assembly dell'app con *dotnet.exe*. Nel comando seguente sostituire `<assembly_name>` con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`
   * Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd), avviare il file eseguibile dell'app. Nel comando seguente sostituire `<assembly_name>` con il nome dell'assembly dell'app: `<assembly_name>.exe`
1. L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà inviato alla console Kudu.

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
1. Tornare al portale di Azure. Selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;**. Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
1. Usando la barra di spostamento nella parte superiore della pagina, aprire **Console di debug** e selezionare **CMD**.
1. Selezionare la cartella **LogFiles**.
1. Controllare la colonna **Data modifica** e selezionare l'icona della matita per modificare il log stdout con la data di modifica più recente.
1. Quando si apre il file di log, verrà visualizzato l'errore.

**Importante**: Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.

1. Nella **console diagnostica** Kudu tornare al percorso **site** > **wwwroot** per visualizzare il file *web.config*. Aprire nuovamente il file **web.config** selezionando l'icona della matita.
1. Impostare **stdoutLogEnabled** su `false`.
1. Selezionare **Salva** per salvare il file.

> [!WARNING]
> La mancata disabilitazione del log stdout può causare un errore dell'app o del server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare. Usare la registrazione stdout solo per la risoluzione dei problemi di avvio delle app.
>
> Per la registrazione generale in un'app ASP.NET Core dopo l'avvio, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Errori di avvio comuni 

Vedere le [informazioni di riferimento sugli errori comuni di ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.

## <a name="slow-or-hanging-app"></a>App lenta o bloccata

Quando un'app risponde lentamente o si blocca durante una richiesta, vedere [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure) per indicazioni sul debug.

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

1. Nel portale di Azure selezionare il pannello **Strumenti avanzati** nell'area **STRUMENTI DI SVILUPPO**. Selezionare il pulsante **Vai&rarr;**. Verrà aperta la console Kudu in una nuova scheda o finestra del browser.
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

**Importante**: Al termine della risoluzione dei problemi, assicurarsi di disabilitare la registrazione stdout. Vedere le istruzioni nella sezione [Log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

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

* [Introduzione alla gestione degli errori in ASP.NET Core](xref:fundamentals/error-handling)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Risolvere gli errori HTTP "502 - Gateway non valido" e "503 - Servizio non disponibile" nelle App Web di Azure](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) (Risoluzione dei problemi di prestazioni delle app Web lente in Servizio app di Azure)
* [Domande frequenti sulle prestazioni delle applicazioni in App Web di Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure - Limitazioni di esecuzione di runtime di Servizio app di Azure)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)
