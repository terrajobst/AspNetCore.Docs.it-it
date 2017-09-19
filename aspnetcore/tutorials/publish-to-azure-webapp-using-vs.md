---
title: Pubblicare un'app ASP.NET Core in Azure con Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="6ab56-103">Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab56-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="6ab56-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6ab56-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="6ab56-105">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="6ab56-105">Set up the development environment</span></span>

* <span data-ttu-id="6ab56-106">Installare la versione più recente di [Azure SDK per Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span><span class="sxs-lookup"><span data-stu-id="6ab56-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span></span> <span data-ttu-id="6ab56-107">L'SDK installa Visual Studio, se non è già disponibile.</span><span class="sxs-lookup"><span data-stu-id="6ab56-107">The SDK installs Visual Studio if you don't already have it.</span></span>

* <span data-ttu-id="6ab56-108">Verificare l'[account Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6ab56-108">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="6ab56-109">È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/) o [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="6ab56-109">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="6ab56-110">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="6ab56-110">Create a web app</span></span>

<span data-ttu-id="6ab56-111">Nella Pagina iniziale di Visual Studio selezionare **File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="6ab56-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![File (menu)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="6ab56-113">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="6ab56-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6ab56-114">Nel riquadro a sinistra selezionare **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-114">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="6ab56-115">Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="6ab56-116">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-116">Select **OK**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="6ab56-118">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="6ab56-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="6ab56-119">Selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-119">Select **Web Application**.</span></span>

* <span data-ttu-id="6ab56-120">Selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-120">Select **Change Authentication**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="6ab56-122">Viene visualizzata la finestra di dialogo **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="6ab56-123">Selezionare **Account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-123">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="6ab56-124">Selezionare **OK** per tornare a **Nuova applicazione Web ASP.NET Core** e selezionare nuovamente **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Finestra per autenticazione Nuova applicazione Web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="6ab56-126">Visual Studio crea la soluzione.</span><span class="sxs-lookup"><span data-stu-id="6ab56-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="6ab56-127">Eseguire l'app in locale</span><span class="sxs-lookup"><span data-stu-id="6ab56-127">Run the app locally</span></span>

* <span data-ttu-id="6ab56-128">Per eseguire l'app in locale, scegliere **Debug** e **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-128">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="6ab56-129">Fare clic sui collegamenti **Informazioni su** e su **Contatto** per verificare che l'applicazione Web funzioni.</span><span class="sxs-lookup"><span data-stu-id="6ab56-129">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="6ab56-131">Selezionare **Registra** e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="6ab56-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="6ab56-132">È possibile usare un indirizzo di posta elettronica fittizio.</span><span class="sxs-lookup"><span data-stu-id="6ab56-132">You can use a fictitious email address.</span></span> <span data-ttu-id="6ab56-133">Quando si esegue l'invio, nella pagina viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="6ab56-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="6ab56-134">*"Internal Server Error: A database operation failed while processing the request. Per un'eccezione SQL, non è possibile aprire il database. Applying existing migrations for Application DB context may resolve this issue."* (Errore interno del server: Operazione sul database non riuscita durante l'elaborazione della richiesta. Eccezione SQL: Impossibile aprire il file di database. Per risolvere il problema, applicare le migrazioni esistenti per il contesto di database dell'applicazione).</span><span class="sxs-lookup"><span data-stu-id="6ab56-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="6ab56-135">Selezionare **Apply Migrations** (Applica migrazioni) e, quando la pagina è stata caricata, eseguire l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6ab56-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="6ab56-139">L'app visualizza l'indirizzo di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnessione**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Applicazione Web aperta in Microsoft Edge](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="6ab56-142">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="6ab56-142">Deploy the app to Azure</span></span>

<span data-ttu-id="6ab56-143">Chiudere la pagina Web, tornare a Visual Studio e selezionare **Termina debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-143">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="6ab56-144">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="6ab56-146">Nella finestra di dialogo **Pubblica** selezionare **Servizio app di Microsoft Azure** e fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-146">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="6ab56-148">Assegnare all'app un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="6ab56-148">Name the app a unique name.</span></span> 

* <span data-ttu-id="6ab56-149">Selezionare una sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="6ab56-149">Select a subscription.</span></span>

* <span data-ttu-id="6ab56-150">Selezionare **Nuovo** per il gruppo di risorse e immettere un nome per il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="6ab56-150">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="6ab56-151">Selezionare **Nuovo ** per il piano di servizio app e selezionare una località nelle proprie vicinanze.</span><span class="sxs-lookup"><span data-stu-id="6ab56-151">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="6ab56-152">È possibile tenere il nome che viene generato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6ab56-152">You can keep the name that is generated by default.</span></span>

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="6ab56-154">Selezionare la scheda **Servizi** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="6ab56-154">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="6ab56-155">Selezionare l'icona verde **+** per creare un nuovo database SQL</span><span class="sxs-lookup"><span data-stu-id="6ab56-155">Select the green **+** icon to create a new SQL Database</span></span>

![Nuovo database SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="6ab56-157">Selezionare **Nuovo** nella finestra di dialogo **Configura database SQL** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="6ab56-157">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nuovo database SQL e server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="6ab56-159">Viene visualizzata la finestra di dialogo **Configura SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-159">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="6ab56-160">Immettere nome utente e password di amministratore e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-160">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="6ab56-161">Non dimenticare il nome utente e la password creati in questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="6ab56-161">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="6ab56-162">È possibile mantenere il **Nome server** predefinito.</span><span class="sxs-lookup"><span data-stu-id="6ab56-162">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="6ab56-163">Immettere un nome per la stringa di connessione e il database.</span><span class="sxs-lookup"><span data-stu-id="6ab56-163">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="6ab56-164">La stringa "admin" non è consentita come nome utente di amministratore.</span><span class="sxs-lookup"><span data-stu-id="6ab56-164">"admin" is not allowed as the administrator user name.</span></span>

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="6ab56-166">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-166">Select **OK**.</span></span>

<span data-ttu-id="6ab56-167">Visual Studio torna alla finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="6ab56-168">Selezionare **Crea** nella finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="6ab56-170">Fare clic sul collegamento **Impostazioni** della finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-170">Click the **Settings** link in the **Publish** dialog.</span></span>

![Finestra di dialogo Pubblica: pannello di connessione](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="6ab56-172">Nella pagina **Impostazioni** della finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="6ab56-172">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="6ab56-173">Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-173">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="6ab56-174">Espandere **Migrazioni Entity Framework** e selezionare **Applica questa migrazione in fase di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-174">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="6ab56-175">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-175">Select **Save**.</span></span> <span data-ttu-id="6ab56-176">Visual Studio torna alla finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-176">Visual Studio returns to the **Publish** dialog.</span></span> 

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="6ab56-178">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-178">Click **Publish**.</span></span> <span data-ttu-id="6ab56-179">Visual Studio pubblicherà l'app in Azure e avvierà l'app cloud nel browser.</span><span class="sxs-lookup"><span data-stu-id="6ab56-179">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="6ab56-180">Testare l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="6ab56-180">Test your app in Azure</span></span>

* <span data-ttu-id="6ab56-181">Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="6ab56-181">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="6ab56-182">Registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="6ab56-182">Register a new user</span></span>

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="6ab56-184">Aggiornare l'app</span><span class="sxs-lookup"><span data-stu-id="6ab56-184">Update the app</span></span>

* <span data-ttu-id="6ab56-185">Modificare la pagina Razor *Pages/About.cshtml* e modificarne il contenuto.</span><span class="sxs-lookup"><span data-stu-id="6ab56-185">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="6ab56-186">Ad esempio, è possibile modificare il paragrafo specificando "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="6ab56-186">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="6ab56-187">Fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-187">Right-click on the project and select **Publish...** again.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="6ab56-189">Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure.</span><span class="sxs-lookup"><span data-stu-id="6ab56-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verificare che l'attività sia stata completata](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="6ab56-191">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="6ab56-191">Clean up</span></span>

<span data-ttu-id="6ab56-192">Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.</span><span class="sxs-lookup"><span data-stu-id="6ab56-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="6ab56-193">Selezionare **Gruppi di risorse** e in seguito il gruppo di risorse che è stato creato.</span><span class="sxs-lookup"><span data-stu-id="6ab56-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="6ab56-195">Nella pagina **Gruppi di risorse** selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-195">In the **Resource groups** page, select **Delete**.</span></span>

![Portale di Azure: pagina Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="6ab56-197">Immettere il nome del gruppo di risorse e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="6ab56-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="6ab56-198">A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure.</span><span class="sxs-lookup"><span data-stu-id="6ab56-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6ab56-199">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6ab56-199">Next steps</span></span>

* [<span data-ttu-id="6ab56-200">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab56-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="6ab56-201">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ab56-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="6ab56-202">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="6ab56-202">Fundamentals</span></span>](../fundamentals/index.md)
