---
title: Pubblicare un'app ASP.NET Core in Azure con Visual Studio
author: rick-anderson
description: Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="e4c8f-104">/it-it</span><span class="sxs-lookup"><span data-stu-id="e4c8f-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="e4c8f-105">Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4c8f-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="e4c8f-106">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e4c8f-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e4c8f-107">Vedere [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Pubblicare in Azure da Visual Studio per Mac) se si usa un Mac.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="e4c8f-108">Impostare</span><span class="sxs-lookup"><span data-stu-id="e4c8f-108">Set up</span></span>

* <span data-ttu-id="e4c8f-109">Aprire un [account Azure gratuito](https://aka.ms/K5y5yh) se non è già disponibile un account.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="e4c8f-110">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="e4c8f-110">Create a web app</span></span>

<span data-ttu-id="e4c8f-111">Nella Pagina iniziale di Visual Studio selezionare **File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="e4c8f-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![File (menu)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="e4c8f-113">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e4c8f-114">Nel riquadro a sinistra selezionare **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="e4c8f-115">Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e4c8f-116">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-116">Select **OK**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="e4c8f-118">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="e4c8f-119">Selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-119">Select **Web Application**.</span></span>
* <span data-ttu-id="e4c8f-120">Selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-120">Select **Change Authentication**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="e4c8f-122">Viene visualizzata la finestra di dialogo **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="e4c8f-123">Selezionare **Account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="e4c8f-124">Selezionare **OK** per tornare a **Nuova applicazione Web ASP.NET Core** e selezionare nuovamente **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Finestra per autenticazione Nuova applicazione Web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="e4c8f-126">Visual Studio crea la soluzione.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e4c8f-127">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e4c8f-127">Run the app</span></span>

* <span data-ttu-id="e4c8f-128">Premere CTRL+F5 per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="e4c8f-129">Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="e4c8f-129">Test the **About** and **Contact** links.</span></span>

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="e4c8f-131">Registrare un utente</span><span class="sxs-lookup"><span data-stu-id="e4c8f-131">Register a user</span></span>

* <span data-ttu-id="e4c8f-132">Selezionare **Registra** e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="e4c8f-133">È possibile usare un indirizzo di posta elettronica fittizio.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-133">You can use a fictitious email address.</span></span> <span data-ttu-id="e4c8f-134">Quando si esegue l'invio, nella pagina viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="e4c8f-135">*"Internal Server Error: A database operation failed while processing the request. Per un'eccezione SQL, non è possibile aprire il database. Applying existing migrations for Application DB context may resolve this issue."* (Errore interno del server: Operazione sul database non riuscita durante l'elaborazione della richiesta. Eccezione SQL: Impossibile aprire il file di database. Per risolvere il problema, applicare le migrazioni esistenti per il contesto di database dell'applicazione).</span><span class="sxs-lookup"><span data-stu-id="e4c8f-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="e4c8f-136">Selezionare **Apply Migrations** (Applica migrazioni) e, quando la pagina è stata caricata, eseguire l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="e4c8f-140">L'app visualizza l'indirizzo di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnessione**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Applicazione Web aperta in Microsoft Edge](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="e4c8f-143">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-143">Deploy the app to Azure</span></span>

<span data-ttu-id="e4c8f-144">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="e4c8f-146">Nella finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="e4c8f-147">Selezionare **Servizio app di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="e4c8f-148">Selezionare l'icona a forma di ingranaggio e quindi selezionare **Crea profilo**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="e4c8f-149">Selezionare **Crea profilo**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-149">Select **Create Profile**.</span></span>

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="e4c8f-151">Creare risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-151">Create Azure resources</span></span>

<span data-ttu-id="e4c8f-152">Viene visualizzata la finestra di dialogo **Crea servizio app**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="e4c8f-153">Immettere la sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-153">Enter your subscription.</span></span>
* <span data-ttu-id="e4c8f-154">I campi di immissione **Nome dell'app**, **Gruppo di risorse** e **Piano di servizio app** vengono popolati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="e4c8f-155">È possibile mantenere questi nomi o modificarli.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-155">You can keep these names or change them.</span></span>

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="e4c8f-157">Selezionare la scheda **Servizi** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="e4c8f-158">Selezionare l'icona verde **+** per creare un nuovo database SQL</span><span class="sxs-lookup"><span data-stu-id="e4c8f-158">Select the green **+** icon to create a new SQL Database</span></span>

![Nuovo database SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="e4c8f-160">Selezionare **Nuovo** nella finestra di dialogo **Configura database SQL** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nuovo database SQL e server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="e4c8f-162">Viene visualizzata la finestra di dialogo **Configura SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="e4c8f-163">Immettere nome utente e password di amministratore e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="e4c8f-164">È possibile mantenere il **Nome server** predefinito.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="e4c8f-165">La stringa "admin" non è consentita come nome utente di amministratore.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-165">"admin" is not allowed as the administrator user name.</span></span>

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="e4c8f-167">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-167">Select **OK**.</span></span>

<span data-ttu-id="e4c8f-168">Visual Studio torna alla finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="e4c8f-169">Selezionare **Crea** nella finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="e4c8f-171">Visual Studio crea l'app Web e SQL Server in Azure.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="e4c8f-172">Questo passaggio potrebbe richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-172">This step can take a few minutes.</span></span> <span data-ttu-id="e4c8f-173">Per informazioni sulle risorse create, vedere [Risorse aggiuntive](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="e4c8f-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="e4c8f-174">Al termine della distribuzione, selezionare **impostazioni**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-174">When deployment completes, select **Settings**:</span></span>

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="e4c8f-176">Nella pagina **Impostazioni** della finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="e4c8f-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="e4c8f-177">Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="e4c8f-178">Espandere **Migrazioni Entity Framework** e selezionare **Applica questa migrazione in fase di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="e4c8f-179">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-179">Select **Save**.</span></span> <span data-ttu-id="e4c8f-180">Visual Studio torna alla finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="e4c8f-182">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-182">Click **Publish**.</span></span> <span data-ttu-id="e4c8f-183">Visual Studio pubblica l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="e4c8f-184">Al termine della distribuzione, l'app viene aperta in un browser.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="e4c8f-185">Testare l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-185">Test your app in Azure</span></span>

* <span data-ttu-id="e4c8f-186">Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="e4c8f-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="e4c8f-187">Registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="e4c8f-187">Register a new user</span></span>

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="e4c8f-189">Aggiornare l'app</span><span class="sxs-lookup"><span data-stu-id="e4c8f-189">Update the app</span></span>

* <span data-ttu-id="e4c8f-190">Modificare la pagina Razor *Pages/About.cshtml* e modificarne il contenuto.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="e4c8f-191">Ad esempio, è possibile modificare il paragrafo specificando "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="e4c8f-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="e4c8f-192">Fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-192">Right-click on the project and select **Publish...** again.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="e4c8f-194">Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verificare che l'attività sia stata completata](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="e4c8f-196">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="e4c8f-196">Clean up</span></span>

<span data-ttu-id="e4c8f-197">Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="e4c8f-198">Selezionare **Gruppi di risorse** e in seguito il gruppo di risorse che è stato creato.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="e4c8f-200">Nella pagina **Gruppi di risorse** selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-200">In the **Resource groups** page, select **Delete**.</span></span>

![Portale di Azure: pagina Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="e4c8f-202">Immettere il nome del gruppo di risorse e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="e4c8f-203">A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure.</span><span class="sxs-lookup"><span data-stu-id="e4c8f-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e4c8f-204">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e4c8f-204">Next steps</span></span>

* [<span data-ttu-id="e4c8f-205">Distribuzione continua in Azure con Visual Studio e Git</span><span class="sxs-lookup"><span data-stu-id="e4c8f-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="e4c8f-206">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4c8f-206">Additonal resources</span></span>

* [<span data-ttu-id="e4c8f-207">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="e4c8f-208">Gruppi di risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="e4c8f-209">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="e4c8f-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)