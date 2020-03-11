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
# <a name="monitor-and-debug"></a><span data-ttu-id="8dd95-103">Monitoraggio e debug</span><span class="sxs-lookup"><span data-stu-id="8dd95-103">Monitor and debug</span></span>

<span data-ttu-id="8dd95-104">Dopo la distribuzione dell'app e creato una pipeline di DevOps, è importante capire come monitorare e risolvere i problemi dell'app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="8dd95-105">In questa sezione, si completeranno le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="8dd95-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="8dd95-106">Ricerca di base di monitoraggio e risoluzione dei problemi dei dati nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="8dd95-107">Informazioni su come monitoraggio di Azure offre un approfondimento sul metriche in tutti i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="8dd95-108">Connettere l'app web con Application Insights per la profilatura delle app</span><span class="sxs-lookup"><span data-stu-id="8dd95-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="8dd95-109">Abilitare la registrazione e informazioni su come scaricare i log</span><span class="sxs-lookup"><span data-stu-id="8dd95-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="8dd95-110">Stream log in tempo reale</span><span class="sxs-lookup"><span data-stu-id="8dd95-110">Stream logs in real time</span></span>
* <span data-ttu-id="8dd95-111">Informazioni su dove impostare gli avvisi</span><span class="sxs-lookup"><span data-stu-id="8dd95-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="8dd95-112">Informazioni su remote debug Azure App Service web app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="8dd95-113">Risoluzione dei problemi e monitoraggio di base</span><span class="sxs-lookup"><span data-stu-id="8dd95-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="8dd95-114">App web del servizio App sono facilità di monitoraggio in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="8dd95-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="8dd95-115">Il portale di Azure esegue il rendering di metriche in grafici e grafici di facile comprensione.</span><span class="sxs-lookup"><span data-stu-id="8dd95-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="8dd95-116">Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="8dd95-117">Nella scheda **Panoramica** sono visualizzate informazioni utili, inclusi i grafici che visualizzano le metriche recenti.</span><span class="sxs-lookup"><span data-stu-id="8dd95-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Screenshot che Mostra pannello della panoramica](./media/monitoring/overview.png)

    * <span data-ttu-id="8dd95-119">**Http 5xx**: numero di errori sul lato server, in genere eccezioni nel codice ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dd95-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="8dd95-120">**Dati in**: dati in ingresso nell'app Web.</span><span class="sxs-lookup"><span data-stu-id="8dd95-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="8dd95-121">**Dati in uscita**: dati in uscita dall'app Web ai client.</span><span class="sxs-lookup"><span data-stu-id="8dd95-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="8dd95-122">**Richieste**: numero di richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="8dd95-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="8dd95-123">**Tempo medio di risposta**: tempo medio per l'app Web per rispondere alle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="8dd95-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="8dd95-124">Alcuni strumenti self-service per la risoluzione dei problemi e ottimizzazione sono disponibili anche in questa pagina.</span><span class="sxs-lookup"><span data-stu-id="8dd95-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Screenshot che Mostra strumenti di self-servizi](./media/monitoring/wizards.png)

    * <span data-ttu-id="8dd95-126">**Diagnosticare e risolvere i problemi** è uno strumento di risoluzione dei problemi self-service.</span><span class="sxs-lookup"><span data-stu-id="8dd95-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="8dd95-127">**Application Insights** è per la profilatura delle prestazioni e il comportamento dell'app e viene illustrato più avanti in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="8dd95-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="8dd95-128">**App Service Advisor** apporta consigli per ottimizzare l'esperienza dell'app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="8dd95-129">Monitoraggio avanzato</span><span class="sxs-lookup"><span data-stu-id="8dd95-129">Advanced monitoring</span></span>

<span data-ttu-id="8dd95-130">[Monitoraggio di Azure](/azure/monitoring-and-diagnostics/) è il servizio centralizzato per il monitoraggio di tutte le metriche e l'impostazione degli avvisi nei servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd95-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="8dd95-131">All'interno di monitoraggio di Azure, gli amministratori possono quindi tenere traccia delle prestazioni e identificare le tendenze in modo granulare.</span><span class="sxs-lookup"><span data-stu-id="8dd95-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="8dd95-132">Ogni servizio di Azure offre un proprio [set di metriche](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) in monitoraggio di Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd95-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="8dd95-133">Profilo con Application Insights</span><span class="sxs-lookup"><span data-stu-id="8dd95-133">Profile with Application Insights</span></span>

<span data-ttu-id="8dd95-134">[Application Insights](/azure/application-insights/app-insights-overview) è un servizio di Azure per analizzare le prestazioni e la stabilità delle app Web e il modo in cui gli utenti le usano.</span><span class="sxs-lookup"><span data-stu-id="8dd95-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="8dd95-135">I dati da Application Insights sono più ampio e più profondo rispetto a quello del monitoraggio di Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd95-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="8dd95-136">I dati possono fornire agli sviluppatori e amministratori con informazioni sulla chiave per migliorare le app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="8dd95-137">Application Insights può essere aggiunto a una risorsa servizio App di Azure senza modifiche al codice.</span><span class="sxs-lookup"><span data-stu-id="8dd95-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="8dd95-138">Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="8dd95-139">Nella scheda **Panoramica** fare clic sul riquadro **Application Insights** .</span><span class="sxs-lookup"><span data-stu-id="8dd95-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Riquadro Application Insights](./media/monitoring/app-insights.png)

1. <span data-ttu-id="8dd95-141">Selezionare il pulsante di opzione **Crea nuova risorsa** .</span><span class="sxs-lookup"><span data-stu-id="8dd95-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="8dd95-142">Usare il nome della risorsa predefinita e selezionare il percorso della risorsa di Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8dd95-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="8dd95-143">Il percorso non deve necessariamente corrispondere a quello dell'app web.</span><span class="sxs-lookup"><span data-stu-id="8dd95-143">The location doesn't need to match that of your web app.</span></span>

    ![Programma di installazione di Application Insights](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="8dd95-145">Per **Runtime/Framework**, selezionare **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="8dd95-146">Accettare le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="8dd95-146">Accept the default settings.</span></span>
1. <span data-ttu-id="8dd95-147">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-147">Select **OK**.</span></span> <span data-ttu-id="8dd95-148">Se viene richiesto di confermare, selezionare **continua**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="8dd95-149">Dopo aver creata la risorsa, fare clic sul nome della risorsa di Application Insights per passare direttamente alla pagina di Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8dd95-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Nuova risorsa di Application Insights è pronto](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="8dd95-151">Quando viene usata l'app, i dati si accumulano.</span><span class="sxs-lookup"><span data-stu-id="8dd95-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="8dd95-152">Selezionare **Aggiorna** per ricaricare il pannello con nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="8dd95-152">Select **Refresh** to reload the blade with new data.</span></span>

![Scheda Panoramica di Application Insights](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="8dd95-154">Application Insights offre informazioni utili sul lato server senza alcuna configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="8dd95-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="8dd95-155">Per ottenere il massimo valore da Application Insights, [instrumentare l'app con l'SDK di Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="8dd95-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="8dd95-156">Quando configurati correttamente, il servizio offre il monitoraggio end-to-end tra il server web e browser, tra cui prestazioni lato client.</span><span class="sxs-lookup"><span data-stu-id="8dd95-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="8dd95-157">Per ulteriori informazioni, vedere la [documentazione Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="8dd95-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="8dd95-158">Registrazione</span><span class="sxs-lookup"><span data-stu-id="8dd95-158">Logging</span></span>

<span data-ttu-id="8dd95-159">I log del server e app Web sono disabilitati per impostazione predefinita nel servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd95-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="8dd95-160">Abilitare i log con i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8dd95-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="8dd95-161">Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="8dd95-162">Nel menu a sinistra scorrere verso il basso fino alla sezione **monitoraggio** .</span><span class="sxs-lookup"><span data-stu-id="8dd95-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="8dd95-163">Selezionare **log di diagnostica**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-163">Select **Diagnostics logs**.</span></span>

    ![Collegamento di log di diagnostica](./media/monitoring/logging.png)

1. <span data-ttu-id="8dd95-165">Attivare la **registrazione delle applicazioni (filesystem)** .</span><span class="sxs-lookup"><span data-stu-id="8dd95-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="8dd95-166">Se richiesto, selezionare la casella per installare le estensioni per abilitare la registrazione nell'app web dell'app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="8dd95-167">Impostare la **registrazione del server Web** sul **file System**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="8dd95-168">Immettere il **periodo di conservazione** in giorni.</span><span class="sxs-lookup"><span data-stu-id="8dd95-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="8dd95-169">Ad esempio, 30.</span><span class="sxs-lookup"><span data-stu-id="8dd95-169">For example, 30.</span></span>
1. <span data-ttu-id="8dd95-170">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-170">Click **Save**.</span></span>

<span data-ttu-id="8dd95-171">ASP.NET Core e web log del server (servizio App) vengono generati per l'app web.</span><span class="sxs-lookup"><span data-stu-id="8dd95-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="8dd95-172">Possono essere scaricati usando le informazioni FTP/FTPS visualizzate.</span><span class="sxs-lookup"><span data-stu-id="8dd95-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="8dd95-173">La password è quello utilizzato per le credenziali di distribuzione create in precedenza in questa Guida.</span><span class="sxs-lookup"><span data-stu-id="8dd95-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="8dd95-174">I log possono essere [trasmessi direttamente al computer locale con PowerShell o l'interfaccia della riga di comando di Azure](/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="8dd95-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="8dd95-175">I log possono essere [visualizzati anche in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="8dd95-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="8dd95-176">Streaming dei log</span><span class="sxs-lookup"><span data-stu-id="8dd95-176">Log streaming</span></span>

<span data-ttu-id="8dd95-177">I log del server web e dell'App possono essere trasmessi in tempo reale tramite il portale.</span><span class="sxs-lookup"><span data-stu-id="8dd95-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="8dd95-178">Aprire il [portale di Azure](https://portal.azure.com)e passare al *\<MyWebApp unique_number\>* servizio app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="8dd95-179">Nel menu a sinistra scorrere verso il basso fino alla sezione **monitoraggio** e selezionare **flusso di registrazione**.</span><span class="sxs-lookup"><span data-stu-id="8dd95-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Screenshot che mostra il collegamento flusso log](./media/monitoring/log-stream.png)

<span data-ttu-id="8dd95-181">I log possono essere trasmessi anche tramite l'interfaccia della riga di comando di [Azure o Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), anche tramite l'cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="8dd95-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="8dd95-182">Avvisi</span><span class="sxs-lookup"><span data-stu-id="8dd95-182">Alerts</span></span>

<span data-ttu-id="8dd95-183">Monitoraggio di Azure fornisce anche [avvisi in tempo reale](/azure/monitoring-and-diagnostics/insights-alerts-portal) in base a metriche, eventi amministrativi e altri criteri.</span><span class="sxs-lookup"><span data-stu-id="8dd95-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="8dd95-184">*Nota: attualmente gli avvisi sulle metriche dell'app Web sono disponibili solo nel servizio avvisi (versione classica).*</span><span class="sxs-lookup"><span data-stu-id="8dd95-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="8dd95-185">Il [servizio avvisi (versione classica)](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) si trova in monitoraggio di Azure o nella sezione **monitoraggio** delle impostazioni del servizio app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Collegamento di avvisi (versione classica)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="8dd95-187">Il debug attivo</span><span class="sxs-lookup"><span data-stu-id="8dd95-187">Live debugging</span></span>

<span data-ttu-id="8dd95-188">App Azure servizio può essere sottoposto [a debug in remoto con Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) quando i log non forniscono informazioni sufficienti.</span><span class="sxs-lookup"><span data-stu-id="8dd95-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="8dd95-189">Tuttavia, il debug remoto richiede l'app deve essere compilato con simboli di debug.</span><span class="sxs-lookup"><span data-stu-id="8dd95-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="8dd95-190">Debug non deve essere eseguito nell'ambiente di produzione, ad eccezione come ultima risorsa.</span><span class="sxs-lookup"><span data-stu-id="8dd95-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="8dd95-191">Conclusioni</span><span class="sxs-lookup"><span data-stu-id="8dd95-191">Conclusion</span></span>

<span data-ttu-id="8dd95-192">In questa sezione sono completate le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="8dd95-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="8dd95-193">Ricerca di base di monitoraggio e risoluzione dei problemi dei dati nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="8dd95-194">Informazioni su come monitoraggio di Azure offre un approfondimento sul metriche in tutti i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="8dd95-195">Connettere l'app web con Application Insights per la profilatura delle app</span><span class="sxs-lookup"><span data-stu-id="8dd95-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="8dd95-196">Abilitare la registrazione e informazioni su come scaricare i log</span><span class="sxs-lookup"><span data-stu-id="8dd95-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="8dd95-197">Stream log in tempo reale</span><span class="sxs-lookup"><span data-stu-id="8dd95-197">Stream logs in real time</span></span>
* <span data-ttu-id="8dd95-198">Informazioni su dove impostare gli avvisi</span><span class="sxs-lookup"><span data-stu-id="8dd95-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="8dd95-199">Informazioni su remote debug Azure App Service web app.</span><span class="sxs-lookup"><span data-stu-id="8dd95-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="8dd95-200">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8dd95-200">Additional reading</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="8dd95-201">Monitoraggio delle prestazioni dell'applicazione web di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="8dd95-202">Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="8dd95-203">Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8dd95-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="8dd95-204">Creare avvisi delle metriche classici in monitoraggio di Azure per i servizi di Azure-portale di Azure</span><span class="sxs-lookup"><span data-stu-id="8dd95-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
