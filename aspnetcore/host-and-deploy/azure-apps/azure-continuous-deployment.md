---
title: Distribuzione continua in Azure con Visual Studio e Git con ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="ccfd7-103">Distribuzione continua in Azure con Visual Studio e Git con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ccfd7-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="ccfd7-104">Di [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="ccfd7-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="ccfd7-105">In questa esercitazione viene illustrato come creare un'app web ASP.NET Core con Visual Studio e distribuirlo da Visual Studio al servizio App di Azure utilizzando la distribuzione continua.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="ccfd7-106">Vedere anche [Usare VSTS per Compilare e Pubblicare un'App Web di Azure con una distribuzione continua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), che illustra come configurare un flusso di lavoro di recapito continuo per [Servizio App di Azure](/azure/app-service/app-service-web-overview) con Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="ccfd7-107">Il recapito continuo Azure Team Services semplifica la configurazione di una pipeline di distribuzione affidabile per pubblicare gli aggiornamenti per le app ospitate in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="ccfd7-108">È possibile configurare la pipeline dal portale di Azure per compilare, eseguire i test, distribuire uno slot di gestione temporanea e quindi distribuire nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="ccfd7-109">Per completare questa esercitazione, è necessario un account di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="ccfd7-110">Per ottenere un account, [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) o [iscriversi per una valutazione gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="ccfd7-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccfd7-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ccfd7-111">Prerequisites</span></span>

<span data-ttu-id="ccfd7-112">Questa esercitazione si presuppone che viene installato il software seguente:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="ccfd7-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ccfd7-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="ccfd7-114">[Git](https://git-scm.com/downloads) per Windows</span><span class="sxs-lookup"><span data-stu-id="ccfd7-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="ccfd7-115">Creare un'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ccfd7-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="ccfd7-116">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="ccfd7-117">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="ccfd7-118">Selezionare il modello di progetto **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="ccfd7-119">Viene visualizzato in **Modelli** > **installati** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="ccfd7-120">Denominare il progetto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="ccfd7-121">Selezionare il **Crea nuovo repository Git** opzione e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Finestra di dialogo Nuovo progetto](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="ccfd7-123">Nella finestra di dialogo **Nuovo progetto ASP.NET Core** selezionare il modello ASP.NET Core **Vuoto** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Nuova finestra di dialogo Progetto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="ccfd7-125">La versione più recente di .NET Core è 2.0.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="ccfd7-126">Esecuzione dell'applicazione Web in locale</span><span class="sxs-lookup"><span data-stu-id="ccfd7-126">Running the web app locally</span></span>

1. <span data-ttu-id="ccfd7-127">Una volta terminata la creazione dell'app da parte di Visual Studio, eseguire l'app selezionando **Debug** > **Avvia l'esecuzione del debug**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="ccfd7-128">In alternativa, premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="ccfd7-129">L'inizializzazione di Visual Studio e della nuova app potrebbe richiedere tempo.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="ccfd7-130">Una volta completata, il browser visualizza app in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-130">Once it's complete, the browser shows the running app.</span></span>

   ![La finestra del browser mostra l'applicazione in esecuzione che visualizza "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="ccfd7-132">Dopo aver esaminato l'app Web in esecuzione, chiudere il browser e selezionare l'icona "Interrompi debug" nella barra degli strumenti di Visual Studio per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="ccfd7-133">Creare un'app web nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="ccfd7-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="ccfd7-134">La procedura seguente crea un'app web nel portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="ccfd7-135">Accedere al [portale di Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ccfd7-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="ccfd7-136">Selezionare **NEW** nella parte superiore sinistra dell'interfaccia del portale.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="ccfd7-137">Selezionare **Web e dispositivi mobili** > **App Web**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portale di Microsoft Azure: pulsante Nuovo: Web + Dispositivi mobili in Marketplace: pulsante Web App in App in primo piano](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="ccfd7-139">Nel pannello **App Web** immettere un valore univoco per il **Nome del servizio App**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Pannello App Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="ccfd7-141">Il **nome del servizio App** nome deve essere univoco.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="ccfd7-142">Il portale applica questa regola quando viene specificato il nome.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="ccfd7-143">Se fornire un valore diverso, sostituire tale valore per ogni occorrenza di **SampleWebAppDemo** in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="ccfd7-144">Anche nel pannello **App Web**, selezionare una **Posizione/piano di servizio App** o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="ccfd7-145">Se si crea un nuovo piano, selezionare il piano tariffario, percorso e altre opzioni.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="ccfd7-146">Per ulteriori informazioni sui piani di servizio App, vedere [piani di servizio App di Azure ad approfondimenti su](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="ccfd7-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="ccfd7-147">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-147">Select **Create**.</span></span> <span data-ttu-id="ccfd7-148">Azure sarà il provisioning e avviare l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-148">Azure will provision and start the web app.</span></span>

   ![Portale di Azure: campione di pannello di Essentials Demo 01 di App Web](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="ccfd7-150">Abilitare la pubblicazione di Git per la nuova app web</span><span class="sxs-lookup"><span data-stu-id="ccfd7-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="ccfd7-151">GIT è un sistema di controllo di versione distribuita che può essere usato per distribuire un'app web di servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="ccfd7-152">Codice di app Web è archiviato in un repository Git locale e il codice viene distribuito in Azure mediante il push in un repository remoto.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="ccfd7-153">Accedere al [portale di Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ccfd7-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="ccfd7-154">Selezionare **servizi App** per visualizzare un elenco dei servizi di app associato alla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="ccfd7-155">Selezionare l'app web creata nella sezione precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="ccfd7-156">Nel pannello **Distribuzione** selezionare **Opzioni di distribuzione** > **Scegli origine** > **Repository Git locale**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Pannello delle impostazioni: pannello di origine della distribuzione: scegliere Pannello di origine](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="ccfd7-158">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-158">Select **OK**.</span></span>

1. <span data-ttu-id="ccfd7-159">Se le credenziali di distribuzione per la pubblicazione di un'app web o altre app di servizio App in precedenza non è ancora state configurate, impostarli ora:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="ccfd7-160">Selezionare **impostazioni** > **le credenziali di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="ccfd7-161">Il **impostare le credenziali di distribuzione** pannello viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="ccfd7-162">Creare un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-162">Create a user name and password.</span></span> <span data-ttu-id="ccfd7-163">Salvare la password per un uso successivo durante la configurazione di Git.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="ccfd7-164">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-164">Select **Save**.</span></span>

1. <span data-ttu-id="ccfd7-165">Nel **App Web** pannello seleziona **impostazioni** > **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="ccfd7-166">Viene visualizzato l'URL del repository Git remoto per distribuire in **URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="ccfd7-167">Copiare il valore **URL GIT** per un uso successivo nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portale di Azure: pannello Proprietà dell'applicazione](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="ccfd7-169">Pubblicare l'app web in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ccfd7-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="ccfd7-170">In questa sezione, creare un repository Git locale, tramite Visual Studio e push da quel repository in Azure per distribuire l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="ccfd7-171">La procedura coinvolta include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-171">The steps involved include the following:</span></span>

* <span data-ttu-id="ccfd7-172">Aggiungere l'impostazione di repository remoto utilizzando il valore di URL GIT, pertanto il repository locale può essere distribuito in Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="ccfd7-173">Eseguire il commit delle modifiche al progetto.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-173">Commit project changes.</span></span>
* <span data-ttu-id="ccfd7-174">Push delle modifiche di progetto dall'archivio locale nel repository remoto in Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="ccfd7-175">In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="ccfd7-176">Il **Team Explorer** viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-176">The **Team Explorer** is displayed.</span></span>

   ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="ccfd7-178">In **Team Explorer** selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni del Repository**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="ccfd7-179">Nel **elementi remoti** sezione del **delle impostazioni Repository**selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="ccfd7-180">Il **aggiungere remoto** viene visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="ccfd7-181">Impostare il **Nome** del repository remoto da **Azure SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="ccfd7-182">Impostare il valore per **recuperare** per il **URL Git** che copiato da Azure precedentemente in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="ccfd7-183">Si noti che questo è l'URL che termina con **.git**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Modifica finestra di dialogo remota](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="ccfd7-185">In alternativa, specificare il repository remoto dal **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettendo il comando.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="ccfd7-186">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="ccfd7-187">Selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni globali**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="ccfd7-188">Confermare che il nome e l'indirizzo e-mail siano impostati.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="ccfd7-189">Selezionare **aggiornamento** se necessario.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="ccfd7-190">Selezionare **Home** > **Modifiche** da restituire alla vista **Modifiche**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="ccfd7-191">Immettere un messaggio di commit, ad esempio **iniziale Push #1** e selezionare **Commit**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="ccfd7-192">Questa azione crea un *commit* localmente.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-192">This action creates a *commit* locally.</span></span>

   ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="ccfd7-194">In alternativa, commit cambia dal **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettendo i comandi git.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="ccfd7-195">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="ccfd7-196">Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Aprire il prompt dei comandi**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="ccfd7-197">Consente di aprire il prompt dei comandi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="ccfd7-198">Immettere il comando seguente nella finestra Comando:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="ccfd7-199">Immettere Azure **le credenziali di distribuzione** password creata in precedenza in Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="ccfd7-200">Questo comando avvia il processo di push dei file di progetto locali in Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="ccfd7-201">L'output del comando precedente termina con un messaggio che la distribuzione ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="ccfd7-202">Collaborazione per il progetto è necessario, provare a push [GitHub](https://github.com) prima dell'inserimento di Azure.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="ccfd7-203">Verificare la distribuzione attiva</span><span class="sxs-lookup"><span data-stu-id="ccfd7-203">Verify the Active Deployment</span></span>

<span data-ttu-id="ccfd7-204">Verificare che il trasferimento di app web dall'ambiente locale a Azure ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="ccfd7-205">Nel [portale Azure](https://portal.azure.com), selezionare l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="ccfd7-206">Selezionare **distribuzione** > **opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-206">Select **Deployment** > **Deployment options**.</span></span>

![Portale di Azure: pannello impostazioni: pannello di distribuzione che mostra la distribuzione con esito positivo](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="ccfd7-208">Eseguire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="ccfd7-208">Run the app in Azure</span></span>

<span data-ttu-id="ccfd7-209">Ora che l'app web viene distribuito in Azure, eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="ccfd7-210">Questa operazione può essere eseguita in due modi:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="ccfd7-211">Nel portale di Azure, individuare il pannello app web per l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="ccfd7-212">Selezionare **Sfoglia** per visualizzare l'app nel browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="ccfd7-213">Aprire un browser e immettere l'URL per l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="ccfd7-214">Esempio: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="ccfd7-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="ccfd7-215">Aggiornare l'app web e pubblicare di nuovo</span><span class="sxs-lookup"><span data-stu-id="ccfd7-215">Update the web app and republish</span></span>

<span data-ttu-id="ccfd7-216">Dopo aver apportato modifiche al codice locale, pubblicare di nuovo:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="ccfd7-217">In **Esplora soluzioni** di Visual Studio aprire il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="ccfd7-218">Nel metodo `Configure` modificare il metodo `Response.WriteAsync` in modo che venga visualizzato nel seguente modo:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="ccfd7-219">Salvare le modifiche in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="ccfd7-220">In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="ccfd7-221">Il **Team Explorer** viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="ccfd7-222">Immettere un messaggio di commit, ad esempio `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="ccfd7-223">Premere il pulsante **Commit** per salvare le modifiche del progetto.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="ccfd7-224">Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="ccfd7-225">In alternativa, le modifiche da eseguire il push di **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettere un comando di git.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="ccfd7-226">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ccfd7-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="ccfd7-227">Visualizzare l'app web aggiornata in Azure</span><span class="sxs-lookup"><span data-stu-id="ccfd7-227">View the updated web app in Azure</span></span>

<span data-ttu-id="ccfd7-228">Visualizzare l'app web sono aggiornate selezionando **Sfoglia** nel Pannello di app web nel portale di Azure o aprendo un browser e immettendo l'URL per l'app web.</span><span class="sxs-lookup"><span data-stu-id="ccfd7-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="ccfd7-229">Esempio: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="ccfd7-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccfd7-230">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ccfd7-230">Additional resources</span></span>

* [<span data-ttu-id="ccfd7-231">Utilizzare Visual Studio Team Services per compilare e pubblicare un'App Web di Azure con la distribuzione continua</span><span class="sxs-lookup"><span data-stu-id="ccfd7-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="ccfd7-232">Kudu del progetto</span><span class="sxs-lookup"><span data-stu-id="ccfd7-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
