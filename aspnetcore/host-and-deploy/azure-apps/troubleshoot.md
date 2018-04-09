---
title: Risolvere i problemi relativi a ASP.NET Core nel servizio App di Azure
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
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Risolvere i problemi relativi a ASP.NET Core nel servizio App di Azure

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

In questo articolo vengono fornite istruzioni su come diagnosticare un ASP.NET Core problema di avvio di app utilizzando gli strumenti di diagnostica del servizio App di Azure. Per ulteriori suggerimenti sulla risoluzione dei problemi, vedere [Panoramica di diagnostica di Azure App Service](/azure/app-service/app-service-diagnostics) e [procedura: monitorare le App in Azure App Service](/azure/app-service/web-sites-monitor) nella documentazione di Azure.

## <a name="app-startup-errors"></a>Errori di avvio delle App

**502.5 Errore processo**  
Il processo di lavoro ha esito negativo. Non si avvia l'app.

Il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) tenta di avviare il processo di lavoro ma non viene avviato. Esaminare il registro eventi applicazioni spesso consente di risolvere i problemi di questo tipo. Accesso al registro è illustrato il [registro eventi dell'applicazione](#application-event-log) sezione.

Il *502.5 errore del processo* pagina di errore viene restituito quando un'app con configurazione non valida fa sì che il processo di lavoro esito negativo:

![Visualizzazione della pagina di errore del processo 502.5 finestra del browser](troubleshoot/_static/process-failure-page.png)

**500 Errore interno del Server**  
Avvio dell'app, ma un errore impedisce al server di soddisfare la richiesta.

Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta. La risposta non può contenere alcun contenuto o la risposta potrebbe essere visualizzato come un *500 Internal Server Error* nel browser. Registro eventi dell'applicazione in genere indica che l'app è stato avviato normalmente. Dalla prospettiva del server, che non è corretto. L'app è stato avviato, ma non può generare una risposta valida. [Eseguire l'app nella console di Kudu](#run-the-app-in-the-kudu-console) o [abilitare il log di stdout ASP.NET Core modulo](#aspnet-core-module-stdout-log) per risolvere il problema.

**Reimpostazione della connessione**

Se si verifica un errore dopo le intestazioni vengono inviate, è troppo tardi per il server inviare un **500 Internal Server Error** quando si verifica un errore. Ciò accade spesso quando si verifica un errore durante la serializzazione di oggetti complessi, di una risposta. Questo tipo di errore viene visualizzato come un *reimpostazione della connessione* errore nel client. [Registrazione applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.

## <a name="default-startup-limits"></a>Limiti di avvio predefiniti

Il modulo di base di ASP.NET è configurato con un valore predefinito *startupTimeLimit* di 120 secondi. Quando si mantiene il valore predefinito, un'app potrebbe richiedere fino a due minuti per l'avvio prima che il modulo Registra un errore del processo. Per informazioni sulla configurazione del modulo, vedere [attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Per accedere al registro eventi applicazione, utilizzare il **diagnosticare e risolvere i problemi** pannello nel portale di Azure:

1. Nel portale di Azure, aprire il pannello dell'app nel **servizi App** blade.
1. Selezionare il **diagnosticare e risolvere i problemi** blade.
1. In **Seleziona categoria di problema**, selezionare il **Web App verso il basso** pulsante.
1. In **soluzioni suggerite**, aprire il riquadro per **aprire i registri eventi di applicazione**. Selezionare il **aprire i registri eventi applicazioni** pulsante.
1. Esaminare l'errore più recente fornito dal *IIS AspNetCoreModule* nel **origine** colonna.

Un'alternativa all'utilizzo di **diagnosticare e risolvere i problemi** pannello consiste nell'esaminare il file di registro eventi dell'applicazione direttamente utilizzando [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Selezionare il **strumenti avanzati** pannello nel **gli strumenti di sviluppo** area. Selezionare il **passare&rarr;**  pulsante. La console Kudu viene aperta in una nuova scheda del browser o una finestra.
1. Barra di navigazione nella parte superiore della pagina, aprire **console Debug** e selezionare **CMD**.
1. Aprire il **LogFiles** cartella.
1. Selezionare l'icona della matita accanto al *eventlog.xml* file.
1. Esaminare il registro. Scorrere fino alla fine del log per visualizzare gli eventi più recenti.

### <a name="run-the-app-in-the-kudu-console"></a>Eseguire l'app nella console di Kudu

Molti errori di avvio non producano informazioni utili nel registro eventi dell'applicazione. È possibile eseguire l'app nel [Kudu](https://github.com/projectkudu/kudu/wiki) Console remota di esecuzione per individuare l'errore:

1. Selezionare il **strumenti avanzati** pannello nel **gli strumenti di sviluppo** area. Selezionare il **passare&rarr;**  pulsante. La console Kudu viene aperta in una nuova scheda del browser o una finestra.
1. Barra di navigazione nella parte superiore della pagina, aprire **console Debug** e selezionare **CMD**.
1. Aprire le cartelle nel percorso **sito** > **wwwroot**.
1. Nella console, eseguire l'app tramite l'esecuzione di assembly dell'applicazione.
   * Se l'applicazione è un [framework dipendente distribuzione](/dotnet/core/deploying/#framework-dependent-deployments-fdd), eseguire l'assembly dell'applicazione con *dotnet.exe*. Il comando seguente, sostituire il nome dell'assembly dell'applicazione per `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Se l'applicazione è un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd), eseguire l'app dell'eseguibile. Il comando seguente, sostituire il nome dell'assembly dell'applicazione per `<assembly_name>`: `<assembly_name>.exe`
1. La console di output dall'app, che mostra gli eventuali errori, viene reindirizzata alla console Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Log di stdout ASP.NET modulo Core

Il log di stdout ASP.NET Core modulo spesso registra i messaggi di errore utile non trovati nel registro eventi dell'applicazione. Per abilitare e visualizzare i log di stdout:

1. Passare il **diagnosticare e risolvere i problemi** pannello nel portale di Azure.
1. In **Seleziona categoria di problema**, selezionare il **Web App verso il basso** pulsante.
1. In **soluzioni suggerite** > **abilitare il reindirizzamento di Log di Stdout**, selezionare il pulsante di **Apri la Console Web. config modificare Kudu**.
1. Nel Kudu **Console diagnostica**, aprire le cartelle nel percorso **sito** > **wwwroot**. Scorrere fino a mostrare la *Web. config* file nella parte inferiore dell'elenco.
1. Fare clic sull'icona della matita accanto al *Web. config* file.
1. Impostare **stdoutLogEnabled** a `true` e modificare il **stdoutLogFile** percorso: `\\?\%home%\LogFiles\stdout`.
1. Selezionare **salvare** salvare aggiornato *Web. config* file.
1. Effettuare una richiesta per l'app.
1. Tornare al portale di Azure. Selezionare il **strumenti avanzati** pannello nel **gli strumenti di sviluppo** area. Selezionare il **passare&rarr;**  pulsante. La console Kudu viene aperta in una nuova scheda del browser o una finestra.
1. Barra di navigazione nella parte superiore della pagina, aprire **console Debug** e selezionare **CMD**.
1. Selezionare il **LogFiles** cartella.
1. Controllare il **Modified** colonna e selezionare l'icona della matita per modificare il stdout accedere con la data dell'ultima modifica.
1. Quando si apre il file di log, verrà visualizzato l'errore.

**Importante**: Disabilitare la registrazione di stdout quando la risoluzione dei problemi è stata completata.

1. Nel Kudu **Console diagnostica**, restituito nel percorso **sito** > **wwwroot** per rivelare il *Web. config* file. Aprire il **Web. config** file nuovo selezionando l'icona della matita.
1. Impostare **stdoutLogEnabled** a `false`.
1. Selezionare **salvare** per salvare il file.

> [!WARNING]
> Mancata possibilità di disabilitazione del log di stdout può causare un errore di applicazione o server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare. Utilizzare solo stdout la registrazione per la risoluzione dei problemi di avvio dell'app.
>
> Per generale registrare in un'applicazione ASP.NET Core dopo l'avvio, utilizzare una libreria di registrazione che limita dimensioni file di log e ruota i log. Per ulteriori informazioni, vedere [provider di log di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Errori più comuni di avvio 

Vedere il [riferimento per gli errori comune ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). La maggior parte dei problemi comuni che impediscono l'avvio dell'app sono descritte nell'argomento di riferimento.

## <a name="slow-or-hanging-app"></a>App lento o bloccati

Quando un'app risponde lentamente o si blocca una richiesta, vedere [risoluzione dei problemi problemi di prestazioni lente web app in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) per il debug di istruzioni.

## <a name="remote-debugging"></a>Debug remoto

Vedere gli argomenti seguenti:

* [Sezione delle App web di un'app web nel servizio App di Azure con Visual Studio di risoluzione dei problemi di debug remoto](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (documentazione di Azure)
* [Remote Debug ASP.NET Core in IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (documentazione di Visual Studio)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) fornisce i dati di telemetria da app ospitate in Azure App Service, inclusi errori di registrazione e le funzionalità di reporting. Application Insights può segnalare solo in caso di errori che si verifica dopo l'avvio dell'app quando diventano disponibili le funzionalità di registrazione dell'app. Per ulteriori informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Monitoraggio di pannelli

Monitoraggio pannelli forniscono un'alternativa esperienza ai metodi descritti in precedenza nell'argomento di risoluzione dei problemi. Per diagnosticare gli errori della serie 500, è possono utilizzare i pannelli.

Verificare che le estensioni di base di ASP.NET siano installate. Se sono installate le estensioni, installarli manualmente:

1. Nel **gli strumenti di sviluppo** sezione pannello, selezionare il **estensioni** blade.
1. Il **ASP.NET Core estensioni** dovrebbe essere visualizzato nell'elenco.
1. Se sono installate le estensioni, selezionare il **Aggiungi** pulsante.
1. Scegliere il **ASP.NET Core estensioni** dall'elenco.
1. Selezionare **OK** per accettare le condizioni legali.
1. Selezionare **OK** sul **aggiungere estensione** blade.
1. Un messaggio popup informativo indica quando le estensioni vengono installate correttamente.

Se la registrazione di stdout non è abilitata, seguire questi passaggi:

1. Nel portale di Azure, selezionare il **strumenti avanzati** pannello nel **gli strumenti di sviluppo** area. Selezionare il **passare&rarr;**  pulsante. La console Kudu viene aperta in una nuova scheda del browser o una finestra.
1. Barra di navigazione nella parte superiore della pagina, aprire **console Debug** e selezionare **CMD**.
1. Aprire le cartelle nel percorso **sito** > **wwwroot** e scorrere fino a mostrare la *Web. config* file nella parte inferiore dell'elenco.
1. Fare clic sull'icona della matita accanto al *Web. config* file.
1. Impostare **stdoutLogEnabled** a `true` e modificare il **stdoutLogFile** percorso: `\\?\%home%\LogFiles\stdout`.
1. Selezionare **salvare** salvare aggiornato *Web. config* file.

Continuare per attivare la registrazione diagnostica:

1. Nel portale di Azure, selezionare il **log di diagnostica** blade.
1. Selezionare il **su** per passare **registrazione delle applicazioni (file System)** e **messaggi di errore dettagliati**. Selezionare il **salvare** pulsante nella parte superiore del pannello.
1. Per includere funzionalità di traccia richieste non riuscite, noto anche come registrazione Impossibile richiesta evento Buffering (FREB), selezionare il **su** per passare **traccia richieste non riuscita**. 
1. Selezionare il **flusso del Log** Pannello di immediatamente sotto il **log di diagnostica** pannello nel portale.
1. Effettuare una richiesta per l'app.
1. All'interno dei dati di flusso di log, viene indicata la causa dell'errore.

**Importante**: Assicurarsi di disabilitare la registrazione stdout quando la risoluzione dei problemi è stata completata. Vedere le istruzioni di [log di stdout ASP.NET Core modulo](#aspnet-core-module-stdout-log) sezione.

Per visualizzare i log di traccia richieste non riuscite (log FREB):

1. Passare il **diagnosticare e risolvere i problemi** pannello nel portale di Azure.
1. Selezionare **Impossibile log di traccia delle richieste** dal **gli strumenti di supporto** area della barra laterale.

Vedere [sezione di abilitare la registrazione diagnostica per App web nell'argomento di servizio App di Azure tiene traccia di richiesta non riuscita](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) e [FAQ le prestazioni dell'applicazione per le app Web in Azure: come è possibile attivare la traccia richieste non riuscite?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Per ulteriori informazioni.

Per ulteriori informazioni, vedere [abilitare la registrazione diagnostica per le app web in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Mancata possibilità di disabilitazione del log di stdout può causare un errore di applicazione o server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.
>
> Per la routine registrazione in un'applicazione ASP.NET di base, utilizzare una libreria di registrazione che limita dimensioni file di log e ruota i log. Per ulteriori informazioni, vedere [provider di log di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione alla gestione degli errori in ASP.NET Core](xref:fundamentals/error-handling)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Risolvere gli errori HTTP di "502 gateway non valido" e "503 Servizio non disponibile" nelle App web di Azure](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Risolvere i problemi di prestazioni lente web app in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Domande frequenti sulle prestazioni dell'applicazione per le app Web in Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure sandbox Web App (limitazioni di esecuzione runtime di servizio App)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)
