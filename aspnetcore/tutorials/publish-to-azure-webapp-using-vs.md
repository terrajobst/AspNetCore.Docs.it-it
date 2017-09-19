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
ms.openlocfilehash: 14ce45f0cd15b2de39f722767df076d2c0313787
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="6b5e8-103">Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b5e8-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="6b5e8-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6b5e8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="6b5e8-105">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="6b5e8-105">Set up the development environment</span></span>

* <span data-ttu-id="6b5e8-106">Installare gli [strumenti di .NET Core e Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306).</span><span class="sxs-lookup"><span data-stu-id="6b5e8-106">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306).</span></span>

* <span data-ttu-id="6b5e8-107">Verificare l'[account Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6b5e8-107">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="6b5e8-108">È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/) o [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="6b5e8-108">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="6b5e8-109">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="6b5e8-109">Create a web app</span></span>

<span data-ttu-id="6b5e8-110">Nella Pagina iniziale di Visual Studio selezionare **File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="6b5e8-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![File (menu)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="6b5e8-112">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="6b5e8-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6b5e8-113">Nel riquadro a sinistra selezionare **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-113">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="6b5e8-114">Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="6b5e8-115">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-115">Select **OK**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="6b5e8-117">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="6b5e8-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="6b5e8-118">Selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-118">Select **Web Application**.</span></span>

* <span data-ttu-id="6b5e8-119">Selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-119">Select **Change Authentication**.</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="6b5e8-121">Viene visualizzata la finestra di dialogo **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="6b5e8-122">Selezionare **Account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-122">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="6b5e8-123">Selezionare **OK** per tornare a **Nuova applicazione Web ASP.NET Core** e selezionare nuovamente **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Finestra per autenticazione Nuova applicazione Web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="6b5e8-125">Visual Studio crea la soluzione.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="6b5e8-126">Eseguire l'app in locale</span><span class="sxs-lookup"><span data-stu-id="6b5e8-126">Run the app locally</span></span>

* <span data-ttu-id="6b5e8-127">Per eseguire l'app in locale, scegliere **Debug** e **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-127">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="6b5e8-128">Fare clic sui collegamenti **Informazioni su** e su **Contatto** per verificare che l'applicazione Web funzioni.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-128">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="6b5e8-130">Selezionare **Registra** e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="6b5e8-131">È possibile usare un indirizzo di posta elettronica fittizio.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-131">You can use a fictitious email address.</span></span> <span data-ttu-id="6b5e8-132">Quando si esegue l'invio, nella pagina viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="6b5e8-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="6b5e8-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."* (Errore interno del server: Operazione sul database non riuscita durante l'elaborazione della richiesta. Eccezione SQL: Impossibile aprire il file di database. Per risolvere il problema, applicare le migrazioni esistenti per il contesto di database dell'applicazione).</span><span class="sxs-lookup"><span data-stu-id="6b5e8-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="6b5e8-134">Selezionare **Apply Migrations** (Applica migrazioni) e, quando la pagina è stata caricata, eseguire l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="6b5e8-138">L'app visualizza l'indirizzo di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnessione**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Applicazione Web aperta in Microsoft Edge](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="6b5e8-141">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="6b5e8-141">Deploy the app to Azure</span></span>

<span data-ttu-id="6b5e8-142">Chiudere la pagina Web, tornare a Visual Studio e selezionare **Termina debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-142">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="6b5e8-143">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="6b5e8-145">Nella finestra di dialogo **Pubblica** selezionare **Servizio app di Microsoft Azure** e fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-145">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="6b5e8-147">Assegnare all'app un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-147">Name the app a unique name.</span></span> 

* <span data-ttu-id="6b5e8-148">Selezionare una sottoscrizione MSDN.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-148">Select an MSDN subscription.</span></span>

* <span data-ttu-id="6b5e8-149">Selezionare **Nuovo** per il gruppo di risorse e immettere un nome per il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-149">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="6b5e8-150">Selezionare **Nuovo ** per il piano di servizio app e selezionare una località nelle proprie vicinanze.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-150">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="6b5e8-151">È possibile tenere il nome che viene generato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-151">You can keep the name that is generated by default.</span></span>

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="6b5e8-153">Selezionare la scheda **Servizi** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-153">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="6b5e8-154">Selezionare l'icona verde **+** per creare un nuovo database SQL</span><span class="sxs-lookup"><span data-stu-id="6b5e8-154">Select the green **+** icon to create a new SQL Database</span></span>

![Nuovo database SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="6b5e8-156">Selezionare **Nuovo** nella finestra di dialogo **Configura database SQL** per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-156">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nuovo database SQL e server](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="6b5e8-158">Viene visualizzata la finestra di dialogo **Configura SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-158">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="6b5e8-159">Immettere nome utente e password di amministratore e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-159">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="6b5e8-160">Non dimenticare il nome utente e la password creati in questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-160">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="6b5e8-161">È possibile mantenere il **Nome server** predefinito.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-161">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="6b5e8-162">Immettere un nome per la stringa di connessione e il database.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-162">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="6b5e8-163">La stringa "admin" non è consentita come nome utente di amministratore.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-163">"admin" is not allowed as the administrator user name.</span></span>

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="6b5e8-165">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-165">Select **OK**.</span></span>

<span data-ttu-id="6b5e8-166">Visual Studio torna alla finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="6b5e8-167">Selezionare **Crea** nella finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="6b5e8-169">Fare clic sul collegamento **Impostazioni** della finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-169">Click the **Settings** link in the **Publish** dialog.</span></span>

![Finestra di dialogo Pubblica: pannello di connessione](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="6b5e8-171">Nella pagina **Impostazioni** della finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="6b5e8-171">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="6b5e8-172">Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-172">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="6b5e8-173">Espandere **Migrazioni Entity Framework** e selezionare **Applica questa migrazione in fase di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-173">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="6b5e8-174">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-174">Select **Save**.</span></span> <span data-ttu-id="6b5e8-175">Visual Studio torna alla finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-175">Visual Studio returns to the **Publish** dialog.</span></span> 

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="6b5e8-177">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-177">Click **Publish**.</span></span> <span data-ttu-id="6b5e8-178">Visual Studio pubblicherà l'app in Azure e avvierà l'app cloud nel browser.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-178">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="6b5e8-179">Testare l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="6b5e8-179">Test your app in Azure</span></span>

* <span data-ttu-id="6b5e8-180">Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="6b5e8-180">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="6b5e8-181">Registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="6b5e8-181">Register a new user</span></span>

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="6b5e8-183">Aggiornare l'app</span><span class="sxs-lookup"><span data-stu-id="6b5e8-183">Update the app</span></span>

* <span data-ttu-id="6b5e8-184">Modificare la pagina Razor *Pages/About.cshtml* e modificarne il contenuto.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-184">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="6b5e8-185">Ad esempio, è possibile modificare il paragrafo specificando "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="6b5e8-185">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    <span data-ttu-id="6b5e8-186">[!code-html[Informazioni su](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="6b5e8-186">[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="6b5e8-187">Fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-187">Right-click on the project and select **Publish...** again.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="6b5e8-189">Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verificare che l'attività sia stata completata](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="6b5e8-191">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="6b5e8-191">Clean up</span></span>

<span data-ttu-id="6b5e8-192">Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="6b5e8-193">Selezionare **Gruppi di risorse** e in seguito il gruppo di risorse che è stato creato.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="6b5e8-195">Nella pagina **Gruppi di risorse** selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-195">In the **Resource groups** page, select **Delete**.</span></span>

![Portale di Azure: pagina Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="6b5e8-197">Immettere il nome del gruppo di risorse e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="6b5e8-198">A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure.</span><span class="sxs-lookup"><span data-stu-id="6b5e8-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6b5e8-199">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6b5e8-199">Next steps</span></span>

* [<span data-ttu-id="6b5e8-200">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b5e8-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="6b5e8-201">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b5e8-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="6b5e8-202">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="6b5e8-202">Fundamentals</span></span>](../fundamentals/index.md)
