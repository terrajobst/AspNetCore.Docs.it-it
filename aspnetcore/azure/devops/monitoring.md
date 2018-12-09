---
title: 'Monitorare ed eseguire il debug: DevOps con ASP.NET Core e Azure'
author: CamSoper
description: Monitoraggio e debug del codice come parte di una soluzione DevOps con ASP.NET Core e Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: e005b951aec578b396fc19dec5d2f55cbce4f664
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121609"
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

1. Aprire il [portale di Azure](https://portal.azure.com), quindi passare al *mywebapp\<unique_number\>*  servizio App.

1. Il **Panoramica** scheda vengono visualizzate informazioni utili di "in Panoramica", inclusi grafici di visualizzazione delle metriche di recente.

    ![Screenshot che Mostra pannello della panoramica](./media/monitoring/overview.png)

    * **Http 5xx**: numero di errori sul lato server, in genere eccezioni nel codice ASP.NET Core.
    * **I dati In**: ingresso dei dati in arrivo nella tua app web.
    * **Dati in uscita**: i dati in uscita dall'app web ai client.
    * **Le richieste**: conteggio delle richieste HTTP.
    * **Tempo medio di risposta**: tempo medio per l'app web rispondere alle richieste HTTP.

    Alcuni strumenti self-service per la risoluzione dei problemi e ottimizzazione sono disponibili anche in questa pagina.

    ![Screenshot che Mostra strumenti di self-servizi](./media/monitoring/wizards.png)

    * **Diagnosticare e risolvere i problemi** è una risoluzione dei problemi self-service.
    * **Application Insights** è per la profilatura delle prestazioni e comportamento dell'app e viene illustrato più avanti in questa sezione.
    * **App Service Advisor** fornisce consigli per ottimizzare l'esperienza delle app.

## <a name="advanced-monitoring"></a>Monitoraggio avanzato

[Monitoraggio di Azure](/azure/monitoring-and-diagnostics/) è il servizio centralizzato per tutte le metriche di monitoraggio e l'impostazione degli avvisi tra servizi di Azure. All'interno di monitoraggio di Azure, gli amministratori possono quindi tenere traccia delle prestazioni e identificare le tendenze in modo granulare. Ogni servizio di Azure offre un proprio [set di metriche](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) a monitoraggio di Azure.

## <a name="profile-with-application-insights"></a>Profilo con Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) è un servizio di Azure per analizzare le prestazioni e stabilità dell'App web e come utenti le usano. I dati da Application Insights sono più ampio e più profondo rispetto a quello del monitoraggio di Azure. I dati possono fornire agli sviluppatori e amministratori con informazioni sulla chiave per migliorare le app. Application Insights può essere aggiunto a una risorsa servizio App di Azure senza modifiche al codice.

1. Aprire il [portale di Azure](https://portal.azure.com), quindi passare al *mywebapp\<unique_number\>*  servizio App.
1. Dal **Overview** scheda, fare clic sui **Application Insights** riquadro.

    ![Riquadro di Application Insights](./media/monitoring/app-insights.png)

1. Selezionare il **creare una nuova risorsa** pulsante di opzione. Usare il nome della risorsa predefinita e selezionare il percorso della risorsa di Application Insights. Il percorso non deve necessariamente corrispondere a quello dell'app web.

    ![Programma di installazione di Application Insights](./media/monitoring/new-app-insights.png)

1. Per la **Runtime/Framework**, selezionare **ASP.NET Core**. Accettare le impostazioni predefinite.
1. Scegliere **OK**. Se viene richiesto di confermare, selezionare **continuazione**.
1. Dopo aver creata la risorsa, fare clic sul nome della risorsa di Application Insights per passare direttamente alla pagina di Application Insights.

    ![Nuova risorsa di Application Insights è pronto](./media/monitoring/new-app-insights-done.png)

Quando viene usata l'app, i dati si accumulano. Selezionare **Aggiorna** ricaricare il pannello con i nuovi dati.

![Scheda Panoramica di Application Insights](./media/monitoring/app-insights-overview.png)

Application Insights offre informazioni utili sul lato server senza alcuna configurazione aggiuntiva. Per ottenere il massimo valore da Application Insights [instrumentare l'app con Application Insights SDK](/azure/application-insights/app-insights-asp-net-core). Quando configurati correttamente, il servizio offre il monitoraggio end-to-end tra il server web e browser, tra cui prestazioni lato client. Per altre informazioni, vedere la [documentazione di Application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Registrazione

I log del server e app Web sono disabilitati per impostazione predefinita nel servizio App di Azure. Abilitare i log con i passaggi seguenti:

1. Aprire il [portale di Azure](https://portal.azure.com)e passare alle *mywebapp\<unique_number\>*  servizio App.
1. Nel menu a sinistra, scorrere verso il basso il **Monitoring** sezione. Selezionare **log di diagnostica**.

    ![Collegamento di log di diagnostica](./media/monitoring/logging.png)

1. Accendere **registrazione applicazioni (file System)**. Se richiesto, selezionare la casella per installare le estensioni per abilitare la registrazione nell'app web dell'app.
1. Impostare **la registrazione del server Web** al **File System**.
1. Immettere il **periodo di conservazione** in giorni. Ad esempio, 30.
1. Fare clic su **Salva**.

ASP.NET Core e web log del server (servizio App) vengono generati per l'app web. Possono essere scaricati usando le informazioni FTP/FTPS visualizzate. La password è quello utilizzato per le credenziali di distribuzione create in precedenza in questa Guida. I log possono essere [trasmesso direttamente al computer locale con PowerShell o Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download). I log possono rivelarsi [visualizzati in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Streaming dei log

I log del server web e dell'App possono essere trasmessi in tempo reale tramite il portale.

1. Aprire il [portale di Azure](https://portal.azure.com)e passare alle *mywebapp\<unique_number\>*  servizio App.
1. Nel menu a sinistra, scorrere verso il basso il **Monitoring** della sezione e selezionare **flusso di registrazione**.

    ![Screenshot che mostra il collegamento flusso log](./media/monitoring/log-stream.png)

I log possono rivelarsi [trasmessi tramite la CLI di Azure o Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), tra cui tramite il Cloud Shell.

## <a name="alerts"></a>Avvisi

Monitoraggio di Azure offre inoltre [avvisi in tempo reale](/azure/monitoring-and-diagnostics/insights-alerts-portal) basato sulle metriche, gli eventi amministrativi e altri criteri.

> *Nota: Attualmente gli avvisi sulle metriche di app web è disponibili solo nel servizio avvisi (versione classica).*

Il [avvisi (versione classico) del servizio](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) sono disponibili in Monitoraggio di Azure o nel **Monitoring** sezione delle impostazioni del servizio App.

![Collegamento di avvisi (versione classica)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Il debug attivo

Servizio App di Azure può essere [eseguire il debug in remoto con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) quando i log non forniscono informazioni sufficienti. Tuttavia, il debug remoto richiede l'app deve essere compilato con simboli di debug. Debug non deve essere eseguito nell'ambiente di produzione, ad eccezione come ultima risorsa.

## <a name="conclusion"></a>Conclusione

In questa sezione sono completate le attività seguenti:

* Ricerca di base di monitoraggio e risoluzione dei problemi dei dati nel portale di Azure
* Informazioni su come monitoraggio di Azure offre un approfondimento sul metriche in tutti i servizi di Azure
* Connettere l'app web con Application Insights per la profilatura delle app
* Abilitare la registrazione e informazioni su come scaricare i log
* Stream log in tempo reale
* Informazioni su dove impostare gli avvisi
* Informazioni su remote debug Azure App Service web app.

## <a name="additional-reading"></a>Altre informazioni

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Monitorare le prestazioni di app web di Azure con Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log)
* [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Creare avvisi delle metriche classici in Monitoraggio di Azure per servizi di Azure - portale di Azure](/azure/monitoring-and-diagnostics/insights-alerts-portal)
