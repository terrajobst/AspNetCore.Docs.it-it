---
title: Pubblicare un'app ASP.NET Core in Azure con Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="aad07-103">Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aad07-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="aad07-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="aad07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="aad07-105">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="aad07-105">Set up the development environment</span></span>

* <span data-ttu-id="aad07-106">Installare la versione più recente di [Azure SDK per Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span><span class="sxs-lookup"><span data-stu-id="aad07-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="aad07-107">L'SDK installa Visual Studio, se non è già disponibile.</span><span class="sxs-lookup"><span data-stu-id="aad07-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="aad07-108">L'installazione dell'SDK può richiedere più di 30 minuti se il computer non dispone di molte dipendenze.</span><span class="sxs-lookup"><span data-stu-id="aad07-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="aad07-109">Installare gli [strumenti .NET Core e Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)</span><span class="sxs-lookup"><span data-stu-id="aad07-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="aad07-110">Verificare l'[account Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="aad07-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="aad07-111">È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/) o [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="aad07-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="aad07-112">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="aad07-112">Create a web app</span></span>

<span data-ttu-id="aad07-113">Nella pagina iniziale di Visual Studio toccare **Nuovo progetto...**.</span><span class="sxs-lookup"><span data-stu-id="aad07-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![Pagina iniziale](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="aad07-115">In alternativa è possibile usare i menu per creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="aad07-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="aad07-116">Toccare **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="aad07-116">Tap **File > New > Project...**.</span></span>

![File (menu)](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="aad07-118">Completare la finestra di dialogo **Nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="aad07-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="aad07-119">Nel riquadro a sinistra toccare **Web**</span><span class="sxs-lookup"><span data-stu-id="aad07-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="aad07-120">Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="aad07-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="aad07-121">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="aad07-121">Tap **OK**</span></span>

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="aad07-123">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core)**:</span><span class="sxs-lookup"><span data-stu-id="aad07-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="aad07-124">Toccare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="aad07-124">Tap **Web Application**</span></span>

* <span data-ttu-id="aad07-125">Verificare che **Autenticazione** sia impostato su **Account utente individuali**</span><span class="sxs-lookup"><span data-stu-id="aad07-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="aad07-126">Verificare che **Ospita nel cloud** non **sia** selezionata</span><span class="sxs-lookup"><span data-stu-id="aad07-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="aad07-127">Toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="aad07-127">Tap **OK**</span></span>

![Finestra di dialogo Nuova Applicazione Web ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="aad07-129">Eseguire il test dell'app in locale</span><span class="sxs-lookup"><span data-stu-id="aad07-129">Test the app locally</span></span>

* <span data-ttu-id="aad07-130">Premere **Ctrl-F5** per eseguire l'app in locale</span><span class="sxs-lookup"><span data-stu-id="aad07-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="aad07-131">Toccare i collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="aad07-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="aad07-132">A seconda delle dimensioni del dispositivo, può essere necessario toccare l'icona di spostamento per visualizzare i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="aad07-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="aad07-134">Toccare **Registra** e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="aad07-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="aad07-135">È possibile usare un indirizzo di posta elettronica fittizio.</span><span class="sxs-lookup"><span data-stu-id="aad07-135">You can use a fictitious email address.</span></span> <span data-ttu-id="aad07-136">Quando si esegue l'invio, verrà visualizzato un messaggio come il seguente:</span><span class="sxs-lookup"><span data-stu-id="aad07-136">When you submit, you'll get the following error:</span></span>

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="aad07-140">È possibile risolvere il problema in due modi:</span><span class="sxs-lookup"><span data-stu-id="aad07-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="aad07-141">Toccare **Apply Migrations** (Applica migrazioni) e dopo che la pagina è stata aggiornata eseguire il refresh della pagina oppure</span><span class="sxs-lookup"><span data-stu-id="aad07-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="aad07-142">Eseguire il comando seguente da un prompt dei comandi nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="aad07-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="aad07-143">L'app visualizza il messaggio di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnetti**.</span><span class="sxs-lookup"><span data-stu-id="aad07-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![Applicazione Web aperta in Microsoft Edge](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="aad07-146">Distribuire l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="aad07-146">Deploy the app to Azure</span></span>

<span data-ttu-id="aad07-147">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="aad07-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="aad07-148">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="aad07-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="aad07-149">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="aad07-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="aad07-150">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.</span><span class="sxs-lookup"><span data-stu-id="aad07-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="aad07-152">Nella finestra di dialogo **Pubblica** selezionare **Servizio app di Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="aad07-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="aad07-154">Toccare **Nuovo...** per crea un nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="aad07-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="aad07-155">La creazione di un nuovo gruppo di risorse renderà più facile eliminare tutte le risorse di Azure create in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="aad07-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="aad07-157">Creare un nuovo gruppo di risorse e piano del servizio app:</span><span class="sxs-lookup"><span data-stu-id="aad07-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="aad07-158">Toccare **Nuovo...** per il gruppo di risorse e immettere un nome per il nuovo gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="aad07-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="aad07-159">Toccare **Nuovo...**  per il piano di servizio app e selezionare una località nelle proprie vicinanze.</span><span class="sxs-lookup"><span data-stu-id="aad07-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="aad07-160">È possibile mantenere il nome generato predefinito</span><span class="sxs-lookup"><span data-stu-id="aad07-160">You can keep the default generated name</span></span>

* <span data-ttu-id="aad07-161">Toccare **Esplora altri servizi di Azure** per creare un nuovo database</span><span class="sxs-lookup"><span data-stu-id="aad07-161">Tap **Explore additional Azure services** to create a new database</span></span>

![Finestra di dialogo Nuovo gruppo di risorse: pannello Hosting](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="aad07-163">Toccare l'icona  **+**  verde per creare un nuovo database SQL</span><span class="sxs-lookup"><span data-stu-id="aad07-163">Tap the green **+** icon to create a new SQL Database</span></span>

![Finestra di dialogo Nuovo gruppo di risorse: pannello Servizi](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="aad07-165">Toccare **Nuovo...** nella finestra di dialogo **Configura database SQL** per creare un nuovo server di database.</span><span class="sxs-lookup"><span data-stu-id="aad07-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="aad07-167">Immettere nome utente e password di dell'amministratore e quindi toccare **OK**.</span><span class="sxs-lookup"><span data-stu-id="aad07-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="aad07-168">Non dimenticare il nome utente e la password creati in questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="aad07-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="aad07-169">È possibile mantenere il **nome server** predefinito.</span><span class="sxs-lookup"><span data-stu-id="aad07-169">You can keep the default **Server Name**</span></span>

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="aad07-171">La stringa "admin" non è consentita come nome utente di amministratore.</span><span class="sxs-lookup"><span data-stu-id="aad07-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="aad07-172">Toccare **OK** nella finestra di dialogo **Configura database SQL**.</span><span class="sxs-lookup"><span data-stu-id="aad07-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="aad07-174">Toccare **Crea** nella finestra di dialogo **Crea servizio app**.</span><span class="sxs-lookup"><span data-stu-id="aad07-174">Tap **Create** on the **Create App Service** dialog</span></span>

![Finestra di dialogo Crea servizio app](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="aad07-176">Toccare **Successivo** nella finestra di dialogo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="aad07-176">Tap **Next** in the **Publish** dialog</span></span>

![Finestra di dialogo Pubblica: pannello di connessione](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="aad07-178">Nel pannello **Impostazioni** della finestra di dialogo **Pubblica**:</span><span class="sxs-lookup"><span data-stu-id="aad07-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="aad07-179">Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**</span><span class="sxs-lookup"><span data-stu-id="aad07-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="aad07-180">Espandere **Migrazioni Entity Framework** e controllare **Applica questa migrazione in fase di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="aad07-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="aad07-181">Toccare **Pubblica** e attendere fino al completamento dell'app da parte di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aad07-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="aad07-183">Visual Studio pubblicherà l'app in Azure e avvierà l'app cloud nel browser.</span><span class="sxs-lookup"><span data-stu-id="aad07-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="aad07-184">Testare l'app in Azure</span><span class="sxs-lookup"><span data-stu-id="aad07-184">Test your app in Azure</span></span>

* <span data-ttu-id="aad07-185">Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="aad07-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="aad07-186">Registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="aad07-186">Register a new user</span></span>

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="aad07-188">Aggiornare l'app</span><span class="sxs-lookup"><span data-stu-id="aad07-188">Update the app</span></span>

* <span data-ttu-id="aad07-189">Modificare il file di vista Razor `Views/Home/About.cshtml` e cambiarne il contenuto.</span><span class="sxs-lookup"><span data-stu-id="aad07-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="aad07-190">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="aad07-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="aad07-191">Fare clic con il pulsante destro del mouse sul progetto e toccare di nuovo **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="aad07-191">Right-click on the project and tap **Publish...** again</span></span>

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="aad07-193">Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure</span><span class="sxs-lookup"><span data-stu-id="aad07-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="aad07-194">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="aad07-194">Clean up</span></span>

<span data-ttu-id="aad07-195">Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.</span><span class="sxs-lookup"><span data-stu-id="aad07-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="aad07-196">Selezionare **Gruppi di risorse**, quindi toccare il gruppo di risorse che è stato creato</span><span class="sxs-lookup"><span data-stu-id="aad07-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="aad07-198">Nel pannello **Gruppo di risorse** toccare **Elimina**</span><span class="sxs-lookup"><span data-stu-id="aad07-198">In the **Resource group** blade, tap **Delete**</span></span>

![Portale di Azure: pannello Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="aad07-200">Immettere il nome del gruppo di risorse e toccare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="aad07-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="aad07-201">A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure</span><span class="sxs-lookup"><span data-stu-id="aad07-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="aad07-202">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="aad07-202">Next steps</span></span>

* [<span data-ttu-id="aad07-203">Introduzione ad ASP.NET Core MVC e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aad07-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="aad07-204">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aad07-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="aad07-205">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="aad07-205">Fundamentals</span></span>](../fundamentals/index.md)
