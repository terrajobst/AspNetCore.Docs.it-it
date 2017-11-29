---
title: Distribuzione continua in Azure con Visual Studio e Git
author: rick-anderson
description: Informazioni su come creare un'app web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure, usando Git per la distribuzione continua.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="d9001-104">Distribuzione continua in Azure per ASP.NET Core con Visual Studio e Git</span><span class="sxs-lookup"><span data-stu-id="d9001-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="d9001-105">Di [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="d9001-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="d9001-106">Questa esercitazione mostra come creare un'app web ASP.NET Core tramite Visual Studio e distribuirla da Visual Studio nel Servizio app di Azure, usando Git la distribuzione continua.</span><span class="sxs-lookup"><span data-stu-id="d9001-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="d9001-107">Vedere anche [Usare VSTS per Compilare e Pubblicare un'App Web di Azure con una distribuzione continua](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), che illustra come configurare un flusso di lavoro di recapito continuo per [Servizio App di Azure](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) con Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d9001-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="d9001-108">Il recapito continuo di Azure in Team Services semplifica la configurazione di una pipeline di distribuzione solida per pubblicare gli aggiornamenti per l'app cel servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="d9001-109">È possibile configurare la pipeline dal portale di Azure per compilare, eseguire i test, distribuire in uno slot di staging e quindi distribuire nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d9001-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="d9001-110">Per completare questa esercitazione, è necessario un account di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="d9001-111">Se non si dispone di un account, è possibile [attivare i benefici per il sottoscrittore MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) o [iscriversi per una versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d9001-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9001-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d9001-112">Prerequisites</span></span>

<span data-ttu-id="d9001-113">Questa esercitazione presuppone che è già stato installato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d9001-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="d9001-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9001-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="d9001-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime e strumenti)</span><span class="sxs-lookup"><span data-stu-id="d9001-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="d9001-116">[Git](https://git-scm.com/downloads) per Windows</span><span class="sxs-lookup"><span data-stu-id="d9001-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="d9001-117">Creare un'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9001-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="d9001-118">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9001-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="d9001-119">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="d9001-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="d9001-120">Selezionare il modello di progetto **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d9001-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="d9001-121">Viene visualizzato in **Modelli** > **installati** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d9001-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="d9001-122">Denominare il progetto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="d9001-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="d9001-123">Selezionare l'opzione **Creare un nuovo repository Git** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9001-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Finestra di dialogo Nuovo progetto](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="d9001-125">Nella finestra di dialogo **Nuovo progetto ASP.NET Core** selezionare il modello ASP.NET Core **Vuoto** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9001-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Nuova finestra di dialogo Progetto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="d9001-127">La versione più recente di .NET Core è la 2.0.</span><span class="sxs-lookup"><span data-stu-id="d9001-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="d9001-128">Esecuzione dell'applicazione Web in locale</span><span class="sxs-lookup"><span data-stu-id="d9001-128">Running the web app locally</span></span>

1. <span data-ttu-id="d9001-129">Una volta terminata la creazione dell'app da parte di Visual Studio, eseguire l'app selezionando **Debug** -> **Avvia l'esecuzione del debug**.</span><span class="sxs-lookup"><span data-stu-id="d9001-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="d9001-130">In alternativa è possibile premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="d9001-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="d9001-131">L'inizializzazione di Visual Studio e della nuova app potrebbe richiedere tempo.</span><span class="sxs-lookup"><span data-stu-id="d9001-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="d9001-132">Una volta completata, il browser visualizzerà l'app in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d9001-132">Once it is complete, the browser will show the running app.</span></span>

   ![La finestra del browser mostra l'applicazione in esecuzione che visualizza "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="d9001-134">Dopo aver revisionato l'app Web in esecuzione, chiudere il browser e fare clic sull'icona "Interrompi l'esecuzione del debug" nella barra degli strumenti di Visual Studio per arrestare l'app.</span><span class="sxs-lookup"><span data-stu-id="d9001-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="d9001-135">Creare un'app web nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="d9001-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="d9001-136">La procedura seguente guiderà nella creazione di un'app web nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="d9001-137">Registrarsi al [Portale di Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="d9001-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="d9001-138">TOOCARE **NUOVO** nella parte superiore sinistra del Portale</span><span class="sxs-lookup"><span data-stu-id="d9001-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="d9001-139">TOCCARE **Web + Dispositivi mobili** > **App Web**</span><span class="sxs-lookup"><span data-stu-id="d9001-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Portale di Microsoft Azure: pulsante Nuovo: Web + Dispositivi mobili in Marketplace: pulsante Web App in App in primo piano](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="d9001-141">Nel pannello **App Web** immettere un valore univoco per il **Nome del servizio App**.</span><span class="sxs-lookup"><span data-stu-id="d9001-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Pannello App Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="d9001-143">Il nome **Nome del servizio App** deve essere univoco.</span><span class="sxs-lookup"><span data-stu-id="d9001-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="d9001-144">Quando si tenta di immettere il nome, il portale applicherà questa regola.</span><span class="sxs-lookup"><span data-stu-id="d9001-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="d9001-145">Dopo avere immesso un valore diverso, è necessario sostituire tale valore per ogni occorrenza di **SampleWebAppDemo** visualizzata in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d9001-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="d9001-146">Anche nel pannello **App Web**, selezionare una **Posizione/piano di servizio App** o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="d9001-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="d9001-147">Se si crea un nuovo piano, selezionare il piano tariffario, la posizione e altre opzioni.</span><span class="sxs-lookup"><span data-stu-id="d9001-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="d9001-148">Per altre informazioni sui piani del servizio App, [Panoramica di approfondimento dei piani del Servizio App di Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="d9001-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="d9001-149">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d9001-149">Click **Create**.</span></span> <span data-ttu-id="d9001-150">Azure eseguirà il provisioning e avvierà l'app web.</span><span class="sxs-lookup"><span data-stu-id="d9001-150">Azure will provision and start your web app.</span></span>

    ![Portale di Azure: campione di pannello di Essentials Demo 01 di App Web](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="d9001-152">Abilitare la pubblicazione di Git per la nuova app web</span><span class="sxs-lookup"><span data-stu-id="d9001-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="d9001-153">GIT è un sistema di controllo della versione distribuita che è possibile usare per distribuire l'app web del servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="d9001-154">Si archivierà il codice scritto per l'app web in un repository Git locale e il codice verrà distribuito in Azure eseguendo il push in un repository remoto.</span><span class="sxs-lookup"><span data-stu-id="d9001-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="d9001-155">Accedere al [portale di Azure](https://portal.azure.com), se non si è già connessi.</span><span class="sxs-lookup"><span data-stu-id="d9001-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="d9001-156">Fare clic su **Servizi app** per visualizzare un elenco dei servizi app associati alla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="d9001-157">Selezionare l'app web che è stata creata nella sezione precedente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d9001-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="d9001-158">Nel pannello **Distribuzione** selezionare **Opzioni di distribuzione** > **Scegli origine** > **Repository Git locale**.</span><span class="sxs-lookup"><span data-stu-id="d9001-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Pannello delle impostazioni: pannello di origine della distribuzione: scegliere Pannello di origine](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="d9001-160">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9001-160">Click **OK**.</span></span>

6. <span data-ttu-id="d9001-161">Se le credenziali di distribuzione per la pubblicazione di un'app web o di altri servizi app web non sono state precedentemente configurate, impostarle ora:</span><span class="sxs-lookup"><span data-stu-id="d9001-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="d9001-162">Fare clic su **impostazioni** > **Credenziali di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="d9001-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="d9001-163">Il pannello **Impostare le credenziali di distribuzione** verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d9001-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="d9001-164">Creare un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="d9001-164">Create a user name and password.</span></span>  <span data-ttu-id="d9001-165">Questa password sarà necessaria successivamente quando si configurerà Git.</span><span class="sxs-lookup"><span data-stu-id="d9001-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="d9001-166">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="d9001-166">Click **Save**.</span></span>

7. <span data-ttu-id="d9001-167">Nel pannello **App Web** fare clic su **Impostazioni** > **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="d9001-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="d9001-168">L'URL del repository Git remoto che verrà distribuita è visualizzata in **URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="d9001-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="d9001-169">Copiare il valore **URL GIT** per un uso successivo nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d9001-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portale di Azure: pannello Proprietà dell'applicazione](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="d9001-171">Pubblicare l'app Web nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="d9001-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="d9001-172">In questa sezione si creerà un repository Git locale, tramite Visual Studio, e si eseguirà il push da quel repository in Azure per distribuire l'app web.</span><span class="sxs-lookup"><span data-stu-id="d9001-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="d9001-173">La procedura coinvolta include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d9001-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="d9001-174">Aggiungere l'impostazione del repository remoto tramite il valore di URL GIT, in modo da poter distribuire il repository locale in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="d9001-175">Eseguire il commit delle modifiche del progetto.</span><span class="sxs-lookup"><span data-stu-id="d9001-175">Commit your project changes.</span></span>

   * <span data-ttu-id="d9001-176">Effettuare il push delle modifiche di progetto dal repository locale al repository remoto in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="d9001-177">In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**.</span><span class="sxs-lookup"><span data-stu-id="d9001-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="d9001-178">Il **Team Explorer** verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d9001-178">The **Team Explorer** will be displayed.</span></span>

    ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="d9001-180">In **Team Explorer** selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni del Repository**.</span><span class="sxs-lookup"><span data-stu-id="d9001-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="d9001-181">Nella sezione **Remoti** delle **Impostazioni del Repository** selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="d9001-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="d9001-182">La finestra di dialogo **Aggiungi remoto** verrà visualizzata.</span><span class="sxs-lookup"><span data-stu-id="d9001-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="d9001-183">Impostare il **Nome** del repository remoto da **Azure SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="d9001-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="d9001-184">Impostare il valore per **Recupero** per l'**URL Git** precedentemente copiato da Azure in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d9001-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="d9001-185">Si noti che questo è l'URL che termina con **.git**.</span><span class="sxs-lookup"><span data-stu-id="d9001-185">Note that this is the URL that ends with **.git**.</span></span>

    ![Modifica finestra di dialogo remota](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="d9001-187">In alternativa è possibile specificare il repository remoto dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo il comando.</span><span class="sxs-lookup"><span data-stu-id="d9001-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="d9001-188">Ad esempio:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="d9001-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="d9001-189">Selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni globali**.</span><span class="sxs-lookup"><span data-stu-id="d9001-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="d9001-190">Accertarsi di disporre del nome e dell'indirizzo di posta elettronica impostato.</span><span class="sxs-lookup"><span data-stu-id="d9001-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="d9001-191">È anche necessario selezionare **Aggiornamento**.</span><span class="sxs-lookup"><span data-stu-id="d9001-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="d9001-192">Selezionare **Home** > **Modifiche** da restituire alla vista **Modifiche**.</span><span class="sxs-lookup"><span data-stu-id="d9001-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="d9001-193">Immettere un messaggio di commit, ad esempio **Push iniziale #1** e fare clic su **Commit**.</span><span class="sxs-lookup"><span data-stu-id="d9001-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="d9001-194">Questa azione consente di creare un *Commit* a livello locale.</span><span class="sxs-lookup"><span data-stu-id="d9001-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="d9001-195">Successivamente, è necessaria la *sincronizzazione* con Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-195">Next, you need to *sync* with Azure.</span></span>

    ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="d9001-197">In alternativa è possibile eseguire il commit delle modifiche dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo i comandi git.</span><span class="sxs-lookup"><span data-stu-id="d9001-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="d9001-198">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9001-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="d9001-199">Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Aprire il prompt dei comandi**.</span><span class="sxs-lookup"><span data-stu-id="d9001-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="d9001-200">Il prompt dei comandi aprirà la directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="d9001-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="d9001-201">Immettere il comando seguente nella finestra Comando:</span><span class="sxs-lookup"><span data-stu-id="d9001-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="d9001-202">Immettere la password delle **le credenziali di distribuzione** di Azure creata precedentemente in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="d9001-203">La password non sarà visibile mentre viene immessa.</span><span class="sxs-lookup"><span data-stu-id="d9001-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="d9001-204">Questo comando avvierà il processo di esecuzione del push dei file di progetto locali in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="d9001-205">L'output del comando precedente termina con un messaggio che indica che la distribuzione ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="d9001-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="d9001-206">Se è necessario collaborare ad un progetto, è necessario considerare di eseguire il push su [GitHub](https://github.com) tra l'inserimento in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="d9001-207">Verificare la distribuzione attiva</span><span class="sxs-lookup"><span data-stu-id="d9001-207">Verify the Active Deployment</span></span>

<span data-ttu-id="d9001-208">È possibile verificare di aver trasferito con esito positivo l'app web dall'ambiente locale in Azure.</span><span class="sxs-lookup"><span data-stu-id="d9001-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="d9001-209">Verrà visualizzata la corretta distribuzione elencata.</span><span class="sxs-lookup"><span data-stu-id="d9001-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="d9001-210">Nel [portale Azure](https://portal.azure.com) selezionare l'app web.</span><span class="sxs-lookup"><span data-stu-id="d9001-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="d9001-211">Selezionare quindi **Distribuzione** > **Opzioni di distribuzione**.</span><span class="sxs-lookup"><span data-stu-id="d9001-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Portale di Azure: pannello impostazioni: pannello di distribuzione che mostra la distribuzione con esito positivo](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="d9001-213">Eseguire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="d9001-213">Run the app in Azure</span></span>

<span data-ttu-id="d9001-214">Ora che è stato distribuita l'app web in Azure, è possibile eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d9001-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="d9001-215">Questa operazione può essere eseguita nei due modi seguenti.</span><span class="sxs-lookup"><span data-stu-id="d9001-215">This can be done in two ways:</span></span>

* <span data-ttu-id="d9001-216">Nel portale di Azure posizionare il pannello dell'app web per l'app web e fare clic su **Sfoglia** per visualizzare l'app nel browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="d9001-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="d9001-217">Aprire un browser e immettere l'URL per l'app web.</span><span class="sxs-lookup"><span data-stu-id="d9001-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="d9001-218">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9001-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="d9001-219">Aggiornare l'app web e pubblicare di nuovo</span><span class="sxs-lookup"><span data-stu-id="d9001-219">Update your web app and republish</span></span>

<span data-ttu-id="d9001-220">Dopo aver apportato delle modifiche al codice locale, è possibile pubblicare di nuovo.</span><span class="sxs-lookup"><span data-stu-id="d9001-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="d9001-221">In **Esplora soluzioni** di Visual Studio aprire il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d9001-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="d9001-222">Nel metodo `Configure` modificare il metodo `Response.WriteAsync` in modo che venga visualizzato nel seguente modo:</span><span class="sxs-lookup"><span data-stu-id="d9001-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="d9001-223">Salvare le modifiche a *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d9001-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="d9001-224">In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**.</span><span class="sxs-lookup"><span data-stu-id="d9001-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="d9001-225">Il **Team Explorer** verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d9001-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="d9001-226">Immettere un messaggio per il commit, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9001-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="d9001-227">Premere il pulsante **Commit** per salvare le modifiche del progetto.</span><span class="sxs-lookup"><span data-stu-id="d9001-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="d9001-228">Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Push**.</span><span class="sxs-lookup"><span data-stu-id="d9001-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="d9001-229">In alternativa è possibile eseguire il push delle modifiche dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo un comando git.</span><span class="sxs-lookup"><span data-stu-id="d9001-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="d9001-230">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9001-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="d9001-231">Visualizzare l'app web aggiornata in Azure</span><span class="sxs-lookup"><span data-stu-id="d9001-231">View the updated web app in Azure</span></span>

<span data-ttu-id="d9001-232">Visualizzare l'app web aggiornata selezionando **Sfoglia** dal pannello dell'app web nel Portale di Azure o aprendo un browser e immettendo l'URL per l'app web.</span><span class="sxs-lookup"><span data-stu-id="d9001-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="d9001-233">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9001-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="d9001-234">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d9001-234">Additional Resources</span></span>

* [<span data-ttu-id="d9001-235">Pubblicazione e Distribuzione</span><span class="sxs-lookup"><span data-stu-id="d9001-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="d9001-236">Kudu del progetto</span><span class="sxs-lookup"><span data-stu-id="d9001-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
