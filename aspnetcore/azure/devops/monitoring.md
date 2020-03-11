---
title: 'Monitorare ed eseguire il debug: DevOps con ASP.NET Core e Azure'
author: CamSoper
description: Monitoraggio e debug del codice come parte di una soluzione DevOps con ASP.NET Core e Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659502"
---
# <a name="monitor-and-debug"></a>Monitoraggio e debug

Dopo la distribuzione dell'app e creato una pipeline di DevOps, è importante capire come monitorare e risolvere i problemi dell'app.

In questa sezione, si completeranno le attività seguenti:

* Ricerca di base di monitoraggio e risoluzione dei problemi dei dati nel portale di Azure
* Informazioni su come monitoraggio di Azure offre un approfondimento sul metriche in tutti i servizi di Azure
* Connettere l'app web con Application Insights per la profilatura delle app
* Abilitare la registrazione e informazioni su come scaricare i log
* Stream log in tempo reale
* Informazioni su dove impostare gli avvisi
* Informazioni su remote debug Azure App Service web app.

## <a name="basic-monitoring-and-troubleshooting"></a>Risoluzione dei problemi e monitoraggio di base

App web del servizio App sono facilità di monitoraggio in tempo reale. Il portale di Azure esegue il rendering di metriche in grafici e grafici di facile comprensione.

1. Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.

1. Nella scheda **Panoramica** sono visualizzate informazioni utili, inclusi i grafici che visualizzano le metriche recenti.

    ![Screenshot che Mostra pannello della panoramica](./media/monitoring/overview.png)

    * **Http 5xx**: numero di errori sul lato server, in genere eccezioni nel codice ASP.NET Core.
    * **Dati in**: dati in ingresso nell'app Web.
    * **Dati in uscita**: dati in uscita dall'app Web ai client.
    * **Richieste**: numero di richieste HTTP.
    * **Tempo medio di risposta**: tempo medio per l'app Web per rispondere alle richieste HTTP.

    Alcuni strumenti self-service per la risoluzione dei problemi e ottimizzazione sono disponibili anche in questa pagina.

    ![Screenshot che Mostra strumenti di self-servizi](./media/monitoring/wizards.png)

    * **Diagnosticare e risolvere i problemi** è uno strumento di risoluzione dei problemi self-service.
    * **Application Insights** è per la profilatura delle prestazioni e il comportamento dell'app e viene illustrato più avanti in questa sezione.
    * **App Service Advisor** apporta consigli per ottimizzare l'esperienza dell'app.

## <a name="advanced-monitoring"></a>Monitoraggio avanzato

[Monitoraggio di Azure](/azure/monitoring-and-diagnostics/) è il servizio centralizzato per il monitoraggio di tutte le metriche e l'impostazione degli avvisi nei servizi di Azure. All'interno di monitoraggio di Azure, gli amministratori possono quindi tenere traccia delle prestazioni e identificare le tendenze in modo granulare. Ogni servizio di Azure offre un proprio [set di metriche](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) in monitoraggio di Azure.

## <a name="profile-with-application-insights"></a>Profilo con Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) è un servizio di Azure per analizzare le prestazioni e la stabilità delle app Web e il modo in cui gli utenti le usano. I dati da Application Insights sono più ampio e più profondo rispetto a quello del monitoraggio di Azure. I dati possono fornire agli sviluppatori e amministratori con informazioni sulla chiave per migliorare le app. Application Insights può essere aggiunto a una risorsa servizio App di Azure senza modifiche al codice.

1. Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.
1. Nella scheda **Panoramica** fare clic sul riquadro **Application Insights** .

    ![Riquadro Application Insights](./media/monitoring/app-insights.png)

1. Selezionare il pulsante di opzione **Crea nuova risorsa** . Usare il nome della risorsa predefinita e selezionare il percorso della risorsa di Application Insights. Il percorso non deve necessariamente corrispondere a quello dell'app web.

    ![Programma di installazione di Application Insights](./media/monitoring/new-app-insights.png)

1. Per **Runtime/Framework**, selezionare **ASP.NET Core**. Accettare le impostazioni predefinite.
1. Selezionare **OK**. Se viene richiesto di confermare, selezionare **continua**.
1. Dopo aver creata la risorsa, fare clic sul nome della risorsa di Application Insights per passare direttamente alla pagina di Application Insights.

    ![Nuova risorsa di Application Insights è pronto](./media/monitoring/new-app-insights-done.png)

Quando viene usata l'app, i dati si accumulano. Selezionare **Aggiorna** per ricaricare il pannello con nuovi dati.

![Scheda Panoramica di Application Insights](./media/monitoring/app-insights-overview.png)

Application Insights offre informazioni utili sul lato server senza alcuna configurazione aggiuntiva. Per ottenere il massimo valore da Application Insights, [instrumentare l'app con l'SDK di Application Insights](/azure/application-insights/app-insights-asp-net-core). Quando configurati correttamente, il servizio offre il monitoraggio end-to-end tra il server web e browser, tra cui prestazioni lato client. Per ulteriori informazioni, vedere la [documentazione Application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Registrazione

I log del server e app Web sono disabilitati per impostazione predefinita nel servizio App di Azure. Abilitare i log con i passaggi seguenti:

1. Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.
1. Nel menu a sinistra scorrere verso il basso fino alla sezione **monitoraggio** . Selezionare **log di diagnostica**.

    ![Collegamento di log di diagnostica](./media/monitoring/logging.png)

1. Attivare la **registrazione delle applicazioni (filesystem)** . Se richiesto, selezionare la casella per installare le estensioni per abilitare la registrazione nell'app web dell'app.
1. Impostare la **registrazione del server Web** sul **file System**.
1. Immettere il **periodo di conservazione** in giorni. Ad esempio, 30.
1. Fare clic su **Salva**.

ASP.NET Core e web log del server (servizio App) vengono generati per l'app web. Possono essere scaricati usando le informazioni FTP/FTPS visualizzate. La password è quello utilizzato per le credenziali di distribuzione create in precedenza in questa Guida. I log possono essere [trasmessi direttamente al computer locale con PowerShell o l'interfaccia della riga di comando di Azure](/azure/app-service/web-sites-enable-diagnostic-log#download). I log possono essere [visualizzati anche in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Streaming dei log

I log del server web e dell'App possono essere trasmessi in tempo reale tramite il portale.

1. Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.
1. Nel menu a sinistra scorrere verso il basso fino alla sezione **monitoraggio** e selezionare **flusso di registrazione**.

    ![Screenshot che mostra il collegamento flusso log](./media/monitoring/log-stream.png)

I log possono essere trasmessi anche tramite l'interfaccia della riga di comando di [Azure o Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), anche tramite l'cloud Shell.

## <a name="alerts"></a>Avvisi

Monitoraggio di Azure fornisce anche [avvisi in tempo reale](/azure/monitoring-and-diagnostics/insights-alerts-portal) in base a metriche, eventi amministrativi e altri criteri.

> *Nota: attualmente gli avvisi sulle metriche dell'app Web sono disponibili solo nel servizio avvisi (versione classica).*

Il [servizio avvisi (versione classica)](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) si trova in monitoraggio di Azure o nella sezione **monitoraggio** delle impostazioni del servizio app.

![Collegamento di avvisi (versione classica)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Il debug attivo

App Azure servizio può essere sottoposto [a debug in remoto con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) quando i log non forniscono informazioni sufficienti. Tuttavia, il debug remoto richiede l'app deve essere compilato con simboli di debug. Debug non deve essere eseguito nell'ambiente di produzione, ad eccezione come ultima risorsa.

## <a name="conclusion"></a>Conclusioni

In questa sezione sono completate le attività seguenti:

* Ricerca di base di monitoraggio e risoluzione dei problemi dei dati nel portale di Azure
* Informazioni su come monitoraggio di Azure offre un approfondimento sul metriche in tutti i servizi di Azure
* Connettere l'app web con Application Insights per la profilatura delle app
* Abilitare la registrazione e informazioni su come scaricare i log
* Stream log in tempo reale
* Informazioni su dove impostare gli avvisi
* Informazioni su remote debug Azure App Service web app.

## <a name="additional-reading"></a>Informazioni aggiuntive

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Monitoraggio delle prestazioni dell'applicazione web di Azure](/azure/application-insights/app-insights-azure-web-apps)
* [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log)
* [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Creare avvisi delle metriche classici in monitoraggio di Azure per i servizi di Azure-portale di Azure](/azure/monitoring-and-diagnostics/insights-alerts-portal)
